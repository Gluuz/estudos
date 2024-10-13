### Escala Vertical
- **Escala Vertical** envolve aumentar a capacidade de uma única máquina, adicionando mais poder computacional (CPU, RAM, etc.).
- Cada vez que se aumenta os recursos da máquina, ela pode lidar com mais requisições.
- O trade-off está no custo: aumentar a capacidade computacional pode ser caro e tem um limite físico.
  
### Escala Horizontal
- **Escala Horizontal** significa aumentar a capacidade computacional adicionando mais máquinas ao invés de melhorar uma única.
- Normalmente, se utiliza um **load balancer** para distribuir as requisições entre várias máquinas.
- A escala horizontal é mais eficiente para lidar com um grande número de requisições e permite expandir o sistema de forma mais flexível.

## Relação entre Performance e Escalabilidade
- A **escalabilidade** está diretamente relacionada à **performance** porque aumentar a capacidade de lidar com requisições melhora o desempenho do sistema.
- Você pode escalar verticalmente ou horizontalmente, dependendo das necessidades do seu sistema.
  
---

# Concorrência e Paralelismo

### Diferença entre Concorrência e Paralelismo
- **Concorrência**: É a habilidade de lidar com muitas coisas ao mesmo tempo, mas não necessariamente fazer tudo ao mesmo tempo. O sistema organiza várias tarefas e as executa intercalando, sem bloquear.
  - Exemplo: Responder várias requisições web, intercalando a execução.
  
- **Paralelismo**: É fazer várias coisas ao mesmo tempo. Requer múltiplos núcleos ou CPUs para processar tarefas simultaneamente.
  - Exemplo: Processar várias requisições ao mesmo tempo em diferentes threads ou processos.

### Rob Pike sobre Concorrência e Paralelismo
- Rob Pike, um dos criadores de Go, define:
  - "Concorrência é lidar com várias coisas ao mesmo tempo."
  - "Paralelismo é fazer várias coisas ao mesmo tempo."

### Exemplo de Concorrência e Paralelismo em Web Servers
- Imagine um **web server** recebendo cinco requisições, onde cada requisição leva 10 milissegundos para ser processada:
  - Se o servidor processar as requisições de forma **serial** (uma de cada vez), demorará 50 milissegundos para processar todas.
  - Em um servidor **concorrente ou paralelo**, onde se utilizam múltiplas threads, ele pode processar as cinco requisições simultaneamente em 10 milissegundos.

### Concorrência em Web Servers
- **Processos bloqueantes** reduzem a eficiência, pois cada requisição precisa esperar a anterior terminar.
- Servidores como **Apache** usam **workers**, onde cada worker pode processar uma requisição de cada vez. O número de workers determina quantas requisições podem ser processadas simultaneamente.
- Para cada thread criada no sistema operacional, há um custo de memória. Cada thread pode consumir até 1MB de memória, limitando o número de threads que podem ser criadas simultaneamente.
  
### Go e Concorrência com Green Threads
- O **Go** usa um conceito de **green threads**, onde o gerenciamento de threads é feito pelo runtime da linguagem, e não pelo sistema operacional. Isso reduz o custo de memória, já que uma green thread pode consumir apenas 2KB ao invés de 1MB.
- Essa abordagem permite ao Go criar muitas threads e processar várias requisições de forma altamente eficiente.

### Node.js e o Sucesso com Concorrência
- **Node.js** também se destacou por sua capacidade de lidar com requisições de forma **não bloqueante**, utilizando um **event loop** para processar múltiplas requisições simultaneamente.
- **PHP Swoole** é outra tecnologia que adota uma estratégia semelhante, permitindo ao PHP lidar com requisições concorrentes com maior eficiência.

---

- A escolha entre concorrência e paralelismo depende da arquitetura do sistema e do objetivo de performance.
- Em aplicações web, servidores que utilizam threads não bloqueantes ou green threads geralmente conseguem lidar melhor com altas cargas.
- A **escalabilidade** vem como solução para aumentar a capacidade computacional e melhorar o throughput, seja através de escala vertical ou horizontal.
