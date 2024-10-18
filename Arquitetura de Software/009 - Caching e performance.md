# Caching e Performance

## O que é Caching?
- **Caching** é uma técnica que ajuda a aumentar a performance de sistemas ao armazenar resultados de processos repetitivos, para que possam ser reutilizados posteriormente sem a necessidade de novo processamento.
- Reduz o **response time** (tempo de resposta) e aumenta o **throughput** (capacidade de processamento de requisições).

## Tipos de Cache

### 1. Cache na Borda (Edge Computing)
- O **cache na borda** processa dados em servidores distribuídos, localizados o mais próximo possível dos usuários, antes de chegarem à cloud principal.
- Plataformas como **Cloudflare** oferecem caching de arquivos estáticos (HTML, CSS, JavaScript, imagens) diretamente na borda.
- **Benefício**: Acesso mais rápido e menos dependência da infraestrutura principal (cloud, Kubernetes), com custos reduzidos.

### 2. Cache de Dados Estáticos
- Dados como imagens, folhas de estilo (CSS), JavaScript e outros arquivos estáticos podem ser armazenados em cache na borda ou servidores proxy.
- **Objetivo**: Evitar que o sistema precise processar ou servir os mesmos dados repetidamente, liberando recursos computacionais para outras tarefas.

### 3. Cache de Páginas Web
- Cachear páginas inteiras, como home pages, páginas de contato, ou páginas de notícias, evita o reprocessamento de conteúdos que não mudam com frequência.
  - **Exemplo**: Cache de 5 minutos para uma home page.
  - **Benefício**: Ao invés de acessar o banco de dados e gerar a página dinamicamente, o sistema entrega uma versão já processada da página.
  
### 4. Cache de Funções Internas
- Algoritmos complexos que precisam ser recalculados a cada requisição podem ser cacheados.
  - **Exemplo**: Um algoritmo que muda seus resultados a cada meia hora pode ter seu resultado cacheado, evitando novo processamento para cada requisição.
  - **Benefício**: Reduz a carga computacional, especialmente para algoritmos que acessam bancos de dados.

### 5. Cache de Objetos
- Sistemas como ORMs (Object-Relational Mappers), que mapeiam classes para bancos de dados, podem se beneficiar de caching para evitar a constante correlação entre estruturas de banco de dados e objetos de código.
  - **Exemplo**: O **Doctrine ORM** no PHP pode cachear o mapeamento de tabelas e classes, evitando o reprocessamento de dados que raramente mudam.

## Cache Exclusivo vs Cache Compartilhado

### Cache Exclusivo
- **Cache exclusivo** é gerado e utilizado por uma única máquina, o que resulta em **baixa latência** devido ao processamento local.
  - **Problema**: Cada máquina tem seu próprio cache, o que leva à duplicação de dados e à necessidade de gerar cache em várias máquinas para o mesmo conteúdo.
  - **Exemplo**: Se um usuário loga em uma máquina, a sessão é cacheada localmente, mas ele precisará fazer login novamente se acessar outra máquina.

### Cache Compartilhado
- **Cache compartilhado** é armazenado em um servidor central, acessado por todas as máquinas da rede.
  - **Benefício**: Não há duplicação de cache, e todos os servidores podem acessar o mesmo conteúdo cacheado.
  - **Desvantagem**: Há uma **latência maior** ao acessar o cache centralizado, mas ele elimina a necessidade de cachear o mesmo conteúdo em várias máquinas.
  - **Exemplo**: Se o login de um usuário está cacheado em um servidor compartilhado, ele permanecerá logado independentemente de qual máquina acessar.

## Ferramentas e Soluções de Cache
- **Redis**: Um dos caches mais populares, considerado um banco de dados em memória. Extremamente rápido e capaz de armazenar dados temporários.
- **Memcached**: Outra solução de cache em memória, útil para caching de objetos e dados simples.
- **MySQL Query Cache**: O MySQL pode cachear resultados de consultas, acelerando o tempo de resposta em consultas repetidas.

---
- **Caching** é uma prática essencial para melhorar a performance de sistemas, reduzindo o tempo de resposta e liberando recursos computacionais.
- Entender as diferenças entre **cache exclusivo** e **compartilhado** é fundamental para escolher a melhor estratégia para o seu sistema.
- Ferramentas como **Redis** e **Cloudflare** são amplamente utilizadas e eficazes para diferentes tipos de caching, desde dados estáticos até sessões de usuários.
