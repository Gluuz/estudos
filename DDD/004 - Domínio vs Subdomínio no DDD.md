### 1. Visão Geral
- **Domínio**  
  O coração do negócio que a aplicação pretende modelar. É o “problema geral” que queremos resolver.
- **Subdomínio**  
  Partes específicas do domínio, identificadas conforme exploramos o negócio (metáfora do farolete):  
  > “Ao iluminar com um farol, vemos só um pedacinho de cada vez.”

### 2. Tipos de Subdomínios
1. **Core Domain** (Domínio Central)  
   - *Definição*: elemento que traz o diferencial competitivo e sem o qual o sistema perde seu sentido.  
   - *Exemplo*: emissão de tickets em uma plataforma de eventos (sem filmes, não há Netflix).  
   - *Implicação*: deve receber o maior investimento em design, modelagem e testes; geralmente é feito sob medida.  

2. **Subdomínio de Suporte**  
   - *Definição*: apoia o Core Domain, garantindo que a operação principal funcione.  
   - *Exemplo*: gestão de estoque/centro de distribuição em e-commerce.  
   - *Implicação*: pode ser parcialmente customizado ou construído internamente, mas não é a “estrela” do sistema.  

3. **Subdomínio Genérico**  
   - *Definição*: funcionalidades comuns a vários sistemas, sem diferencial competitivo.  
   - *Exemplo*: geração de boletos bancários, integração com gateways de pagamento, CRM genérico.  
   - *Implicação*: recomendável usar soluções de prateleira (“off-the-shelf”) ou serviços de terceiros, pois são facilmente substituíveis.

### 3. Por que Separar?
- **Gerenciamento de Complexidade**  
  Dividir um domínio grande em subdomínios facilita o foco e a evolução incremental.
- **Alocação de Recursos**  
  Concentra esforços (tempo e dinheiro) no Core Domain, enquanto aspectos genéricos podem ser terceirizados ou padronizados.
- **Clareza de Responsabilidades**  
  Define equipes, times e contextos de desenvolvimento distintos (e.g., bounded contexts).

### 4. Próximos Passos em DDD
- **Mapeamento de Contextos (Context Map)**  
  Identificar como os subdomínios se relacionam e trocam informações.
- **Bounded Context**  
  Determinar limites claros de modelo e linguagem em cada subdomínio.
- **Integração e Anticorrupção**  
  Projetar como diferentes contextos conversam sem “vazar” regras ou modelos.

---

> **DDD não é só padrões de código**, mas uma abordagem de modelagem guiada pelo entendimento profundo do negócio e pela integração gradual de conhecimento.  
