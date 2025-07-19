**Services** do Kubernetes,  são fundamentais para abstrair a comunicação entre microservices internos no cluster ou com o mundo externo, oferecendo um ponto único de acesso através de balanceamento de carga.

## Objetivos

- Entender os benefícios de agrupar Pods logicamente com Services.
    
- Explicar o papel do daemon kube-proxy.
    
- Explorar as opções de descoberta de Services.
    
- Discutir diferentes tipos de Services.
    

## Acesso aos Pods das Aplicações

Pods são recursos efêmeros que possuem IPs dinâmicos. A utilização direta desses IPs gera overhead e riscos de indisponibilidade. Para resolver isso, o Kubernetes introduziu o conceito de **Service**, uma abstração que agrupa Pods logicamente por meio de **Labels** e **Selectors**, permitindo o acesso estável às aplicações.

Exemplo de agrupamento com Deployment:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: frontend
  name: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - image: frontend-application
        name: frontend-application
        ports:
        - containerPort: 5000
```

## Estrutura dos Services

Services agrupam Pods com pares chave-valor usando Labels e Selectors. Cada Service recebe um nome registrado no DNS interno do cluster. Serviços monitoram Pods automaticamente, atualizando dinamicamente endpoints que representam o conjunto IP:porta dos Pods.

Exemplo de definição de Service:

```
apiVersion: v1
kind: Service
metadata:
  name: frontend-svc
spec:
  selector:
    app: frontend
  ports:
  - protocol: TCP
    port: 80
    targetPort: 5000
```

## kube-proxy

**kube-proxy** é um daemon executado em cada nó do cluster que monitora alterações nos Services e endpoints, configurando regras de roteamento (iptables ou ipvs) para direcionar o tráfego aos Pods correspondentes. Este processo ocorre automaticamente sempre que um Service é criado ou removido.

## Políticas de Tráfego

A distribuição do tráfego para Pods é feita pelo kube-proxy usando iptables, sendo o balanceamento padrão aleatório. Existem duas políticas disponíveis:

- **Cluster (padrão)**: Balanceia tráfego entre todos os endpoints disponíveis no cluster.
    
- **Local**: Limita o tráfego aos endpoints locais ao nó que recebe a requisição, podendo rejeitar o tráfego caso não existam Pods locais disponíveis.
    

Exemplo:

```
apiVersion: v1
kind: Service
metadata:
  name: frontend-svc
spec:
  selector:
    app: frontend
  ports:
  - protocol: TCP
    port: 80
    targetPort: 5000
  internalTrafficPolicy: Local
  externalTrafficPolicy: Local
```

## Descoberta de Services

Existem dois métodos principais de descoberta:

### Variáveis de Ambiente

Cada Pod tem automaticamente variáveis com informações sobre Services existentes no momento da criação do Pod.

Exemplo:

```
REDIS_MASTER_SERVICE_HOST=172.17.0.6
REDIS_MASTER_SERVICE_PORT=6379
```

### DNS

Um addon DNS é utilizado para resolução dos nomes dos Services no formato `my-svc.my-namespace.svc.cluster.local`. Este método é o mais recomendado.

## Tipos de Services

### ClusterIP (padrão)

Fornece um IP virtual interno ao cluster. Não acessível externamente.

```
apiVersion: v1
kind: Service
metadata:
  name: frontend-svc
spec:
  selector:
    app: frontend
  ports:
  - protocol: TCP
    port: 80
    targetPort: 5000
  type: ClusterIP
```

### NodePort

Além do ClusterIP, atribui uma porta alta no nó (30000-32767), permitindo acesso externo.

```
apiVersion: v1
kind: Service
metadata:
  name: frontend-svc
spec:
  selector:
    app: frontend
  ports:
  - protocol: TCP
    port: 80
    targetPort: 5000
    nodePort: 32233
  type: NodePort
```

### LoadBalancer

Utiliza o Load Balancer externo da infraestrutura, criando automaticamente ClusterIP e NodePort. Útil em ambientes cloud.

### ExternalIP

Permite mapeamento manual de IPs externos ao Service. A administração do mapeamento é feita externamente ao Kubernetes.

### ExternalName

Não possui Selectors ou endpoints. Serve como CNAME para serviços externos.

## Services Multi-Porta

Um Service pode expor múltiplas portas simultaneamente.

Exemplo:

```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: myapp
  type: NodePort
  ports:
  - name: http
    protocol: TCP
    port: 8080
    targetPort: 80
    nodePort: 31080
  - name: https
    protocol: TCP
    port: 8443
    targetPort: 443
    nodePort: 31443
```

## Encaminhamento de Portas (Port Forwarding)

Permite acessar remotamente Pods ou Services através da máquina local, ideal para testes e depuração:

```
kubectl port-forward svc/frontend-svc 8080:80
kubectl port-forward deploy/frontend 8080:5000
kubectl port-forward pod/frontend-pod 8080:5000
```
