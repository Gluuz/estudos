### 1. O Papel do Contexto  
- **Contexto determina tudo**: qual área do negócio estamos tratando, quais regras se aplicam e o significado dos termos.  
- **“Contexto é rei”** significa que, antes de qualquer modelo ou entidade, precisamos saber em que contexto estamos atuando.

---

### 2. Mesmo Termo, Significados Diferentes  
- **Exemplo do Ticket**  
  - **Contexto de Vendas** → “Ticket” é o ingresso para um evento.  
  - **Contexto de Suporte** → “Ticket” é o chamado de atendimento ao cliente.  
- Quando encontramos a mesma palavra com dois significados, isso indica frontieras de bounded contexts e exige duas entidades/modularizações distintas.

---

### 3. Termos Diferentes, Mesmo Significado  
- **Exemplo da “Francesinha”**  
  - **Contabilidade** → “Relatório de boletos pagos”  
  - **Agência Bancária** → “Francesinha”  
- Vocabulários distintos para o mesmo conceito também revelam contexts distintos.

---

### 4. Implicações Práticas  
- **Modelo Isolado**: Cada contexto tem seu próprio conjunto de entidades, serviços e repositórios.  
- **Modularização**: No mesmo sistema (monolito ou distribuído), separe componentes por contextos para evitar colisão de significados.  
- **Comunicação Explícita**: Quando contextos precisarem trocar dados, defina contratos claros (DTOs, APIs, eventos) que preservem o significado de cada termo.

---

### 5. Próximos Passos  
- **Context Maps**: identificar como e quando cada contexto interage.  
- **Anticorrupção**: evitar que modelos “vazem” entre contextos com adaptações ou traduções.  
- **Casos de Uso Concretos**: mapear quais operações pertencem a cada contexto para guiar a implementação.

> No DDD, dominar o contexto significa garantir que o mesmo termo não vire fonte de bugs e mal-entendidos — “contexto é rei” ajuda a manter modelos claros e código desacoplado.  
