Exploraremos a arquitetura do Kubernetes, os componentes do control plane, o papel dos worker nodes, o gerenciamento de estado com etcd e os requisitos de configuração da rede. Também estudaremos o Container Network Interface (CNI), especificação de rede do Kubernetes.
## Arquitetura Kubernetes

Kubernetes é um cluster de sistemas computacionais organizados em:

- Um ou mais nodes de control plane.
    
- Um ou mais worker nodes (opcional, mas recomendado).
    

## Visão Geral do Node de Control Plane

O node de control plane gerencia o estado do cluster e é essencial para o seu funcionamento. Para garantir alta disponibilidade, pode-se configurar réplicas do node de control plane.

A persistência do estado é feita em um key-value store distribuído (etcd). Há duas topologias principais:

- **Stacked topology**: etcd roda junto com outros componentes do control plane.
    
- **External topology**: etcd em hosts dedicados (alta disponibilidade exigirá hosts extras).
    

## Componentes do Node de Control Plane

O node do control plane roda:

- API Server
    
- Scheduler
    
- Controller Managers
    
- Key-Value Store (etcd)
    
- Container Runtime
    
- kubelet
    
- kube-proxy
    
- Add-ons opcionais (monitoramento e logging)
    

### API Server

Centraliza a comunicação RESTful, processando solicitações e armazenando o estado no etcd.

### Scheduler

Distribui workloads pelos nodes baseando-se em requisitos como recursos, restrições, afinidades e topologia.

### Controller Managers

Monitoram o estado do cluster e garantem que o estado desejado corresponda ao estado atual:

- **kube-controller-manager**: gerencia falhas de nodes, endpoints, contas de serviço e pods.
    
- **cloud-controller-manager**: interage com provedores cloud para gerenciamento de volumes e balanceamento.
    

### Key-Value Store (etcd)

Persiste o estado do cluster. Apenas o API Server pode acessar o etcd diretamente.

## Visão Geral do Worker Node

Nodes workers executam aplicações clientes encapsuladas em Pods (unidade mínima de execução no Kubernetes).

### Componentes do Worker Node

- Container Runtime
    
- kubelet
    
- kube-proxy
    
- Add-ons (DNS, dashboards, monitoramento, logging, plugins de dispositivos)
    

### Container Runtime

Necessário para gerenciar o ciclo de vida dos containers. Exemplos:

- CRI-O
    
- containerd
    
- Docker Engine
    
- Mirantis Container Runtime
    

### kubelet

Agente que gerencia Pods e containers, monitorando sua saúde e recursos. Usa o Container Runtime Interface (CRI) para se comunicar com o runtime.

### kube-proxy

Gerencia regras de rede, garantindo comunicação entre Pods e clientes externos usando iptables.

## Add-ons

Funcionalidades adicionais não nativas:

- DNS interno
    
- Dashboard web
    
- Monitoramento centralizado
    
- Logging centralizado
    
- Plugins para dispositivos especiais (GPUs, FPGAs)
    

## Desafios de Rede

O Kubernetes enfrenta desafios como comunicação entre containers e Pods, entre Pods em nodes diferentes e acesso externo aos serviços.

### Comunicação Container-Container

Containers em um Pod compartilham o mesmo network namespace e se comunicam via localhost.

### Comunicação Pod-Pod

Cada Pod possui um endereço IP único (modelo IP-per-Pod), permitindo comunicação direta sem NAT. Esta comunicação utiliza plugins CNI como Flannel, Calico e Cilium.

### Comunicação Externa-Pod

Realizada por meio de Services, que definem regras de rede gerenciadas pelo kube-proxy e iptables, permitindo acesso externo via IPs virtuais.

## Conclusão

A arquitetura do Kubernetes garante uma gestão eficiente, tolerante a falhas e escalável de aplicações em containers, resolvendo desafios complexos de orquestração e redes.