Objetivo:

- Discutir opções de configuração do Kubernetes.
    
- Avaliar considerações de infraestrutura antes da instalação do Kubernetes.
    
- Discutir escolhas de infraestrutura para implantar um cluster Kubernetes.
    
- Revisar ferramentas de instalação e soluções certificadas para Kubernetes.
    

## Configuração do Kubernetes

Kubernetes pode ser instalado usando diferentes configurações de cluster:

### Instalação Single-Node (All-in-One)

- Todos os componentes de control plane e worker estão em um único node.
    
- Indicado apenas para aprendizado e testes, não recomendado para produção.
    

### Single-Control Plane e Multi-Worker

- Um único node de control plane com etcd embutido.
    
- Vários nodes workers gerenciados por esse único control plane.
    

### Single-Control Plane com etcd externo e Multi-Worker

- Node de control plane único com instância externa do etcd.
    
- Gerencia vários worker nodes.
    

### Multi-Control Plane com Multi-Worker (HA)

- Vários nodes de control plane configurados para alta disponibilidade (HA).
    
- Cada control plane possui etcd embutido em configuração HA.
    
- Gerencia múltiplos worker nodes.
    

### Multi-Control Plane com Multi-Node etcd Externo (HA)

- Vários nodes de control plane configurados em HA, cada um com instâncias externas do etcd.
    
- Solução mais avançada e recomendada para ambientes de produção.
    

Conforme aumenta a complexidade do cluster Kubernetes, os requisitos de hardware e recursos também aumentam.

## Infraestrutura para Instalação Kubernetes

Antes da instalação, é preciso considerar:

- Ambiente: bare metal, cloud pública, privada ou híbrida.
    
- Sistema Operacional: Linux (Red Hat, Debian) ou Windows.
    
- Solução de rede (CNI).
    

Consulte a documentação oficial do Kubernetes para escolher as soluções corretas.

## Instalação de Clusters para Aprendizado

Ferramentas populares para instalação local:

- **Minikube**: single ou multi-node para ambientes de aprendizado.
    
- **Kind**: multi-node Kubernetes em containers Docker.
    
- **Docker Desktop**: Kubernetes integrado para usuários Docker.
    
- **Podman Desktop**: Kubernetes integrado para usuários Podman.
    
- **MicroK8s**: clusters locais e em cloud para desenvolvedores.
    
- **K3S**: Kubernetes leve para edge, IoT e cloud.
    

## Instalação de Clusters em Produção

Ferramentas recomendadas para ambientes de produção:

- **kubeadm**: cluster HA seguro para ambientes locais e cloud.
    
- **kubespray**: clusters HA usando Ansible para AWS, GCP, Azure, OpenStack, entre outros.
    
- **kops**: criação e manutenção de clusters HA na AWS e GCE.
    

Para instalação manual detalhada, consulte o projeto **Kubernetes The Hard Way** por Kelsey Hightower.

## Soluções Certificadas para Produção

Provedores gerenciados oferecem soluções certificadas, incluindo:

### Soluções Hospedadas

- Alibaba Cloud Container Service (ACK)
    
- Amazon Elastic Kubernetes Service (EKS)
    
- Azure Kubernetes Service (AKS)
    
- DigitalOcean Kubernetes (DOKS)
    
- Google Kubernetes Engine (GKE)
    
- IBM Cloud Kubernetes Service
    
- Oracle Container Engine for Kubernetes (OKE)
    
- Red Hat OpenShift
    
- VMware Tanzu Kubernetes Grid
    

### Parceiros Certificados

- Aqua Security
    
- Canonical
    
- D2IQ
    
- Dell Technologies Consulting
    
- Deloitte
    
- Fujitsu
    
- GitLab
    
- HPE
    
- Kubermatic
    
- Kublr
    
- Mirantis
    
- Platform9
    
- SAP
    
- SUSE
    
- Sysdig
    
- Weaveworks
    

### Soluções "Turnkey" na Cloud

- Linode Kubernetes Engine
    
- Nirmata Managed Kubernetes
    
- Nutanix Karbon
    
- Vultr Kubernetes Engine
    

## Kubernetes no Windows

Desde a versão 1.14, o Kubernetes oferece suporte para nodes workers Windows (Windows Server 2019 e 2022), permitindo clusters híbridos (Linux e Windows). Nodes control plane ainda são limitados ao Linux.