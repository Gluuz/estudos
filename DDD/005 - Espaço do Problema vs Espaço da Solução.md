### 1. Definições
- **Espaço do Problema**  
  - Visão macro do domínio e suas complexidades iniciais.  
  - Aqui identificamos o que precisa ser resolvido e fragmentamos o domínio em subdomínios (problemas menores).

- **Espaço da Solução**  
  - Onde fazemos a **modelagem** do domínio: definimos entidades, agregados, serviços etc.  
  - Cada subdomínio vira um **contexto delimitado** (bounded context), pronto para implementação.

### 2. Metáfora dos Quadrados
- 🟥 **Quadrado 1 (Problema)**  
  - Representa o domínio geral e seus desafios.  
  - Mapeia subdomínios como “pedaços” do problema a enfrentar.

- 🟦 **Quadrado 2 (Solução)**  
  - Abrange a modelagem detalhada de cada subdomínio.  
  - Transforma problemas identificados em contextos delimitados, com regras e linguagem próprias.

### 3. Fluxo em DDD
1. **Exploração do Problema**  
   - Use o “farolete” para entender o domínio e descobrir subdomínios.  
2. **Identificação de Subdomínios**  
   - Separe o grande domínio em problemas menores (core, suporte, genéricos).  
3. **Delimitação de Contextos**  
   - Cada subdomínio torna-se um bounded context, isolando modelo e linguagem.  
4. **Modelagem da Solução**  
   - Defina objetos de valor, entidades, agregados e serviços dentro de cada contexto.  
5. **Priorização & Implementação**  
   - Concentre-se primeiro no Core Domain e nos contextos de maior valor para o negócio.

### 4. Benefícios
- **Clareza de Responsabilidades**  
  Cada contexto tem fronteiras e um modelo claros.
- **Modularidade**  
  Times trabalham em paralelo sem conflitar modelos.
- **Manutenibilidade**  
  Mudanças em um contexto têm impacto limitado nos demais.
- **Foco no Negócio**  
  A modelagem direcionada garante que a solução reflita verdadeiramente as regras do domínio.

---

> Em DDD, o **problema** é o ponto de partida, e a **solução** é a modelagem organizada desse problema em contextos delimitados — é assim que atacamos o software “no coração”.  
