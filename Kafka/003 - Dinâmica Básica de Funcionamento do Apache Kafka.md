Antes abordamos os  "superpoderes" do Kafka e as empresas que já adotaram essa tecnologia. Agora vamos nos aprofundar um pouco mais na dinâmica básica e na arquitetura interna que faz o Kafka funcionar de forma tão eficiente.

### Produtores e Consumidores: Conceitos Fundamentais

Para entender o Kafka, primeiro precisamos compreender dois conceitos centrais:

- **Producer (Produtor)**: É o componente ou sistema responsável por gerar e enviar eventos para o Kafka. Por exemplo, em um e-commerce, ao realizar um checkout, o sistema de pagamento gera um evento indicando que uma compra foi realizada.
    
- **Consumer (Consumidor)**: É o componente ou sistema que consome esses eventos, ou seja, que lê as mensagens armazenadas no Kafka. Seguindo nosso exemplo, o sistema de emissão de notas fiscais é o consumidor, que lê os eventos referentes às compras para emitir notas fiscais.
    

### Kafka como Intermediário

Diferente de sistemas tradicionais que têm comunicação direta (ponto-a-ponto), Kafka atua como intermediário. Ele não envia ativamente mensagens aos consumidores; em vez disso, os consumidores acessam o Kafka e leem as mensagens que estão armazenadas.

Esse modelo traz grandes vantagens:

- **Desacoplamento**: Produtores e consumidores não precisam estar disponíveis ao mesmo tempo, o que melhora muito a resiliência e flexibilidade do sistema.
    
- **Escalabilidade**: Novos consumidores podem ser adicionados sem interferir nos produtores e vice-versa.
    

### Brokers: O Coração do Kafka

Kafka não é uma única máquina ou serviço; é composto por um cluster formado por várias máquinas chamadas de **brokers**. Cada broker é responsável por:

- Receber e armazenar eventos.
    
- Disponibilizar esses eventos para os consumidores.
    

Cada broker possui seu próprio armazenamento interno (uma espécie de banco de dados otimizado para eventos). Quando um producer envia uma mensagem ao Kafka, ela pode ser armazenada em qualquer broker do cluster, garantindo alta disponibilidade e tolerância a falhas.

### Comunicação dentro do Cluster

Um cluster Kafka típico em produção deve ter no mínimo **3 brokers**, garantindo alta disponibilidade e resiliência. Mas como esses brokers sabem quais mensagens armazenar, qual máquina está online, ou como distribuir a carga?

Historicamente, o Kafka utiliza um sistema chamado **Zookeeper**, um serviço da Apache Foundation que atua principalmente como sistema de gerenciamento e descoberta de serviços (service discovery). O Zookeeper ajuda a gerenciar:

- Descoberta e coordenação dos brokers.
    
- Distribuição de carga.
    
- Monitoramento do status dos nós (brokers).
    

Porém, o Kafka está evoluindo para uma arquitetura sem Zookeeper (**ZooKeeperless Kafka** ou **KRaft**), permitindo que o Kafka faça essa coordenação internamente. Por isso, embora você ainda veja o Zookeeper em muitos ambientes atuais, saiba que ele está sendo gradualmente removido da arquitetura padrão do Kafka.

### O que você precisa lembrar?

- Kafka é uma solução intermediária altamente eficiente para comunicação assíncrona entre sistemas distribuídos.
    
- Produtores geram eventos, e consumidores acessam esses eventos diretamente no Kafka.
    
- O Kafka armazena eventos em um cluster distribuído de brokers, garantindo segurança e performance.
    
- Zookeeper é o atual gerenciador dos brokers, mas está sendo progressivamente substituído pela arquitetura interna própria do Kafka.