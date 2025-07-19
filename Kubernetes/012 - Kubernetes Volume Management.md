Kubernetes abstrai, provisiona e conecta armazenamento a Pods, diferenciando dados efêmeros de dados persistentes, e apresentando **Volumes**, **PersistentVolumes (PV)**, **PersistentVolumeClaims (PVC)**, **StorageClasses** e a padronização via **Container Storage Interface (CSI)**.

## Objetivos

- Explicar a necessidade de gerenciamento de dados persistentes.
    
- Comparar tipos de Volume (efêmeros e persistentes) e quando utilizar cada um.
    
- Descrever o papel de PVs, PVCs e StorageClasses no provisionamento dinâmico.
    
- Entender modos de acesso, modos de volume e políticas de reciclagem.
    

---

## 1. Conceito Geral de Volumes

Containers dentro de Pods são efêmeros: reinícios descartam o _filesystem_ do container (camadas copy-on-write). Um **Volume** fornece um ponto de montagem dentro de um ou mais containers de um Pod. Principais características:

- Associado ao ciclo de vida do **Pod** (para Volumes efêmeros), não ao container isolado → dados sobrevivem a reinícios de containers dentro do mesmo Pod.
    
- O _backend_ (mídia de armazenamento) e semântica (persistência, performance, rede, segurança) dependem do **tipo de Volume**.
    

### Compartilhamento Intra-Pod

Múltiplos containers no mesmo Pod podem montar o mesmo Volume em caminhos distintos (ou idênticos) para compartilhar dados (p.ex. sidecar gerando arquivos estáticos consumidos pelo container principal _web_).

---

## 2. Container Storage Interface (CSI)

Antes do CSI, cada orchestrator possuía plugins específicos (_in-tree_). Problemas: acoplamento ao código-fonte do Kubernetes, ciclos de release lentos e manutenção complexa para vendors.

**CSI** padroniza a interface entre orquestradores e provedores de armazenamento:

- Drivers CSI rodam “out-of-tree” (DaemonSets / sidecars) e expõem operações: **CreateVolume**, **DeleteVolume**, **ControllerPublish/Unpublish**, **NodeStage/Unstage**, **NodePublish/Unpublish** etc.
    
- Facilita adicionar novos backends sem modificar o core do Kubernetes.
    
- Evolução: alfa (v1.9), beta (v1.10‑1.12), estável (v1.13+).
    

### Benefícios

- Independência de release do core.
    
- Suporte a funcionalidades avançadas: snapshots, expansão de volume, clonagem.
    
- Ecossistema amplo (EBS, GCE PD, Azure Disk/File, Ceph, OpenEBS, Longhorn, etc.).
    

---

## 3. Tipos de Volume (Ephemeral & Inline)

A seguir, uma visão funcional (foco em comportamento):

| Tipo                                                 | Persistência Pós-Pod                   | Backend                                                        | Uso Comum                                       | Observações                                     |
| ---------------------------------------------------- | -------------------------------------- | -------------------------------------------------------------- | ----------------------------------------------- | ----------------------------------------------- |
| emptyDir                                             | Não (apaga ao remover Pod)             | Disco do nó (ou memória se `medium: Memory`)                   | Cache temporário, scratch, _workspace_ de build | Se nó falha, dados perdidos.                    |
| hostPath                                             | Sim (enquanto nó existir)              | Diretório do nó                                                | Diagnóstico, agentes, acesso a sockets do host  | Risco de acoplamento e segurança; não portátil. |
| secret                                               | Recriado pela API; conteúdo gerenciado | etcd (codificado/base64)                                       | Injeção de segredos (certs, tokens)             | Montado tmpfs; permissões 0400/0444 típicas.    |
| configMap                                            | Recriado pela API                      | etcd                                                           | Configuração externa, scripts                   | Atualizações podem propagar com atraso (~sync). |
| downwardAPI                                          | Recriado                               | n/a                                                            | Expor metadados do Pod ao container             | Alternativa a variáveis de ambiente.            |
| projected                                            | Recriado                               | Composto (secret, configMap, downwardAPI, serviceAccountToken) | Unificar múltiplas fontes                       | Controla path & permissões.                     |
| persistentVolumeClaim                                | Sim (independente do Pod)              | Backend do PV (rede/local/cloud)                               | Dados de aplicação persistentes                 | Desacopla consumidor do provedor.               |
| nfs / cephfs / iscsi                                 | Dependente do servidor                 | Armazenamento em rede                                          | Compartilhamento RW entre múltiplos Pods        | Latência de rede; tuning necessário.            |
| awsElasticBlockStore / gcePersistentDisk / azureDisk | Sim                                    | Disco em bloco na cloud                                        | Banco de dados, logs                            | Normalmente RWO (um nó por vez).                |
| azureFile / smb / cifs                               | Sim                                    | Share de arquivos                                              | Multi attach RW                                 | Performance menor vs bloco.                     |

