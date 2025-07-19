**ConfigMaps** e **Secrets** permitem externalizar configuração e dados sensíveis das imagens de container, promovendo reutilização, segurança operacional e flexibilidade de deploy.

## Visão Geral

Ao implantar múltiplas aplicações ou variações (ex.: white‑label para vários clientes), repetir imagens apenas para mudar textos ou parâmetros é ineficiente. **ConfigMaps** resolvem isso fornecendo pares _chave→valor_ não sensíveis. Quando a informação é confidencial (senhas, tokens, certificados, chaves), usamos **Secrets**. Ambos podem ser injetados em Pods como variáveis de ambiente, argumentos/comandos ou arquivos montados via Volume.

## Objetivos

- Desacoplar configuração das imagens usando ConfigMaps.
    
- Entregar dados sensíveis com Secrets de forma controlada.
    
- Comparar modos de criação (imperativo vs declarativo vs arquivos / diretórios).
    
- Consumir ConfigMaps e Secrets em variáveis de ambiente e Volumes.
    

---

## 1. ConfigMaps

**ConfigMap** é um objeto (`kind: ConfigMap`) que armazena dados de configuração em texto simples. Não há limitação rígida de tamanho pequena (mas recomenda-se manter abaixo de alguns MB) e não é pensado para grandes blobs binários.

### 1.1 Criação (Imperativa – valores literais)

```
kubectl create configmap my-config \
  --from-literal=key1=value1 \
  --from-literal=key2=value2
```

Inspecionar:

```
kubectl get configmap my-config -o yaml
```

`data:` conterá as chaves.

### 1.2 Criação (Declarativa – manifesto)

`customer1-configmap.yaml`:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: customer1
data:
  TEXT1: Customer1_Company
  TEXT2: Welcomes You
  COMPANY: Customer1 Company Technology Pct. Ltd.
```

Aplicar:

```
kubectl create -f customer1-configmap.yaml
```

### 1.3 Criação a partir de Arquivo(s)

Arquivo `permission-reset.properties`:

```
permission=read-only
allowed="true"
resetCount=3
```

Criar ConfigMap:

```
kubectl create configmap permission-config \
  --from-file=permission-reset.properties
```

Cada arquivo vira uma chave; conteúdo vira o valor.

### 1.4 Fontes Suportadas

- `--from-literal`
    
- `--from-file=<path>` (um arquivo → uma chave com nome do arquivo)
    
- `--from-file=dir/` (todos os arquivos do diretório)
    
- `--from-env-file=vars.env` (arquivo estilo `.env`)
    

### 1.5 Consumo em Pods

#### Como Variáveis de Ambiente (todas as chaves)

```
containers:
- name: myapp-full
  image: myapp
  envFrom:
  - configMapRef:
      name: full-config-map
```

#### Como Variáveis de Ambiente (chaves específicas)

```
containers:
- name: myapp-specific
  image: myapp
  env:
  - name: SPECIFIC_ENV_VAR1
    valueFrom:
      configMapKeyRef:
        name: config-map-1
        key: SPECIFIC_DATA
  - name: SPECIFIC_ENV_VAR2
    valueFrom:
      configMapKeyRef:
        name: config-map-2
        key: SPECIFIC_INFO
```

#### Como Volume (arquivos)

```
volumes:
- name: config-volume
  configMap:
    name: vol-config-map
containers:
- name: myapp-vol
  image: myapp
  volumeMounts:
  - name: config-volume
    mountPath: /etc/config
