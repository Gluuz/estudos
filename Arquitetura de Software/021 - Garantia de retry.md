## Introdução
Neste conteúdo, exploramos a importância da garantia de entrega ao trabalharmos com resiliência em sistemas. Quando um sistema realiza uma requisição, é crucial assegurar que a mensagem enviada realmente chegue ao destino. Entretanto, diversos fatores, como falhas temporárias ou lentidão do sistema receptor, podem impedir o sucesso da entrega. Para contornar esses desafios, uma das estratégias utilizadas é a **política de retry**, que reenvia a mensagem em intervalos de tempo até que o sistema consiga responder. 

## Problema do Retry Linear
Um problema comum ao realizar retries é que múltiplos sistemas podem estar fazendo tentativas simultâneas. Imagine que dez sistemas tentem acessar o mesmo serviço ao mesmo tempo, mas ele falha em responder. Após um período fixo (ex., 2 segundos), todos os sistemas fazem um retry ao mesmo tempo novamente, sobrecarregando o serviço e causando mais falhas. Esse padrão de retries lineares frequentemente resulta em uma série de falhas seguidas, pois o serviço não tem tempo para se recuperar.

## Solução: Exponential Backoff
Para evitar essa sobrecarga simultânea, uma técnica utilizada é o **exponential backoff**, que aumenta exponencialmente o tempo entre cada tentativa. Por exemplo, se a primeira tentativa falhar, a próxima ocorre após 2 segundos, a seguinte após 4 segundos, depois 8, 16, e assim por diante. Essa espera crescente entre as tentativas permite que o sistema receptor tenha mais tempo para se recuperar, aumentando as chances de sucesso no retry sem sobrecarregar o sistema.

## Estratégia Avançada: Exponential Backoff com Jitter
Uma melhoria adicional ao exponential backoff é a adição de **jitter**, que introduz um pequeno fator aleatório nos tempos de espera para que as requisições não sejam repetidas em intervalos exatamente iguais. Com o jitter, os tempos de retry são ligeiramente diferentes entre os sistemas, evitando picos de tentativa simultâneos. Por exemplo, em vez de tentar após 2, 4, 8, 16 segundos fixos, os retries podem ser feitos após 2.1, 4.05, 8.3, e assim por diante.

Essa variação ajuda a distribuir as tentativas, permitindo que o sistema receptor processe as requisições de forma mais eficiente e reduzindo as chances de falhas consecutivas devido à sobrecarga.

## Vantagens do Exponential Backoff com Jitter
1. **Redução de Sobrecarga Simultânea**: A introdução de jitter evita que todas as requisições sejam repetidas exatamente ao mesmo tempo, distribuindo-as ao longo do tempo e reduzindo a sobrecarga simultânea.
2. **Maior Probabilidade de Sucesso**: Com tempos de espera mais longos e distribuídos, o sistema receptor tem maior chance de se recuperar e processar as requisições, aumentando a resiliência geral.
3. **Melhor Uso dos Recursos**: Essa técnica evita tentativas desnecessárias e frequentes em momentos de indisponibilidade do sistema receptor, resultando em uso mais eficiente dos recursos.

## Considerações Finais
Implementar políticas de retry é uma estratégia fundamental para garantir a resiliência em sistemas distribuídos. A combinação de **exponential backoff com jitter** é uma das melhores práticas para reduzir a sobrecarga de sistemas sob pressão, garantindo que as requisições cheguem ao destino de forma eficiente. Ao configurar o retry, é essencial evitar números fixos e aplicar uma lógica estruturada que favoreça o sucesso das tentativas em momentos oportunos, sem criar novos gargalos.

Este conceito é uma peça importante para desenvolver sistemas robustos e resilientes que possam lidar com interrupções temporárias e cargas elevadas, mantendo a integridade das operações.