> Muitos tipos “legados” migraram para drivers CSI equivalentes; avisos de _deprecated_ tratam migração do plugin _in-tree_ para CSI.

---

## 4. PersistentVolumes (PV)

Um **PersistentVolume** é um objeto de cluster que representa um “pool” ou unidade de armazenamento pré-provisionada (estático) **ou** criada dinamicamente via **StorageClass**.

### Características

- Escopo: cluster (não pertence a namespace).
    
- Especifica: capacidade (`spec.capacity.storage`), _accessModes_, _reclaimPolicy_, _storageClassName_, _volumeMode_ (filesystem ou block), parâmetros específicos do provedor.
    
- Estado: `Available` → `Bound` → (`Released`/`Failed`) dependendo do ciclo de uso.
    

### Provisionamento

1. **Estático**: Administrador cria PV apontando para recurso existente (ex: EBS já criado).
    
2. **Dinâmico**: PVC sem correspondência aciona provisioner do `StorageClass` para criar o volume sob demanda.
    

### StorageClass

Define _provisioner_ (ex: `ebs.csi.aws.com`), parâmetros (tipo de disco, IOPS), política de expansão, _binding mode_ (imediato ou _WaitForFirstConsumer_), e política de volume (retention).

`volumeBindingMode: WaitForFirstConsumer` garante que o agendamento do Pod influencie a topologia (zona/affinity) antes de realmente alocar o volume.

---

## 5. PersistentVolumeClaims (PVC)

Um **PVC** é o pedido de armazenamento do usuário (namespace scope):

- Especifica: `resources.requests.storage`, `accessModes`, opcional `storageClassName`, `volumeMode`.
    
- Matching: Control loop busca PV compatível (capacidade ≥ solicitada, modos compatíveis, classe correspondente). Ao encontrar, PV ↔ PVC ficam _Bound_ de forma exclusiva (exceto backends que suportam multiattach com ReadOnlyMany / ReadWriteMany).
    

### Access Modes

|                            |                                                           |                                      |
| -------------------------- | --------------------------------------------------------- | ------------------------------------ |
| Modo                       | Significado                                               | Casos típicos                        |
| ReadWriteOnce (RWO)        | Leitura/escrita por **um** nó (pode vários Pods nesse nó) | EBS, Disk local, DB single-node      |
| ReadOnlyMany (ROX)         | Somente leitura por vários nós                            | Distribuição de libs, mídia estática |
| ReadWriteMany (RWX)        | Leitura/escrita por vários nós                            | NFS, CephFS, AzureFile, EFS          |
| ReadWriteOncePod (RWO-Pod) | Exclusivo a um único Pod (≥1.22+)                         | Isolamento forte de dados            |

### Volume Mode

- **Filesystem**: (default) formata/monta dentro do container em um diretório.
    
- **Block**: expõe _raw block device_ (útil para bancos que gerenciam filesystem internamente, p. ex. Oracle ASM, ou para alta performance sem overhead de FS).
    

### Reclaim Policy (PV)

|   |   |   |
|---|---|---|
|Política|Ação ao liberar PVC|Uso|
|Retain|PV vira _Released_ (dados preservados) → admin intervém|Auditoria, forense, backup manual|
|Delete|Volume físico destruído|Ambientes efêmeros, CI|
|Recycle (LEGACY)|Limpa (rm -rf) e volta a _Available_|Desencorajado; substituído por dynamic provisioning|

---

## 6. Ciclo de Vida PV/PVC (Dinâmico)

1. Usuário cria PVC.
    
2. Controlador observa ausência de PV compatível → aciona provisioner do StorageClass.
    