```

Cada chave → arquivo dentro de `/etc/config`.

### 1.6 Atualização e Propagação

Atualizar dados (`kubectl apply` / `kubectl create --dry-run -o yaml | kubectl apply`). Containers já em execução **não** recebem automaticamente novo conteúdo de variáveis de ambiente (necessário restart/rollout). Para Volumes de ConfigMap, kubelet atualiza arquivos montados (atraso típico até ~1 minuto). Aplicações devem re‑carregar config dinamicamente se desejado.

### 1.7 Boas Práticas

- Separar ConfigMaps por domínio funcional (evita “mega” ConfigMap).
    
- Usar sufixos versionados se desejar rollbacks simples (ex.: `app-config-v2`).
    
- Validar presença de variáveis obrigatórias no startup da aplicação.
    

---

## 2. Secrets

**Secret** armazena dados sensíveis em base64 (apenas codificação, não criptografia). Tipos comuns: `Opaque` (genérico), `kubernetes.io/dockerconfigjson` (pull secret), `kubernetes.io/tls` (certificados), etc.

> **Segurança:** Proteger acesso ao API Server e etcd. Para criptografia em repouso, habilitar _Encryption at Rest_ no servidor de API (config `EncryptionConfiguration`). Controlar RBAC para impedir listagem ampla de secrets.

### 2.1 Criação (Imperativa – literal)

```
kubectl create secret generic my-password \
  --from-literal=password=mysqlpassword
```

Listar:

```
kubectl get secret my-password
kubectl describe secret my-password
```

`describe` mostra tamanhos, não os valores.

### 2.2 Criação (Manifesto – data vs stringData)

`data:` requer valores já base64:

```
echo -n 'mysqlpassword' | base64  # bXlzcWxwYXNzd29yZA==
```

Manifesto:

```
apiVersion: v1
kind: Secret
metadata:
  name: my-password
type: Opaque
data:
  password: bXlzcWxwYXNzd29yZA==
```

`stringData:` aceita texto plano (kubectl converte):

```
apiVersion: v1
kind: Secret
metadata:
  name: my-password
type: Opaque
stringData:
  password: mysqlpassword
```

Aplicar:

```
kubectl create -f mypass.yaml
```

### 2.3 Criação a partir de Arquivo(s)

```
echo -n 'mysqlpassword' > password.txt
kubectl create secret generic my-file-password \
  --from-file=password.txt
```

Nome da chave vira o nome do arquivo (`password.txt`). Use `--from-file=password=password.txt` para renomear a chave.

### 2.4 Consumo em Pods

#### Variável de Ambiente (chave única)

```
containers:
- name: wordpress
  image: wordpress:4.7.3-apache
  env:
  - name: WORDPRESS_DB_PASSWORD
    valueFrom:
      secretKeyRef:
        name: my-password
        key: password
```

#### Volume (arquivos)

```
volumes:
- name: secret-volume
  secret:
    secretName: my-password
containers:
- name: wordpress
  image: wordpress:4.7.3-apache
  volumeMounts:
  - name: secret-volume
    mountPath: /etc/secret-data
    readOnly: true
```

Cada chave → arquivo contendo o valor decodificado em texto.

### 2.5 Estratégias de Proteção

- **RBAC mínimo**: restringir `get/list/watch` de secrets.
    
- **Namespaces** para segmentação de tenants.
    
- **Encryption at Rest** ativada (AES-CBC, aescbc, secretbox, ou providers cloud).
    
- **Evitar commit em Git**: usar _sealed-secrets_, _SOPS_, _External Secrets Operator_ ou Vault.
    
- **ImagePullSecrets** separados de secrets de runtime.
    
- Rotacionar segredos (gerar novos, atualizar Deployments, revogar antigos).
    

### 2.6 Differences ConfigMap vs Secret

|Aspecto|ConfigMap|Secret (Opaque)|
|---|---|---|
|Uso|Config não sensível|Dados sensíveis|
|Armazenamento etcd|Texto plano|Texto base64 (opcionalmente criptografado)|
|Tamanho recomendado|Pequenos/medianos|Pequenos (senhas, tokens, chaves)|
|Montagem|Env / Volume|Env / Volume (permite `defaultMode`)|
|Atualização Volume|Propagação eventual|Propagação eventual|
|Controle de acesso|Menos restrito|RBAC rigoroso|

---

## 3. Demo: ConfigMap como Conteúdo Web (GREEN App)

Objetivo: armazenar `index.html` em um ConfigMap e montar no container Nginx via Volume para servir conteúdo estático.

1. Criar arquivo `index.html` (conteúdo personalizado).
    
2. Criar ConfigMap:
    

```
kubectl create configmap green-web-cm --from-file=index.html
```

3. Manifesto `web-green-with-cm.yaml` (Deployment + Volume montando ConfigMap):
    

```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: green-web
  name: green-web
