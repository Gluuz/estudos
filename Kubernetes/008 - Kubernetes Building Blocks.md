Vamos explorar o modelo de objetos do Kubernetes e descrever alguns dos seus componentes fundamentais, como Nodes, Namespaces, Pods, ReplicaSets, Deployments, DaemonSets, entre outros. Também discutiremos o papel essencial das Labels e Selectors em arquiteturas baseadas em microsserviços, pois agrupam logicamente objetos desacoplados.

## Objetivos de Aprendizado

- Descrever o modelo de objetos do Kubernetes.
    
- Discutir os componentes básicos do Kubernetes, como Nodes, Namespaces, Pods, ReplicaSets, Deployments, DaemonSets.
    
- Discutir Labels e Selectors.
    

## Modelo de Objetos do Kubernetes

O Kubernetes ganhou popularidade devido às suas capacidades avançadas de gerenciamento do ciclo de vida das aplicações, implementadas através de um modelo rico de objetos. Esses objetos representam diferentes entidades persistentes no cluster Kubernetes, descrevendo:

- Quais aplicações conteinerizadas estão sendo executadas.
    
- Os nodes onde essas aplicações estão implantadas.
    
- Consumo de recursos das aplicações.
    
- Políticas associadas às aplicações, como políticas de reinicialização/atualização, tolerância a falhas, acesso, entre outras.
    

Cada objeto tem uma seção de especificação (`spec`) onde declaramos o estado desejado. O Kubernetes gerencia a seção de status (`status`), registrando o estado real do objeto, tentando sempre aproximá-lo ao estado desejado.

## Nodes

Nodes são identidades virtuais atribuídas pelo Kubernetes aos sistemas que compõem o cluster. Cada node é gerenciado por agentes do Kubernetes (`kubelet` e `kube-proxy`) e hospeda um container runtime para executar as cargas de trabalho.

Existem dois tipos principais de nodes:

- **Control plane nodes:** executam agentes essenciais do Kubernetes.
    
- **Worker nodes:** executam aplicações conteinerizadas.
    

## Namespaces

Namespaces são usados para particionar o cluster em subclusters virtuais. Recursos dentro de um namespace possuem nomes únicos, mas nomes podem se repetir entre namespaces diferentes.

Namespaces padrão incluem:

- `kube-system`
    
- `default`
    
- `kube-public`
    
- `kube-node-lease`
    

Novos namespaces podem ser criados para organizar e isolar usuários, equipes e aplicações:

```
kubectl create namespace novo-namespace
```

## Pods

Um Pod é a menor unidade de trabalho no Kubernetes, representando uma instância única da aplicação. Um Pod agrupa um ou mais containers que compartilham o mesmo namespace de rede, volume de armazenamento externo, entre outros recursos.

Exemplo de manifesto YAML para um Pod:

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    run: nginx-pod
spec:
  containers:
  - name: nginx-pod
    image: nginx:1.22.1
    ports:
    - containerPort: 80
```

## Labels

Labels são pares de chave-valor usados para organizar e selecionar grupos de objetos no Kubernetes. Exemplo: `app=my-app`, `env=dev`.

## Label Selectors

Selectors são utilizados por controladores e serviços para selecionar objetos com base em labels. O Kubernetes suporta dois tipos:

- **Equality-Based Selectors:** usam operadores `=` e `!=`.
    
- **Set-Based Selectors:** usam operadores `in`, `notin`, `exists`, e `!exists`.
    

## ReplicaSets

ReplicaSets garantem que um número específico de Pods esteja sempre em execução, oferecendo replicação e recuperação automática.

Exemplo de ReplicaSet:

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
  labels:
    app: guestbook
spec:
  replicas: 3
  selector:
    matchLabels:
      app: guestbook
  template:
    metadata:
      labels:
        app: guestbook
    spec:
      containers:
      - name: php-redis
        image: gcr.io/google_samples/gb-frontend:v3
```

## Deployments

Deployments gerenciam atualizações declarativas e rollbacks de Pods e ReplicaSets, oferecendo estratégias de atualização avançadas como RollingUpdate.

Exemplo de Deployment:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-deployment
  template:
    metadata:
      labels:
        app: nginx-deployment
    spec:
      containers:
      - name: nginx
        image: nginx:1.20.2
        ports:
        - containerPort: 80
```

## DaemonSets

DaemonSets garantem que um Pod específico rode em todos os nodes do cluster (ou em um grupo específico de nodes). São utilizados para agentes como monitoramento, armazenamento, rede, entre outros.

Exemplo de DaemonSet:

```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd-agent
  labels:
    k8s-app: fluentd-agent
spec:
  selector:
    matchLabels:
      k8s-app: fluentd-agent
  template:
    metadata:
      labels:
        k8s-app: fluentd-agent
    spec:
      containers:
      - name: fluentd
        image: quay.io/fluentd_elasticsearch/fluentd:v4.5.2
```

## Services

Services são utilizados para expor portas de aplicações conteinerizadas na rede do Kubernetes, garantindo a descoberta e o balanceamento de carga para Pods com múltiplas réplicas.