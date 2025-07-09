Agora, vamos aprofundar nos conceitos fundamentais de tópicos e offsets, que são essenciais para compreender melhor o fluxo e a persistência das mensagens no Kafka.
### O que é um Tópico (Topic)?

No Kafka, um **tópico** é um canal estruturado responsável por armazenar, organizar e disponibilizar as mensagens produzidas pelos sistemas. Cada mensagem enviada ao Kafka obrigatoriamente precisa ser direcionada a um tópico específico.

Embora às vezes comparado às filas de sistemas como RabbitMQ, há uma diferença fundamental:

- **RabbitMQ**: As mensagens enviadas para uma fila são consumidas por um único consumidor (uma mensagem lida e confirmada sai da fila).
    
- **Kafka**: Mensagens enviadas para um tópico podem ser consumidas por múltiplos consumidores diferentes, permitindo que vários sistemas processem as mesmas mensagens simultaneamente.

### Produtores e Consumidores no Kafka

- **Produtor (Producer)**: Envia mensagens para um tópico específico.
    
- **Consumidor (Consumer)**: Lê mensagens armazenadas em um tópico.
    

Exemplo:

- O sistema de checkout (produtor) envia um evento para o tópico de "compras".
    
- Sistemas diversos (consumidores) como emissão de notas fiscais, estoque e logística acessam o mesmo tópico para processar as informações necessárias independentemente.
    

### Tópicos como Logs

Uma analogia útil para entender o funcionamento do Kafka é pensar no tópico como um grande **log**, onde cada nova mensagem é acrescentada ao final desse log em sequência.

Cada mensagem adicionada ao tópico recebe um identificador único sequencial chamado **offset**, que representa a posição exata da mensagem dentro do tópico. Exemplo:

- Offset 0: Mensagem inicial
    
- Offset 1: Segunda mensagem
    
- Offset 2: Terceira mensagem, e assim por diante...
    

### Offset e Consumo Independente

Como os offsets são gerados sequencialmente, consumidores podem ler as mensagens em velocidades diferentes ou em momentos diferentes, sem interferir um no outro:

- Consumidor A pode estar lendo o offset 5 enquanto Consumidor B ainda está no offset 2.
    
- Consumidores podem até mesmo "rebobinar" o tópico, retornando para mensagens antigas caso necessário (útil em situações de reprocessamento por falha ou auditoria).
    

### Armazenamento Persistente em Disco

Diferentemente de algumas tecnologias de filas, que trabalham predominantemente em memória, o Kafka armazena todas as mensagens persistentemente em disco. Essa abordagem garante:

- Persistência das mensagens para longa duração.
    
- Capacidade para múltiplas leituras (replay) das mensagens armazenadas.
    
- Maior segurança e resistência a falhas.
    
## Nomenclatura de Tópicos no Kafka

A escolha dos nomes dos tópicos é fundamental para manter a organização, a padronização e a escalabilidade dos sistemas que utilizam Kafka. Um bom padrão de nomenclatura facilita o entendimento e o uso correto dos tópicos por diferentes times e sistemas.

Uma analogia simples é imaginar o **tópico** como uma pasta em um sistema de arquivos, enquanto os eventos (mensagens) seriam os arquivos dentro dessa pasta. Dessa forma, o nome do tópico deve ser claro, descritivo e consistente.

### Boas práticas para nomear tópicos

- Use nomes que representem o **domínio** e o **conteúdo** do tópico, evitando nomes genéricos ou atrelados a uma aplicação específica.
    
- Defina um **padrão** (convenção) para o nome dos tópicos, como separar partes do nome por ponto (`.`), hífen (`-`) ou underline (`_`), e defina o significado de cada parte.
    
- Evite nomes que possam mudar com frequência, ou que estejam diretamente ligados a detalhes de implementação de um único serviço (a não ser que o tópico seja de uso exclusivamente interno).
    
- Utilize letras minúsculas e seja consistente no formato.
    

### Estrutura sugerida de nomenclatura

Um padrão bastante utilizado segue o modelo:
`<domínio>.<classificação>.<descrição>[.<grupo>]`

**Onde:**

- **Domínio:** Identifica o contexto principal do tópico (ex: `inventory`, `device`, `user`).
    
- **Classificação:** Define o tipo de evento ou finalidade do tópico (ex: `cdc` para change data capture, `fct` para fatos, `cmd` para comandos, `sys` para tópicos de sistema).
    
- **Descrição:** Dá mais detalhes sobre o objeto ou ação relacionada ao evento (ex: `warehouse`, `device`, `checkout`).
    
- **Grupo (opcional):** Pode ser usado para separar diferentes subconjuntos de dados ou usos específicos do mesmo tópico.
    

### Exemplos práticos

- `inventory.cdc.warehouse`: Tópico no domínio de inventário, com eventos de alteração de dados de um armazém.
    
- `device.cdc.device.modbus`: Eventos de alteração em dispositivos que usam o protocolo Modbus.
    
- `access.fct.login`: Fatos de login realizados por usuários.
    
- `system.sys.healthcheck`: Eventos internos de healthcheck de um sistema.
    
### Entendendo Partições (Introdução)

Embora não seja detalhado agora, é importante saber que cada tópico pode ser subdividido em **partições** para facilitar a escalabilidade e distribuição da carga entre múltiplos brokers no cluster Kafka. Cada partição mantém seu próprio log sequencial com offsets independentes.