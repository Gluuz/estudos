## Configurabilidade
- O software deve ser configurável para facilitar mudanças sem modificar o código fonte.
- Exemplos incluem configuração de variáveis de ambiente para conexões de banco de dados ou API keys.
- Teste a configurabilidade subindo a aplicação em diferentes ambientes (produção, staging).

## Extensibilidade
- A aplicação deve ser projetada para crescer, permitindo adição de novos módulos e integração com serviços de terceiros sem grandes alterações.
- Utilização de interfaces e adaptadores pode evitar acoplamento a fornecedores específicos (e.g., gateways de pagamento).
- Conceitos como camadas anticorrupção ajudam a separar a lógica interna do mundo externo.

## Facilidade de Instalação
- A padronização do ambiente, como o uso de containers (e.g., Docker), simplifica a instalação.
- Aplicações configuráveis facilitam a instalação em diferentes ambientes.
- Dependências complexas, como ElasticSearch ou Kafka, precisam de uma abordagem clara para instalação e configuração.

## Reutilização de Componentes
- Em sistemas monolíticos, é possível reutilizar componentes e bibliotecas dentro do mesmo sistema.
- Em arquiteturas distribuídas, evitar duplicidade entre equipes ao compartilhar bibliotecas comuns é fundamental.

## Internacionalização
- No Front End, a mudança de idioma pode desconfigurar layouts e alterar a experiência do usuário.
- No Back End, é necessário considerar aspectos como moedas, gateways de pagamento internacionais e políticas de preços.

## Facilidade de Manutenção
- Sistemas de fácil manutenção são projetados de forma simples, com uso de boas práticas como SOLID, adaptadores e interfaces.
- Testes automatizados são essenciais para garantir a facilidade de manutenção.
- O processo de manutenção envolve tanto a correção de bugs quanto a adição de novas features.

## Portabilidade
- A aplicação deve ser capaz de mudar de fornecedores (e.g., banco de dados, message brokers, ferramentas de observabilidade) sem grande impacto.
- Ferramentas como o OpenTelemetry ajudam na migração entre diferentes soluções de observabilidade.

## Observabilidade
- Implementar uma boa observabilidade garante que você possa identificar problemas rapidamente, antes que o cliente os reporte.
- Centralização de logs, métricas e benchmarks internos ajudam a monitorar o comportamento da aplicação.
- Consolidação dos logs em um único padrão facilita a operação e o suporte.

## Conclusão
Garantir que sua aplicação seja configurável, extensível, fácil de instalar, manter, e observável vai melhorar tanto a operação quanto a evolução do sistema.
