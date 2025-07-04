### 1. O que é Context Mapping  
- **Modelagem Estratégica** dos seus bounded contexts e das **regras de interação** entre eles.  
- Serve para orientar a **organização de times**, definir **responsabilidades** e projetar **fluxos de dados** de forma clara.

### 2. Identificando seus Contexts  
1. **Venda de Ingressos Online** (Core Domain)  
2. **Venda de Ingressos via Parceiros**  
3. **Pagamentos**  
4. **Suporte ao Cliente**  

> Cada contexto possui seu modelo, sua equipe e seus “especialistas de domínio”.

### 3. Padrões de Relacionamento Comuns  
1. **Parceria (Partnership)**  
   - Dois contexts que colaboram e consumem APIs um do outro.  
   - Ex.: Vendas Online e Vendas de Parceiros expõem/consomem mutuamente endpoints para gerir ingressos.

2. **Shared Kernel**  
   - Núcleo compartilhado de código (biblioteca, SDK) usado por ambos.  
   - Útil para evitar duplicação de lógica, mas cuidado com dependências fortes.

3. **Cliente / Fornecedor (Upstream / Downstream)**  
   - **Upstream** (fornecedor): define o contrato e evolui suas APIs primeiro.  
   - **Downstream** (cliente): adapta-se ao contrato do upstream.  
   - Ex.: Pagamentos (upstream) fornece serviço para Vendas (downstream).

4. **Conformista vs. Camada Anticorrupção (ACL)**  
   - **Conformista**: o client usa diretamente a API do fornecedor, aceitando seus modelos e limitações (alto acoplamento).  
   - **ACL (Anticorruption Layer)**: camada adaptadora que “traduz” o modelo externo para o seu, permitindo trocar fornecedores sem afetar a lógica interna.

5. **Serviços Externos**  
   - Gateways de pagamento (XPTO, bancos) são contexts **externos** com relação conformista; quase sempre exigem ACL para isolamento.

### 4. Organizando Times e Dependências  
- **Times por Contexto**  
  - Cada bounded context pode ter um time dedicado, falando a mesma linguagem ubíqua e gerenciando seu próprio backlog.  
- **Mapeamento Visual**  
  - Desenhe caixas (contexts) e setas (relacionamentos: partnership, shared kernel, upstream/downstream, ACL).

### 5. Benefícios do Context Mapping  
- **Visão Estratégica**: clareza sobre quem afeta quem e como evoluir cada parte sem surpresas.  
- **Menor Acoplamento**: identifica pontos críticos de integração e permite equipe independentes.  
- **Planejamento de Roadmap**: prioriza entregas e mudanças conforme o impacto nos contexts vizinhos.

### 6. Próximos Passos  
- Explorar padrões avançados de Context Mapping:  
  - **Open Host Service**, **Published Language**, **Conformist**, **Anticorruption Layer**, **Separate Ways**, entre outros.  
- Criar um **Context Map** visual para sua organização e validar com stakeholders.
- Definir **contratos de integração** e **roteiros de migração** para reduzir riscos.

> Um bom Context Map é tão valioso quanto o próprio modelo de domínio: ele garante que todos saibam **quem faz o quê** e **como conversar** de forma controlada entre contexts.  
