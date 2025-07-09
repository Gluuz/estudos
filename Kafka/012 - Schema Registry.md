## Introdução
No ecossistema do Kafka, os dados trafegam em forma de mensagens estruturadas de acordo com **esquemas** bem definidos. À medida que múltiplos consumidores passam a ler os dados de um mesmo _topic_, muitas vezes mantidos por times diferentes ou até departamentos distintos, torna-se fundamental que todos entendam e concordem com o formato das mensagens recebidas.

Com o tempo, esses esquemas podem evoluir: campos são adicionados ou removidos, tipos de dados mudam, novas versões são introduzidas. Sem um controle adequado dessas mudanças, alterações de esquema podem **quebrar consumidores** desavisados, causando falhas em aplicações downstream. Para gerenciar a evolução de esquemas de forma transparente e evitar surpresas desagradáveis, surgiu o conceito de **Schema Registry** integrado ao uso do Kafka.

## O que é um Schema Registry?

O Schema Registry é essencialmente um serviço externo dedicado a armazenar e gerenciar os esquemas utilizados pelas mensagens nos tópicos Kafka. **Não se trata de um componente interno do Apache Kafka** em si, ao contrário, ele roda como um processo separado, geralmente fornecido pela Confluent como uma solução de código aberto.

Na prática, o registro de esquemas funciona como um **repositório central de metadados** de esquemas. Para o cluster Kafka, ele se parece apenas com mais um cliente produtor/consumidor de mensagens, porém sua finalidade é específica: manter um banco de dados de todos os esquemas relativos aos tópicos que monitora. Essa base de esquemas é armazenada de forma durável **dentro de um tópico interno do Kafka** (por padrão chamado `_schemas`), garantindo alta disponibilidade e persistência das definições. Em configurações distribuídas, o Schema Registry pode ser replicado em cluster para evitar ponto único de falha, aproveitando a replicação interna do Kafka para confiabilidade dos dados de esquema.

## Como funciona na prática?

Quando um **produtor** vai enviar mensagens para um tópico, ele primeiro interage com o Schema Registry (geralmente via uma chamada REST) para **registrar o esquema** dos dados que pretende enviar. Se esse esquema ainda não existe no registro, o serviço o valida e armazena, retornando um identificador único (ID) para o novo esquema. A partir daí, o produtor inclui esse **ID de esquema** junto a cada mensagem serializada que publica no tópico Kafka. Como o ID é bem menor que a definição completa do esquema, isso evita ter que anexar todo o esquema em cada mensagem, economizando banda e espaço de mensagem.

Do lado do **consumidor**, ao ler uma mensagem do tópico ele detecta o ID do esquema embutido. O consumidor então consulta o Schema Registry (caso ainda não tenha esse esquema em cache local) para obter a definição completa do esquema correspondente àquele ID. Com o esquema em mãos, o consumidor consegue desserializar os dados binários da mensagem de volta para um objeto ou estrutura de dados compreensível, seguindo o formato esperado.

Se por acaso o esquema do dado recebido **não for compatível** com o que o consumidor espera (por exemplo, o produtor enviou uma versão nova do esquema que o consumidor ainda não conhece), duas coisas podem acontecer dependendo da configuração:

- O **consumidor** pode lançar uma exceção/falha ao tentar processar a mensagem, indicando incompatibilidade.
    
- Ou até mesmo o **Schema Registry** pode rejeitar o registro de um novo esquema incompatível com versões anteriores, impedindo o produtor de enviar dados que violem o contrato estabelecido.
    

Essa mediação garante que mudanças de formato não ocorram silenciosamente a ponto de quebrar aplicativos consumidores sem aviso.

## Regras de Compatibilidade de Esquema

Uma das funcionalidades mais importantes do Schema Registry é oferecer **regras de compatibilidade** para evolução segura dos esquemas. Ao definir um esquema em um sujeito (subject), geralmente atrelado a um tópico específico, podemos estabelecer um modo de compatibilidade que determina como novas versões de esquemas podem diferir das versões anteriores. Os modos comuns incluem:

