### 1. Definição  
- **Elementos transversais** são conceitos ou entidades que aparecem em múltiplos contextos delimitados, mas com **perspectivas e informações diferentes**.

### 2. Exemplo do Cliente  
- **Contexto de Vendas**  
  - Atributos relevantes:  
    - Histórico de ingressos comprados  
    - Eventos favoritos  
    - Vendedor responsável  
- **Contexto de Suporte**  
  - Atributos relevantes:  
    - Chamados abertos (tickets de dúvida)  
    - Departamento e agente responsável  
    - SLA e status de atendimento  

Embora seja **o mesmo “cliente”**, cada contexto define **um modelo distinto** — e tentar unificar tudo numa única classe gera complexidade e acoplamento.

### 3. Por que Separar?  
1. **Coesão de Modelo**  
   - Cada entidade reflete apenas os dados e comportamentos necessários ao seu contexto.  
2. **Manutenção e Evolução**  
   - Alterações num contexto não “vazam” para os demais.  
3. **Facilidade de Modularização**  
   - Facilita a extração de microsserviços ou a refatoração de partes do sistema.

### 4. Quando Surgem Conflitos  
- **Entidade inchada**: ao misturar campos de vários contextos, a classe cresce demais.  
- **Dificuldade de escalar**: separar responsabilidades se torna custoso em aplicações monolíticas.

### 5. Como Lidar  
- **Modelos Contextuais**  
  - Crie **DTOs** ou **view models** específicos para cada bounded context.  
  - Use repositórios e serviços que atendam somente à perspectiva local.  
- **Tradução e Sincronização**  
  - Se necessário, traduza dados entre contextos via adaptadores, eventos ou APIs, respeitando as fronteiras.

---

> Elementos transversais existem em vários contextos, mas cada “visão” carrega apenas o que é essencial para resolver o problema daquele bounded context.  