3. Provisioner cria volume (ex: API da cloud) e PV correspondente.
    
4. PV _Bound_ ao PVC.
    
5. Pod referencia PVC em `spec.volumes[].persistentVolumeClaim.claimName`.
    
6. Kubelet monta volume no container (`volumeMode=Filesystem` → path; `Block` → device).
    
7. Ao excluir o PVC: aplica-se `reclaimPolicy`.
    

---

## 7. Exemplo Integrado (PVC + Deployment)

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: data-nginx
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: gp2
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-pv
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-pv
  template:
    metadata:
      labels:
        app: nginx-pv
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        volumeMounts:
        - name: web-content
          mountPath: /usr/share/nginx/html
      volumes:
      - name: web-content
        persistentVolumeClaim:
          claimName: data-nginx
```

---

## 8. hostPath (Demonstração Compartilhada)

Exemplo adaptado do guia: compartilhar diretório do host entre **nginx** e container utilitário _debian_ escrevendo um `index.html`.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blue-app
  labels:
    app: blue-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: blue-app
  template:
    metadata:
      labels:
        app: blue-app
        type: canary
    spec:
      volumes:
      - name: host-volume
        hostPath:
          path: /home/docker/blue-shared-volume
          type: DirectoryOrCreate
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
        volumeMounts:
        - name: host-volume
          mountPath: /usr/share/nginx/html
      - name: debian
        image: debian
        command: ["/bin/sh", "-c", "echo 'Welcome to BLUE App!' > /host-vol/index.html; sleep infinity"]
        volumeMounts:
        - name: host-volume
          mountPath: /host-vol
```

> **Atenção:** `hostPath` é específico do nó. Se o Pod for re-agendado em outro nó, dados não acompanham.

---

## 9. Boas Práticas

1. **Preferir StorageClasses** + PVC dinâmico para portabilidade e automação.
    
2. **Evitar hostPath** em produção (exceto DaemonSets utilitários / agentes).
    
3. **Gerenciar segredos via Secret Volume** e _config_ via ConfigMap — não embutir em imagem.
    
4. **Definir requests/limits I/O** (quando suportado por classes avançadas) para isolar workloads.
    
5. **Backups & Snapshots**: usar capacidades de CSI (VolumeSnapshot CRDs) ou ferramentas (Velero).
    
6. **Expansão de Volume**: garantir que `allowVolumeExpansion: true` na StorageClass e atualizar PVC.
    
7. **Controle de Acesso**: aplicar RBAC para restringir quem pode criar/alterar StorageClasses e PVCs.
    
8. **Topologia**: usar `WaitForFirstConsumer` para garantir alocação compatível com zone/region.
    
9. **Monitoramento**: coletar métricas (latência, IOPS, throughput, erro de attach) via plugins de observabilidade.
    
10. **Segurança**: montar volumes como `readOnly` quando possível; considerar criptografia (em repouso + em trânsito — ex: NFS/TLS, EBS encryption).
    

---

## 10. Troubleshooting Essencial

|   |   |   |
|---|---|---|
|Sintoma|Possível Causa|Ação|
|PVC Pending|Sem PV compatível / StorageClass incorreta|Verificar `kubectl describe pvc`; conferir `storageClassName` e provisioner.|
|Pod Pending (volume attaching)|Volume em outra zona / limite de attach do nó|Checar eventos do Pod e topologia.|
|Montagem falha|Permissões / modo de acesso incompatível|`kubectl logs -n kube-system <csi-driver>` e eventos do Pod.|
|Desmontagem lenta|Processo usando descriptor aberto|Garantir que app libera arquivos antes de terminar.|
|Expansão não reflete no FS|Redimensionamento do filesystem não executado|Verificar suporte do driver; pode exigir reinício do Pod.|

---

## 11. Resumo

- Volumes efêmeros resolvem persistência intra-Pod entre reinícios de containers, mas não sobrevivem à remoção do Pod.
    
- Persistência real exige PV + PVC (estático ou dinâmico via StorageClass / CSI).
    
- Modos de acesso e volume influenciam escalabilidade e arquitetura da aplicação.
    
- Políticas de _reclaim_ definem fluxo de ciclo de vida pós-liberação.
    
- Boas práticas e observabilidade são cruciais para performance e confiabilidade.