- **Compatibilidade retroativa (_backward compatibility_):** Consumidores _novos_ (usando versão mais recente do esquema) conseguem ler mensagens produzidas com esquemas _antigos_. Exemplo: remover um campo opcional ou adicionar um novo campo com valor padrão.
    
- **Compatibilidade futura (_forward compatibility_):** Consumidores _antigos_ conseguem ler mensagens produzidas com esquemas _novos_. Exemplo: adicionar um campo opcional ou ignorar um campo desconhecido.
    
- **Compatibilidade total (_full_):** Mantém-se simultaneamente a compatibilidade _backward_ e _forward_.
    
- **Sem compatibilidade (_none_):** Nenhuma verificação automática é feita; qualquer alteração de esquema é permitida.
    

Essas regras orientam como realizar migrações de esquema sem interrupções. O Schema Registry valida cada novo esquema submetido contra essas regras (incluindo modos transitivos que checam todas as versões anteriores, se configurado), rejeitando alterações incompatíveis e assim evitando _deploys_ problemáticos.

## Formatos de Serialização Suportados

Como o Schema Registry atua em conjunto com a serialização/desserialização de mensagens, ele oferece suporte nativo aos principais **formatos de dados** usados no Kafka:

- **Avro:** Formato binário compacto e muito popular no ecossistema Kafka. Define esquemas via arquivos `.avsc` (JSON) e oferece validação e controle rígido dos dados.
    
- **JSON Schema:** Esquemas definidos no padrão JSON Schema, com dados trafegando em JSON textual. Muito usado para integrações web por sua legibilidade e flexibilidade.
    
- **Protobuf (Protocol Buffers):** Formato binário eficiente e neutro em linguagem, bastante usado em arquiteturas de microsserviços e gRPC.
    
Ao usar qualquer um desses formatos, os produtores e consumidores utilizam **serializadores/desserializadores** específicos que integram a lógica do Schema Registry. Isso facilita o registro e a busca de esquemas automaticamente durante o processo de (de)serialização.

## Por que o Schema Registry é essencial?

Em sistemas de mensageria _event streaming_ não triviais, o Schema Registry torna-se uma peça **indispensável** para manter a integridade e flexibilidade dos dados. Dentre os benefícios-chave, podemos destacar:

- **Gerenciamento centralizado de esquemas:** Todos compartilham um entendimento comum dos formatos de mensagem.
    
- **Garantia de compatibilidade:** Impede mudanças “quebra-galho” que causariam falhas em consumidores.
    
- **Governança e colaboração entre times:** Permite negociar e revisar mudanças de esquema de forma segura.
    
- **Verificações em tempo de compilação:** Ferramentas permitem validar compatibilidade de esquemas antes do deployment.
    
- **Desempenho otimizado:** O ID do esquema é muito menor do que o esquema completo, reduzindo o tamanho das mensagens.
    

## Schema Registry Open Source vs. Confluent Cloud

Embora não faça parte do núcleo do Apache Kafka, o Schema Registry da Confluent tornou-se a implementação mais popular. Está disponível como parte da plataforma Confluent  e pode ser executado on-premises em clusters Kafka open source.

Existem também **alternativas open source** como o **Apicurio Registry** (Red Hat) e opções gerenciadas em nuvem, como o _AWS Glue Schema Registry_ ou o registro embutido no **Redpanda**. Entretanto, a solução da Confluent é a mais completa em termos de recursos e integração com Kafka.

No **Confluent Cloud** (Kafka como serviço), o Schema Registry é totalmente gerenciado, trazendo funcionalidades extras como:

- **Schema Linking:** Replicação de esquemas entre registros de diferentes clusters/ambientes.
    
- **Contexts de Esquema:** Isolamento lógico de conjuntos de esquemas por domínio ou time.
    
- **Governança avançada:** Controle de acesso, auditoria, catálogo de esquemas e integração com demais componentes enterprise.