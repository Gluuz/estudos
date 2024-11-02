A resiliência é um conceito crucial para garantir a robustez de um sistema. Ela envolve a adoção de estratégias intencionais para que um software consiga se adaptar a falhas inesperadas. A famosa frase "resiliência: você dobra ou quebra" ilustra bem a ideia: quando ocorre um erro, o sistema pode falhar completamente ou se recuperar e continuar funcionando, mesmo que parcialmente.

## O que é Resiliência?

Resiliência refere-se à capacidade de um sistema de se adaptar a falhas. Não basta o sistema lançar uma exceção e abandonar a requisição; ele deve ter um "plano B", ou até um "plano C", para garantir que o cliente tenha sua solicitação atendida, mesmo que de maneira degradada.

### Exemplo:

- **Problema:** Um sistema de consulta de CEP está fora do ar.
- **Solução Resiliente:** O sistema ainda permite criar um usuário e tenta processar a requisição posteriormente, ao invés de simplesmente falhar.

## Importância da Resiliência

É certo que o software vai falhar em algum momento, seja por problemas internos (bugs) ou por problemas externos (como uma falha na rede). A questão é: quão preparado está o sistema para lidar com isso? Sistemas resilientes minimizam os impactos dessas falhas, como perda de dados ou transações críticas.

## Estratégias de Resiliência

Um dos maiores desafios é garantir que o sistema tenha planos de contingência para lidar com problemas, como:

- Falhas em integrações com APIs externas (ex: gateway de pagamento fora do ar).
- Problemas de rede ou latência.
- Bugs inesperados.

Essas estratégias garantem que, mesmo diante de um erro, o impacto seja mínimo para o usuário e o negócio. Portanto, resiliência é essencial para entregar a melhor experiência possível ao cliente, mesmo em cenários adversos.

# Resiliência em Sistemas Distribuídos

Quando falamos sobre resiliência, não basta criar planos A, B ou C sem estratégia. É crucial adotar mecanismos que protejam o sistema e o ecossistema no qual ele está inserido. Em sistemas distribuídos, como os que utilizam microsserviços, temos que proteger tanto a nossa aplicação quanto a dos demais serviços, já que todos dependem uns dos outros.

## Proteger e Ser Protegido

Imagine que você tem três sistemas integrados. Se o sistema "A" depende de uma resposta do sistema "B", que por sua vez precisa do sistema "C", e o "C" está lento ou falhando, todo o ecossistema é impactado. Isso gera um "efeito dominó", onde a lentidão ou falha de um sistema compromete todos os outros. 

Por isso, uma abordagem resiliente envolve adotar mecanismos que permitam ao sistema falhar de forma controlada, informando que não pode lidar com mais requisições (por exemplo, retornando um código HTTP 500). Isso evita que o sistema continue recebendo e processando requisições enquanto está em recuperação, o que poderia sobrecarregá-lo ainda mais e causar falhas em cascata.

## Efeito Dominó

Um sistema lento no ar pode ser pior do que um sistema fora do ar. Quando um serviço está lento, ele começa a segurar respostas dos sistemas que dependem dele, criando uma cadeia de lentidão que pode levar à falha de todo o ecossistema. Nesse caso, é melhor que o sistema falhe rapidamente e informe sua incapacidade de responder, para que os outros possam tomar decisões inteligentes, como usar um plano alternativo.

## Estratégia de Resiliência

A verdadeira dificuldade em alcançar resiliência não está apenas em programar, mas em compreender esses conceitos. Quando o sistema "C" percebe que está sobrecarregado, ele deve sinalizar isso para "B" e "A", permitindo que todo o ecossistema funcione melhor e evite o colapso. Da mesma forma, os sistemas "A" e "B" precisam ser programados para não sobrecarregar "C" com requisições desnecessárias.

Esses conceitos são essenciais para sistemas distribuídos de alta performance e escalabilidade. Eles garantem que, mesmo em situações adversas, o sistema possa continuar a operar de maneira eficiente, sem comprometer a experiência do usuário.
