### 1. Principais Padrões de Context Mapping  
- **Partnership**  
  Dois contexts colaboram mutuamente, expondo e consumindo APIs um do outro.  
- **Shared Kernel**  
  Núcleo compartilhado (biblioteca/SDK) usado por ambos para evitar duplicação de lógica.  
- **Customer / Supplier (Upstream / Downstream)**  
  Upstream fornece o contrato; downstream adapta-se a ele.  
- **Conformista**  
  Downstream consome diretamente a API do upstream, aceitando seu modelo (alto acoplamento).  
- **Anticorruption Layer (ACL)**  
  Camada adaptadora que traduz e protege seu modelo interno de vazamento de conceitos externos.  
- **Open Host Service**  
  Contexto oferece serviços externos via protocolo bem definido (REST, gRPC, etc.).  
- **Published Language**  
  Linguagem padronizada (JSON, XML, DTOs) usada para comunicação entre contexts.  
- **Separated Ways**  
  Contexts totalmente isolados, sem nenhum canal de comunicação.  
- **Big Ball of Mud**  
  Sistema desestruturado, sem boundaries claros — o antônimo do que buscamos em DDD.

### 2. Starter Kit no GitHub  
- **Repositório DDD Crew/context-mapping**  
  - Cheat sheet com ícones e diagramas para cada padrão.  
  - Tabelas de significado e exemplos visuais prontos para uso.  
- **Template Miro “Read Only”**  
  - Use diretamente no Miro para arrastar e soltar ícones e montar seu próprio context map.

### 3. Como Aplicar no Seu Projeto  
1. **Liste** todos os seus bounded contexts.  
2. **Defina** para cada par de contexts qual padrão de relacionamento se aplica.  
3. **Desenhe** o mapa usando os ícones do starter kit (GitHub/Miro).  
4. **Valide** com as equipes e ajuste contratos (APIs, eventos, adaptadores).

### 4. Benefícios  
- **Agilidade**: acelera a criação de context maps padronizados.  
- **Clareza**: comunicação visual uniforme entre times.  
- **Governança**: evita mal-entendidos e dependências ocultas.

### 5. Próximos Passos  
- Explore outros padrões e variações no repositório.  
- Personalize o starter kit para as convenções da sua empresa.  
- Realize workshops de context mapping para engajar todos os stakeholders.

> Com este kit de padrões e templates, você tem tudo o que precisa para começar seu context mapping de forma rápida, consistente e colaborativa.  
