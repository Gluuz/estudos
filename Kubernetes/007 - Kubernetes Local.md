Minikube é um dos métodos mais fáceis, flexíveis e populares para executar um cluster Kubernetes local em modo all-in-one ou multi-node. Ele pode rodar diretamente em máquinas locais através de máquinas virtuais (VMs) ou containers. Ideal para aprendizado, Minikube facilita a instalação dos componentes Kubernetes, gerenciamento e desmontagem do cluster. Está disponível para macOS, Windows e diversas distribuições Linux.

## Objetivos

- Entender o Minikube.
    
- Instalar o Minikube no seu sistema operacional nativo.
    
- Verificar a instalação local do Minikube.
    

## O que é Minikube?

Minikube é uma ferramenta que permite criar clusters Kubernetes isolados utilizando máquinas virtuais (hypervisors) ou runtimes de containers. Ele gerencia todo o ciclo de vida do cluster sem interferir na configuração do sistema operacional do host.

### Principais características:

- Simples e fácil de usar.
    
- Flexível, suportando múltiplos hypervisors e runtimes.
    
- Capacidade de criação de clusters single-node ou multi-node.
    
- Suporte a múltiplos perfis personalizados para diferentes cenários.
    

## Requisitos para Executar o Minikube

- Suporte à virtualização habilitado (VT-x/AMD-v).
    
- Cliente `kubectl` instalado (pode ser instalado separadamente).
    
- Hypervisor Tipo-2 ou Container Runtime (VirtualBox, Docker, KVM2, Podman, Hyper-V).
    
- Conexão com a internet para download inicial dos componentes.
    

## Instalação do Minikube no Linux (Ubuntu 22.04 LTS)

### 1. Verificar suporte à virtualização:

```
grep -E --color 'vmx|svm' /proc/cpuinfo
```

### 2. Instalar VirtualBox:

Acesse o [site oficial do VirtualBox](https://www.virtualbox.org/) para download e instalação.

### 3. Instalar Minikube:

```
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
```

### 4. Inicializar o Minikube:

```
minikube start --driver=virtualbox
```

### 5. Verificar status:

```
minikube status
```

### 6. Parar e Remover Minikube:

```
minikube stop
minikube delete
```

## Recursos Avançados do Minikube

Minikube suporta perfis personalizados, permitindo diferentes configurações de cluster:

Exemplo para criar um cluster multi-node com VirtualBox e Calico:

```
minikube start --driver=virtualbox --nodes=3 --disk-size=10g --cpus=2 --memory=6g --kubernetes-version=v1.27.12 --cni=calico --container-runtime=cri-o -p multivbox
```

Listar perfis:

```
minikube profile list
```

Mudar contexto entre perfis:

```
minikube profile minibox
```

## Métodos de Acesso ao Cluster Minikube

- Interface de Linha de Comando (CLI): `kubectl`
    
- Interface Web: Kubernetes Dashboard
    
- Acesso via API usando `curl`
    

### Instalar e configurar `kubectl`

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```

### Kubernetes Dashboard

Habilitar Dashboard e Métricas:

```
minikube addons enable metrics-server
minikube addons enable dashboard
```

Abrir Dashboard:

```
minikube dashboard
```

### Acesso via APIs com `kubectl proxy`

Iniciar proxy:

```
kubectl proxy
```

Acessar endpoints:

```
curl http://localhost:8001/api/v1
```

### Autenticação direta com APIs:

Gerar Token e acessar API:

```
export TOKEN=$(kubectl create token default)
export APISERVER=$(kubectl config view | grep https | cut -f 2- -d ":" | tr -d " ")
curl $APISERVER --header "Authorization: Bearer $TOKEN" --insecure
```

## Conclusão

Minikube é uma poderosa ferramenta de aprendizado para Kubernetes, permitindo a criação rápida de clusters isolados com recursos avançados e fáceis de usar, promovendo um ambiente seguro para testes e aprendizado contínuo.