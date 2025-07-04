## O Que São Containers?

Um **container** é uma unidade de software que empacota o código e todas as suas dependências, permitindo que o software seja executado de forma rápida, consistente e em qualquer ambiente.

### Principais Características dos Containers:

- **Imutabilidade**: Uma vez que um container é iniciado, ele roda de forma idêntica todas as vezes. Não há alterações ao longo do tempo.
- **Isolamento de Recursos**: Containers isolam processos, rede, memória, CPU e outros recursos, sem afetar a máquina host.
- **Eficiência e Rapidez**: Diferente de máquinas virtuais, containers são processos leves que iniciam e encerram rapidamente, sem necessidade de boot completo.
- **Kernel Compartilhado**: Containers usam o kernel do sistema operacional host, permitindo eficiência e menor consumo de recursos.

### Containers vs Máquinas Virtuais

Containers são mais rápidos que máquinas virtuais porque não precisam de um sistema operacional completo para serem executados. Eles rodam como processos isolados diretamente sobre o kernel do SO host.

### Imagens de Containers

As **imagens de containers** são pacotes contendo tudo o que é necessário para rodar um container. Elas podem ser vistas como snapshots que garantem a consistência do ambiente em qualquer execução.

## Containers e Linux

Containers são fortemente baseados em Linux. No entanto, mesmo sem familiaridade com Linux, é possível utilizá-los com ferramentas que facilitam a instalação do Docker e o uso de containers.

## "Na Minha Máquina Funciona"

Containers eliminam o problema clássico do "na minha máquina funciona". Com eles, aplicações rodam de forma consistente em qualquer ambiente, seja local ou em provedores de nuvem como AWS, Google Cloud e Azure.