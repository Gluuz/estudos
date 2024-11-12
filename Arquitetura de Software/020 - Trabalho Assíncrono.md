## Introdução
Este conteúdo explora a importância do trabalho assíncrono para aumentar a resiliência em sistemas, abordando a prática através de exemplos cotidianos. Imagine um restaurante de fast-food durante um horário de pico: os pedidos dos clientes são recebidos no caixa, mas o preparo de cada pedido leva tempo. Para lidar com isso, o restaurante organiza os pedidos em uma fila e os processa conforme os funcionários da cozinha têm capacidade para atendê-los. Dessa forma, os clientes fazem seus pedidos e aguardam para serem chamados quando estiverem prontos, sem necessidade de receberem a comida instantaneamente.

Essa abordagem assíncrona permite ao restaurante lidar com um volume maior de pedidos, mesmo com recursos limitados, e garante que todos os pedidos sejam atendidos na ordem em que foram feitos, sem perda ou esquecimento. Esse conceito pode ser aplicado no desenvolvimento de sistemas para aumentar a eficiência e a resiliência, permitindo que sistemas lidem melhor com picos de demanda.

## Problema do Processamento Síncrono
Em um sistema com processamento síncrono, cada requisição é tratada no momento exato em que chega. Se a capacidade do sistema é de 50 requisições por minuto e ele recebe 100, as requisições excedentes podem ser perdidas ou causar falhas, pois o sistema não consegue processá-las de imediato. Isso ocorre porque o design síncrono depende de resposta instantânea e, quando o sistema está sobrecarregado ou passa por problemas, os dados podem ser perdidos.

Por exemplo, ao tentar realizar um pagamento em um sistema que está temporariamente fora do ar, o usuário recebe uma resposta de erro como “tente novamente mais tarde”. Essa abordagem não só frustra a experiência do usuário como também representa uma perda potencial de dados e transações.

## Solução: Processamento Assíncrono
O trabalho assíncrono permite que os sistemas desacoplem o recebimento e o processamento das requisições. Em vez de processar cada requisição imediatamente, o sistema coloca as requisições em uma fila e as processa conforme a capacidade disponível. Para implementar isso, são utilizados intermediários conhecidos como **message brokers** (ex.: RabbitMQ, Kafka, SQS), que armazenam as requisições até que o sistema esteja pronto para processá-las. Assim, mesmo se o sistema estiver temporariamente offline, as mensagens ficam seguras no broker e serão processadas assim que o sistema retornar.

### Funcionamento do Message Broker
O message broker é uma camada intermediária simples, cuja única função é receber e armazenar mensagens. Ele não executa nenhuma lógica de negócios; apenas retém as mensagens até que o sistema backend, responsável pelo processamento, esteja disponível. Isso assegura que nenhuma mensagem é perdida e que o sistema consegue responder a um volume de requisições muito superior à sua capacidade de processamento imediato.

## Vantagens do Trabalho Assíncrono
1. **Maior Resiliência**: Como as mensagens são armazenadas pelo broker, o sistema é capaz de continuar recebendo requisições, mesmo durante períodos de indisponibilidade. Assim que o sistema volta ao ar, ele retoma o processamento a partir das mensagens armazenadas, evitando perdas de dados.
2. **Escalabilidade**: O trabalho assíncrono permite lidar com um volume maior de requisições sem a necessidade de aumentar instantaneamente os recursos computacionais. Como as requisições são processadas aos poucos, o sistema não precisa escalar abruptamente para atender picos de demanda.

## Desafios e Cuidados com Message Brokers
Ao optar pelo trabalho assíncrono, é essencial entender profundamente as ferramentas de message brokers para evitar configurações incorretas que ainda possam levar à perda de mensagens. Alguns pontos importantes incluem:
- **Garantias de Entrega**: Verificar se o broker oferece garantias como "ao menos uma vez" ou "exatamente uma vez" para evitar duplicação ou perda de mensagens.
- **Persistência de Mensagens**: Avaliar se o broker armazena as mensagens em disco ou apenas em memória, pois isso afeta a segurança dos dados em caso de falhas do próprio broker.
- **Configurações de Timeout e Retentativa**: Compreender os mecanismos de timeout e tentativas para evitar que mensagens não sejam processadas no caso de erros intermitentes.

Esses fatores ajudam a assegurar que o sistema, mesmo com falhas, seja resiliente e capaz de processar todas as requisições no momento adequado.

## Conclusão
Implementar o trabalho assíncrono é uma estratégia crucial para construir sistemas mais resilientes e escaláveis. Com message brokers, as requisições são armazenadas de forma segura e processadas conforme a capacidade do sistema, evitando perda de dados e garantindo uma melhor experiência para o usuário. Para aproveitar o máximo da resiliência oferecida pelo trabalho assíncrono, é necessário um entendimento profundo das ferramentas de message brokers e das configurações que asseguram a integridade das mensagens.
 