spec:
  replicas: 1
  selector:
    matchLabels:
      app: green-web
  template:
    metadata:
      labels:
        app: green-web
    spec:
      volumes:
      - name: web-config
        configMap:
          name: green-web-cm
      containers:
      - image: nginx
        name: nginx
        ports:
        - containerPort: 80
        volumeMounts:
        - mountPath: /usr/share/nginx/html
          name: web-config
```

4. Aplicar:
    

```
kubectl apply -f web-green-with-cm.yaml
```

5. Expor (opcional):
    

```
kubectl expose deployment green-web --type=NodePort --port=80
```

---

## 4. Padrões Avançados

- **Valores hierárquicos**: agrupar prefixos de chave (ex.: `db.host`, `db.port`).
    
- **Config Reload**: sidecar (ex.: `configmap-reload`) observando mudanças em arquivos montados.
    
- **Secret Projection**: usar `items:` para controlar nome de arquivo e _mode_ por chave.
    
- **Immutable ConfigMaps/Secrets** (`immutable: true`): evita mutação acidental e reduz pressão no kube-apiserver.
    
- **External Secrets**: integrar com Vault, AWS Secrets Manager, GCP Secret Manager via CRDs (External Secrets Operator).
    

### Exemplo: Tornando um Secret imutável

```
apiVersion: v1
kind: Secret
metadata:
  name: api-cred
immutable: true
type: Opaque
stringData:
  token: abc123
```

### Exemplo: Selecionar chaves específicas ao montar

```
volumes:
- name: filtered-secret
  secret:
    secretName: my-password
    items:
    - key: password
      path: db.pass
```

`/etc/secret-data/db.pass` conterá só a chave desejada.

---

## 5. Fluxo de Uso Típico

1. Dev cria manifestos de Deployment referenciando nomes de ConfigMaps/Secrets.
    
2. Operações provisiona (ou rotaciona) ConfigMaps/Secrets nos Namespaces apropriados.
    
3. Pipeline CI aplica/atualiza Deployments (rollout). Pods resolvem dependências no start.
    
4. Monitoramento verifica falhas (CrashLoopBackOff) causadas por ausências de chaves.
    
5. Para alterações não sensíveis: atualizar ConfigMap → (se env vars) reiniciar Pods; (se volume) recarregar app.
    
6. Para segredos: criar nova versão, atualizar referências, _rollout restart_, remover versão antiga.
    

---

## 6. Boas Práticas Resumidas

- Mínimo privilégio (RBAC) para leitura de Secrets.
    
- Evitar inline de credenciais em manifests de Deployments.
    
- Usar `immutable: true` quando o ciclo de vida exigir estabilidade.
    
- Automatizar rotação (scripts ou operador).
    
- Validar config em _init containers_ antes de iniciar container principal.
    
- Para multi‑tenant: Namespaces isolam ConfigMaps/Secrets.
    

---

**Conclusão:** ConfigMaps e Secrets fornecem um mecanismo flexível para parametrizar e proteger aplicações no Kubernetes, permitindo que imagens sejam genéricas enquanto a configuração e credenciais variam por ambiente, cliente ou release. A correta distinção entre o que é configuração não sensível e o que é dado sigiloso, aliada a boas práticas de segurança (RBAC, criptografia, rotação), é essencial para operações seguras e escaláveis.