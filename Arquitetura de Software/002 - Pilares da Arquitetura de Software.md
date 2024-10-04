Os pilares da arquitetura de software são fundamentais para garantir que um sistema seja robusto, escalável e capaz de evoluir ao longo do tempo. Vamos explorar cada um desses pilares em detalhes:

## 1. Estruturação

- A estrutura de um software é a base sobre a qual ele é construído. Uma arquitetura bem estruturada permite fácil evolução, modularidade e manutenção.
- A estrutura deve ser clara e atender aos objetivos do negócio, garantindo que os componentes sejam organizados de forma a facilitar a comunicação e a interação entre eles.
- A estruturação envolve definir como as partes do sistema se conectam, como os dados fluem entre os componentes e como os sistemas se comunicam.

## 2. Componentização

- Componentização refere-se à divisão do software em partes menores, chamadas de componentes, que podem ser desenvolvidas, testadas e mantidas de forma independente.
- Essa abordagem não só facilita a evolução do software, mas também permite que diferentes equipes trabalhem em paralelo em partes distintas do sistema.
- A escolha de componentes apropriados e sua interação é crucial, pois a comunicação inadequada entre eles pode levar a problemas de desempenho e falhas no sistema.

## 3. Relacionamento entre Sistemas

- É importante considerar como diferentes sistemas se comunicam, especialmente em um cenário onde um software pode incorporar múltiplos sistemas internos ou interagir com sistemas externos.
- A comunicação deve ser bem definida, com protocolos apropriados e regras de segurança em vigor.
- Uma interação inadequada pode resultar em falhas graves, o que torna crucial entender como os sistemas se conectam e como eles trocam informações.

## 4. Governança

- Governança em arquitetura de software envolve a criação de normas, padrões e documentação que guiem o desenvolvimento do software.
- Essa governança não precisa ser burocrática, mas deve incluir regras sobre padronização de código, documentação, práticas de versionamento (como o uso do Git), e definições claras sobre como as equipes devem trabalhar juntas.
- Uma boa governança garante que, independentemente das mudanças de equipe ou de liderança, o software possa continuar a evoluir sem depender de indivíduos específicos. Isso facilita a integração de novos desenvolvedores e garante que todos sigam as mesmas diretrizes.

### A Importância dos Pilares

A ausência de um ou mais desses pilares pode resultar em um software difícil de manter, evoluir e escalar. Aqui estão alguns pontos-chave sobre a importância desses pilares:

- **Evolução Sustentável**: Uma boa estrutura e componentização permitem que o software se adapte rapidamente às mudanças nas necessidades do negócio sem exigir reescritas completas.
- **Redução do Risco de Dependência**: Com governança bem definida, as equipes se tornam menos dependentes de indivíduos específicos, o que evita a paralisia do projeto quando um membro-chave deixa a equipe.
- **Facilidade de Onboarding**: Uma documentação clara e regras bem definidas facilitam a integração de novos desenvolvedores, permitindo que eles contribuam mais rapidamente e com maior eficácia.
- **Comunicação Eficiente**: Definir claramente como os sistemas se comunicam minimiza os erros de integração e melhora a eficiência operacional.

### Conclusão

Os pilares da arquitetura de software são essenciais para criar um sistema robusto e sustentável. Ao focar na estruturação, componentização, relacionamento entre sistemas e governança, os desenvolvedores podem garantir que o software não apenas atenda às necessidades do presente, mas também se adapte às exigências futuras. Um software bem arquitetado é um ativo valioso para qualquer organização, pois permite que ela responda rapidamente às mudanças no mercado e continue a agregar valor ao longo do tempo.

---

## Requisitos Arquiteturais (RA's)

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
