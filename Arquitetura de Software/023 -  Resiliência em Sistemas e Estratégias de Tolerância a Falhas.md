## Introdução
A resiliência é um dos aspectos mais cruciais em sistemas modernos e deve ser planejada desde o primeiro dia de desenvolvimento. No entanto, há sempre cenários complexos que podem expor falhas nos planos de resiliência, levando-nos a pensar em como garantir a “resiliência da resiliência.” Este conteúdo levanta provocações e exemplos para refletirmos sobre estratégias para tornar os sistemas robustos frente a falhas imprevistas.

## Desafios de Resiliência: Single Point of Failure
Quando um sistema depende de um único componente, como um message broker (ex.: Kafka, RabbitMQ ou SQS), este pode se tornar um ponto único de falha (single point of failure). Por exemplo:
- **E se o Kafka cair?** O que acontece com as mensagens? O sistema entra em colapso?
- **Como tratar quedas inesperadas de um broker?** Alguns sistemas param de funcionar se não conseguirem se comunicar com o broker, o que pode levar a interrupções e perdas de dados.

### Estratégias de Mitigação
Para lidar com falhas de componentes críticos, podemos considerar:
- **Persistência Local Temporária**: Armazenar temporariamente as mensagens localmente até que o broker esteja disponível novamente.
- **Replicação e Failover**: Configurar clusters de brokers com failover para garantir que, se um falhar, outro assuma suas funções.
- **Monitoramento e Alertas**: Implementar monitoramento rigoroso e alertas para detecção precoce de falhas, permitindo respostas rápidas.

## Escalando a Resiliência: Custo e Complexidade
Aumentar a resiliência de um sistema envolve custos que crescem exponencialmente. Quanto mais camadas de redundância e resiliência são adicionadas, maior o investimento necessário em infraestrutura e expertise:
- **Exemplo de Resiliência em Nuvem**: É comum utilizar múltiplas zonas de disponibilidade (AZs) para evitar falhas de zona. Isso é acessível e amplamente incentivado pelos provedores de nuvem. No entanto, proteger-se contra a falha de uma região inteira implica em maior complexidade e pode envolver replicação inter-regional ou multicloud.
- **Multicloud**: Empresas optam por uma abordagem multicloud não só por custos, mas também para garantir maior resiliência. No entanto, essa estratégia é cara e complexa, exigindo habilidades e ferramentas específicas.

### Decisões Estratégicas e Limites de Resiliência
Nem todas as empresas precisam atingir o mais alto nível de resiliência. Decidir sobre até onde investir em resiliência é uma escolha estratégica:
- **Avaliação de Risco**: Cada incremento na resiliência, como trabalhar com multiregiões ou multicloud, aumenta o custo e a complexidade. A empresa deve avaliar os riscos e definir qual nível de resiliência é necessário para o negócio.
- **Decisão de C-Levels**: Esse nível de decisão é estratégico e cabe aos C-levels (CTO, CEO) que, compreendendo o risco e o custo envolvidos, determinam o nível de resiliência que o sistema deve alcançar.

## Papel dos Desenvolvedores e Decisões Arquiteturais
Embora os desenvolvedores precisem implementar boas práticas como retry e garantir que dados críticos não se percam, a definição dos requisitos de resiliência em níveis elevados é uma responsabilidade dos C-levels. Cabe a eles alinhar o investimento em resiliência com os objetivos de negócio.

## Conclusão
Resiliência, escalabilidade e performance são aspectos interligados da arquitetura de software que exigem decisões cuidadosas e equilibradas. Quanto mais alta a resiliência desejada, maior o custo e o esforço necessário. Com uma compreensão clara dos riscos e objetivos, é possível criar sistemas robustos e capazes de resistir a falhas, mas sempre considerando as restrições de orçamento e complexidade.