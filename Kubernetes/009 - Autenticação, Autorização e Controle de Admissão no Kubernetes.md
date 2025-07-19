Cada solicitação enviada ao servidor de API do Kubernetes passa por uma série de estágios de controle antes de ser aceita e executada. Neste capítulo, vamos explorar detalhadamente as etapas de Autenticação, Autorização e Controle de Admissão.

## Objetivos

- Discutir as etapas de autenticação, autorização e controle de acesso da API do Kubernetes.
    
- Compreender os diferentes tipos de usuários no Kubernetes.
    
- Explorar diferentes módulos para autenticação e autorização.
    

## Visão Geral

Para gerenciar recursos do Kubernetes, é necessário acessar um endpoint específico da API. Cada requisição passa por três etapas fundamentais:

1. **Autenticação**: valida a identidade do usuário com base em credenciais.
    
2. **Autorização**: verifica se o usuário autenticado tem permissão para realizar a operação solicitada.
    
3. **Controle de Admissão**: módulos de software que validam ou modificam as requisições recebidas.
    

## Autenticação

O Kubernetes não possui um objeto específico para usuários nem armazena detalhes sobre eles diretamente, mas ainda assim utiliza nomes de usuário durante a autenticação e no registro das solicitações.

Existem dois tipos principais de usuários no Kubernetes:

### Usuários Normais

Gerenciados fora do cluster, utilizando serviços externos como:

- Certificados de usuários/clientes (X509).
    
- Arquivos estáticos com nome de usuário e senha.
    
- Contas de Google ou outros provedores OAuth.
    

### Contas de Serviço

- Utilizadas por processos dentro do cluster para se comunicarem com o servidor de API.
    
- Geralmente são criadas automaticamente pelo servidor de API, mas também podem ser geradas manualmente.
    
- Associadas a namespaces específicos.
    

### Métodos de Autenticação Suportados:

- **Certificados X509**: configurados com a opção `--client-ca-file`.
    
- **Token Estático**: configurados com a opção `--token-auth-file`.
    
- **Bootstrap Tokens**: usados na inicialização de clusters Kubernetes.
    
- **Tokens de Contas de Serviço**: utilizados automaticamente para autenticação de Pods.
    
- **OpenID Connect (OIDC)**: integração com OAuth2 através de provedores externos.
    
- **Autenticação via Webhook**: autenticação delegada para serviços externos.
    
- **Proxy de Autenticação**: permite lógica adicional de autenticação programável.
    

Recomenda-se utilizar ao menos dois métodos de autenticação, sendo obrigatória a autenticação por tokens das contas de serviço e um outro método de autenticação do usuário.

## Autorização

Após autenticação bem-sucedida, as requisições são avaliadas por módulos de autorização com base em atributos como usuário, grupo, recurso, namespace e API group.

### Módulos de Autorização Suportados:

- **Node Authorization**: específico para requisições do kubelet, permitindo operações restritas a recursos como nodes, pods e eventos.
    
- **Attribute-Based Access Control (ABAC)**: controle baseado em políticas e atributos específicos. Exemplo:
    

```
{
  "apiVersion": "abac.authorization.kubernetes.io/v1beta1",
  "kind": "Policy",
  "spec": {
    "user": "bob",
    "namespace": "lfs158",
    "resource": "pods",
    "readonly": true
  }
}
```

- **Webhook Authorization**: autorização delegada para serviços externos via configuração com `--authorization-webhook-config-file`.
    
- **Role-Based Access Control (RBAC)**: controle baseado em funções (roles) atribuídas a usuários.
    
    - **Role**: restrita a um namespace específico.
        
    - **ClusterRole**: permissões aplicáveis a todo o cluster.
        

Exemplo de Role e RoleBinding:

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: lfs158
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]

---

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-read-access
  namespace: lfs158
subjects:
- kind: User
  name: bob
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

## Controle de Admissão

Após autenticação e autorização, os controles de admissão são aplicados às solicitações, permitindo validação e modificação granular das requisições.

### Tipos de Controles de Admissão:

- **Validadores**: apenas validam, não modificam a requisição.
    
- **Mutantes**: validam e também modificam a requisição.
    

### Exemplos de Controles de Admissão:

- **LimitRanger**: limita recursos nos namespaces.
    
- **ResourceQuota**: aplica cotas de recursos nos namespaces.
    
- **DefaultStorageClass**: define uma classe padrão de armazenamento.
    
- **AlwaysPullImages**: obriga o pull das imagens sempre ao criar um Pod.
    

Os controladores são ativados com a opção:

```
--enable-admission-plugins=NamespaceLifecycle,ResourceQuota,PodSecurity,DefaultStorageClass
```

Também é possível implementar controles personalizados através de webhooks de admissão dinâmicos.

---

Com isso, o Kubernetes consegue fornecer uma gestão robusta e segura dos acessos aos seus recursos, garantindo que apenas usuários autorizados e validados possam executar operações dentro do cluster.