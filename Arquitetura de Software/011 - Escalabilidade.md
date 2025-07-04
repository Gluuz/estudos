# Containers vs. Máquinas Virtuais

## Diferença Fundamental: Container vs. Máquina Virtual

Um container é diferente de uma máquina virtual (VM), e essa diferença é essencial para compreendermos como ambas tecnologias impactam o desempenho e a utilização de recursos.

### O Que É Uma Máquina Virtual (VM)?
Uma máquina virtual executa um sistema operacional completo, incluindo:
- **Kernel próprio**
- **Hypervisor** – Camada de virtualização que gerencia e permite a execução de múltiplas VMs no mesmo hardware.

Essa abordagem resulta em:
- **Alto consumo de recursos** (CPU, memória, etc.)
- **Tempo de inicialização prolongado**
- **Eficiência reduzida** em aplicações que exigem poucos recursos.

**Importante:** Máquinas virtuais são uma revolução na forma como trabalhamos. A maioria dos containers em cloud providers roda sobre VMs.

### Como Funciona a Máquina Virtual?
1. O hypervisor permite instalar diversos sistemas operacionais.
2. Ao criar uma VM, você está criando um novo SO completo.
3. É necessário:
   - Instalar Linux (ou outro SO)
   - Baixar pacotes
   - Alocar recursos (memória, CPU, etc.)
4. Mesmo que uma aplicação consuma 1 MB, o SO pode exigir **4 GB**.
5. Cada VM demora para iniciar e configurar.

### O Que É Um Container?
Um container compartilha o kernel do sistema operacional host, executando apenas processos isolados. Isso torna:
- **Mais leve**
- **Mais rápido**
- **Mais eficiente**

Containers possuem inicialização quase instantânea, semelhante a abrir e fechar um software. Exemplo:
- Aplicação de 1 MB: o container consome cerca de **5 MB**, sem um SO completo.

### Imagens Clássicas de Comparativo (Docker)
**Máquinas Virtuais (VMs):**
- Infraestrutura -> Hypervisor -> VMs (cada VM com aplicação + SO)
- Necessário criar uma VM para cada aplicação.
- Escalar requer criar novas VMs, o que é demorado.

**Containers:**
- Infraestrutura -> SO -> Docker (runtime)
- Docker gerencia aplicações (A, B, C, D, E, F) como processos isolados.
- Permite rodar **milhares de containers** sem SOs duplicados.

### Docker e Containers
O Docker funciona como:
- **Ferramenta completa de containerização**
- **Runtime de containers** (DockerD, daemon que gerencia containers)
- Permite criar aplicações de forma escalável e eficiente.

Containers trazem uma economia significativa de recursos e agilidade na execução e distribuição de aplicações.

---

## Docker e Container Runtimes

Agora que entendemos mais sobre como funciona a ideia de container e a diferença entre uma máquina virtual e um container, vamos falar sobre o Docker e os container runtimes.

### O Que É um Container Runtime?
Container runtime é um software que permite a execução de containers. Ele possui todas as especificações necessárias para que um container possa ser executado. Atualmente, essas especificações são padronizadas.

A Docker, junto com outras empresas, criou a **Open Containers Initiative (OCI)**, que estabelece uma padronização permitindo que qualquer container seja executado de forma consistente, independentemente do runtime utilizado. Por exemplo:
- Podman (alternativa ao Docker) pode executar o mesmo container sem problemas.

Essa padronização é crucial para garantir a interoperabilidade e facilitar o uso de containers em diferentes ambientes.

### Container Runtimes e Docker
Quando falamos de container runtimes, estamos nos referindo à ferramenta que executa containers e segue especificações para garantir compatibilidade. O curso é focado no Docker, então traremos mais informações sobre ele.

**História dos Containers:**
- Em 2008, surgiu o **LXC (Linux Containers)**, que utilizava:
  - **Namespaces** (isolamento de processos, rede, etc.)
  - **cgroups** (controle de recursos como CPU/memória)
- O Docker, criado posteriormente, revolucionou a containerização ao simplificar e automatizar o processo.

### Importante Saber
- Docker não criou o conceito de container, mas aprimorou-o.
- A Open Containers Initiative (​[opencontainers.org](https://opencontainers.org)​) fornece mais informações sobre padronização.


