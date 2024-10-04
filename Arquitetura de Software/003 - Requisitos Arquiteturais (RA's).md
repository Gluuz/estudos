Os Requisitos Arquiteturais (RA's) são essenciais para guiar o desenvolvimento de software, especialmente em um cenário onde múltiplos times e squads estão envolvidos. Eles ajudam a definir como a arquitetura do sistema deve ser planejada para atender tanto às necessidades funcionais quanto às não funcionais. Vamos detalhar os principais aspectos dos RA's:

### O Que São Requisitos Arquiteturais?

- **Definição**: RA's são requisitos que impactam diretamente a arquitetura de um software. Eles podem ser tanto funcionais quanto não funcionais, mas os não funcionais são frequentemente mais relevantes para a arquitetura.
- **Impacto na Arquitetura**: Entender como esses requisitos influenciam a arquitetura é crucial para garantir que o software atenda às expectativas do negócio e funcione de forma eficiente e segura.

### Exemplos de Requisitos Arquiteturais

1. **Performance**:
    - **Tempo de Resposta**: Exemplo: “A aplicação não pode levar mais de 500 milissegundos para responder a requisições.”
    - **Throughput**: Exemplo: “A aplicação deve ser capaz de suportar 50 transações por segundo em uma máquina com 1000 milicores.”

2. **Armazenamento de Dados**:
    - **Compliance**: Exemplo: “Os dados devem ser armazenados em data centers localizados na Europa, se a aplicação operar na Europa.”
    - **Tecnologia**: Escolhas de banco de dados, como usar DynamoDB, também caem nesta categoria.

3. **Escalabilidade**:
    - **Estratégia de Escalabilidade**: O software deve escalar horizontalmente ou verticalmente?
    - **Load Balancer**: Qual o método de balanceamento de carga? Round-robin ou outras abordagens?

4. **Segurança**:
    - **Certificações**: Exemplo: “A aplicação deve cumprir com as certificações PCI para transações com cartões de crédito.”
    - **Comunicação Segura**: Todos os microsserviços devem usar TLS para comunicação.

5. **Requisitos Legais**:
    - **Conformidade com Legislações**: Exemplo: “A aplicação deve seguir as diretrizes da LGPD, garantindo que os dados sejam protegidos contra vazamentos.”
    - **Auditoria**: Como e por quanto tempo os dados de auditoria serão retidos?

6. **Requisitos de Marketing**:
    - **Disponibilidade**: A aplicação deve ter alta disponibilidade durante campanhas de marketing.
    - **Personalização**: Capacidade de rastrear e personalizar experiências de usuário de acordo com campanhas.

### Coleta de Requisitos Arquiteturais

- **Interação com Stakeholders**: É crucial envolver especialistas de domínio, executivos e usuários finais para coletar requisitos. Isso normalmente envolve perguntas direcionadas a entender suas necessidades e expectativas.
- **Documentação**: Embora o uso de planilhas para documentar requisitos ainda seja comum, o processo deve ser contínuo e iterativo, permitindo ajustes conforme novas informações surgem.
- **Clareza e Transparência**: Quanto mais clareza houver sobre os RA's, menos ruído haverá no desenvolvimento e na implementação do software. É vital fazer as perguntas certas e manter uma comunicação aberta entre todos os envolvidos.

### Conclusão

Os Requisitos Arquiteturais são uma parte crítica do processo de desenvolvimento de software, impactando diretamente a arquitetura e a qualidade do produto final. Um entendimento claro e uma documentação eficaz dos RA's podem prevenir problemas futuros e garantir que o software atenda às expectativas do negócio e das partes interessadas. No final das contas, a interação ativa com os stakeholders e a coleta de informações são fundamentais para o sucesso do projeto. Ter em mente que a arquitetura é um reflexo das necessidades de todas as áreas da organização garante que o software seja adaptável e alinhado aos objetivos estratégicos da empresa.
