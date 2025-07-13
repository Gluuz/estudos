 Suas principais funcionalidades e os motivos pelos quais você deveria utilizá-lo. Exploraremos também sua evolução a partir do sistema Borg, criado pelo Google.

Além disso, aprenderemos sobre a Cloud Native Computing Foundation (CNCF), fundação responsável atualmente pelo projeto Kubernetes e outros projetos populares como Argo, Cilium, Prometheus, Fluentd, etcd, CoreDNS, cri-o, containerd, Helm, Envoy, Istio e Linkerd.  

## O que é Kubernetes?

De acordo com o site oficial:

> "Kubernetes é um sistema open-source para automatizar o deployment, a escalabilidade e o gerenciamento de aplicações em containers."

O nome Kubernetes vem do grego "κυβερνήτης" (piloto ou timoneiro). Dessa forma, podemos imaginar o Kubernetes como o piloto em um navio repleto de containers. Kubernetes também é conhecido pela sigla k8s (Kate's), pois existem oito caracteres entre "K" e "s".

Kubernetes é inspirado diretamente no sistema Borg do Google, utilizado há mais de uma década para gerenciar cargas de trabalho globais em containers. O projeto Kubernetes foi lançado inicialmente pelo Google e, em julho de 2015, sua versão 1.0 foi doada para a CNCF, uma sub-fundação da Linux Foundation.

Atualmente, novas versões do Kubernetes são lançadas em ciclos de quatro meses. A versão estável atual é 1.29 (dezembro de 2023).

## De Borg ao Kubernetes

Por mais de uma década, o sistema Borg foi o segredo do Google, rodando centenas de milhares de jobs em milhares de máquinas espalhadas pelo mundo, gerenciando aplicações como Gmail, Drive, Maps e Docs.

Os primeiros desenvolvedores do Kubernetes eram funcionários do Google que tinham experiência direta com o Borg. Diversas funcionalidades do Kubernetes derivam diretamente dessa experiência, incluindo:

- Servidores de API
    
- Pods
    
- IP por Pod
    
- Serviços
    
- Labels
    

## Principais Funcionalidades do Kubernetes

O Kubernetes oferece diversas funcionalidades completas para orquestração de containers:

- **Bin packing automático**: aloca containers automaticamente baseado em recursos.
    
- **Extensibilidade**: permite estender funcionalidades sem alterar código-fonte original.
    
- **Autocura (self-healing)**: substitui e reinicia containers falhos automaticamente.
    
- **Escalabilidade horizontal**: ajusta automaticamente o número de containers.
    
- **Service discovery e balanceamento de carga**: atribui IP e nomes DNS para facilitar comunicação entre containers.
    
- **Atualizações e rollbacks automáticos**: realiza mudanças sem downtime.
    
- **Gerenciamento de configurações e segredos**: mantém dados sensíveis separados das imagens de containers.
    
- **Orquestração de armazenamento**: conecta soluções de armazenamento automaticamente.
    
- **Execução em lote (batch)**: suporta execução de jobs batch e jobs de longa duração.
    
- **Dual-stack IPv4/IPv6**: suporta redes IPv4 e IPv6 simultaneamente.
    

Outras funcionalidades, como controle de acesso baseado em funções (RBAC) e cronjobs, passaram recentemente a status estável em versões específicas.

## Por que usar Kubernetes?

Além de suas funcionalidades técnicas, o Kubernetes é valorizado por:

- **Portabilidade**: pode ser implantado em qualquer ambiente (VMs, bare metal, cloud híbrida e pública).
    
- **Extensibilidade**: permite integração com ferramentas de terceiros, expandindo suas capacidades com recursos adicionais.
    
- **Comunidade ativa**: mais de 3.500 colaboradores e mais de 120.000 commits ao longo do tempo, além de grupos específicos (SIGs) focados em tópicos especializados como redes, armazenamento e escalabilidade.
    

## Usuários de Kubernetes

Desde sua criação, Kubernetes tornou-se a plataforma preferida para empresas de diferentes tamanhos e setores, como:

- BlackRock
    
- Booking.com
    
- Box
    
- CapitalOne
    
- Huawei
    
- IBM
    
- ING
    
- Nokia
    
- OpenAI
    
- Pearson
    
- Spotify
    
- Wikimedia
    

Esses exemplos mostram a versatilidade e robustez do Kubernetes em aplicações reais.

## Cloud Native Computing Foundation (CNCF)

A CNCF, parte da Linux Foundation, promove a adoção de containers, microsserviços e aplicações cloud-native. Projetos CNCF são categorizados por maturidade em Sandbox, Incubating e Graduated.

### Projetos Graduados Populares:

- Kubernetes
    
- Argo
    
- etcd
    
- CoreDNS
    
- containerd
    
- CRI-O
    
- Envoy
    
- Fluentd
    
- Flux
    
- Helm
    
- Linkerd
    
- Open Policy Agent
    
- Prometheus
    
- Rook
    

### Projetos em Incubação:

- Buildpacks.io
    
- Contour
    
- gRPC
    
- CNI
    
- Knative
    
- KubeVirt
    
- Notary
    

A CNCF abrange toda a jornada de aplicações cloud-native, desde execução, monitoramento até segurança.

## CNCF e Kubernetes

A CNCF desempenha um papel crucial para o Kubernetes ao:

- Oferecer uma marca neutra e garantir sua utilização correta.
    
- Realizar auditoria de licenças.
    
- Oferecer consultoria jurídica.
    
- Manter cursos e certificações oficiais (KCNA, CKA, CKAD, CKS).
    
- Gerenciar grupos de conformidade de software.
    
- Apoiar atividades de marketing e eventos para a comunidade Kubernetes.
    

Desta forma, a CNCF contribui diretamente para o crescimento sustentável do Kubernetes e de seu ecossistema.