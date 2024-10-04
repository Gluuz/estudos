Todo sistema possui essas características, sejam elas bem ou mal implementadas. O ponto central é desenvolver o sistema de forma intencional, planejando as características que serão necessárias para evitar problemas futuros. Ao entender a estrutura e o funcionamento do software, é possível antecipar os desafios e lidar com eles de forma proativa.

**Intencionalidade no desenvolvimento**: Trabalhar com intenção significa estar preparado para resolver problemas específicos, ao invés de contar com a sorte. Por exemplo, a resiliência pode surgir de forma "natural" no sistema devido à infraestrutura ou bibliotecas utilizadas, mas isso não garante que o sistema seja verdadeiramente resiliente. Quando a resiliência é pensada de forma explícita, o sistema é projetado para enfrentar crises de maneira eficiente e planejada.

**Requisitos não funcionais**: Muitas características arquiteturais são requisitos não funcionais, ou seja, não estão diretamente ligados às regras de negócio, mas são essenciais para o funcionamento do sistema, como a capacidade de suportar carga, manter a disponibilidade e garantir a confiabilidade.

Seguindo os pontos do livro _"Fundamentos de Arquitetura de Software"_, podemos dividir essas características em três áreas principais:

1. **Operacionais**: Relacionadas ao funcionamento do software.
2. **Estruturais**: Ligadas à estrutura interna do software.
3. **Cross-cutting**: Características que permeiam o sistema como um todo.

### Operacionais

**Disponibilidade**: Um sistema deve estar disponível de acordo com o SLA (acordo com o cliente) e SLO (objetivo interno). A observabilidade é crucial para monitorar a disponibilidade, assim como as técnicas de SRE que ajudam a calcular o tempo máximo de indisponibilidade permitido.

**Recuperação de desastres**: Planejar e testar processos de recuperação é fundamental, especialmente para sistemas críticos. A capacidade de trabalhar em multi-regiões ou multi-cloud deve ser considerada em cenários onde a disponibilidade é crucial.

**Performance**: Envolve dois fatores principais: latência e throughput (quantidade de requisições que o sistema suporta). A arquitetura deve ser planejada de acordo com a carga esperada (por exemplo, 50 vs. 5000 requisições por segundo), podendo exigir soluções mais robustas como bancos de dados específicos ou padrões de design como CQRS.

**Backup**: Além de implementar backups, é essencial testá-los regularmente. O uso de nuvem não elimina a necessidade de armazenar backups em redes separadas, especialmente para evitar ataques como ransomware.

**Confiabilidade e segurança**: Sistemas de missão crítica exigem uma abordagem cuidadosa em relação à segurança, com a implementação de soluções como captchas para prevenir ataques de robôs ou tentativas de brute force. A robustez do sistema deve garantir que ele possa escalar conforme a demanda e continuar operando mesmo sob ataque ou pressão.

**Robustez**: A estrutura do sistema deve ser forte o suficiente para manter operações, mesmo em eventos como falhas de regiões inteiras da AWS. Devemos considerar a capacidade da infraestrutura ao lidar com falhas de datacenters ou de zonas de disponibilidade.

**Escalabilidade**: Um sistema escalável permite aumentar seus recursos de duas formas: verticalmente (mais poder computacional em uma máquina) ou horizontalmente (adicionando mais máquinas). Seguir princípios como os 12 fatores do Heroku ajuda a garantir que o sistema seja escalável, especialmente de forma horizontal.