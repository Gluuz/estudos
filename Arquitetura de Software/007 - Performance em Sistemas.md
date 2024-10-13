## O que é Performance?
- **Performance** é o desempenho de um software em executar um determinado workload.
- Para avaliar performance, **dados são essenciais**.
- Comparação: sempre compare a performance do software com ele mesmo ao longo do tempo, à medida que for otimizado.

## Principais Métricas de Performance

### 1. Latência (Response Time)
- **Latência**: Tempo entre a requisição e a resposta do software.
- **Medição**: Geralmente em milissegundos (ms). Quando passa para segundos, o sistema pode não ser performático.
- **Fatores que afetam a latência**:
  - Processamento da aplicação
  - Rede (distância do datacenter, qualidade da rede)
  - Chamadas externas (APIs, bancos de dados)

### 2. Throughput
- **Throughput**: Número de requisições que o software consegue processar simultaneamente.
- **Objetivo**: Aumentar o número de requisições simultâneas e diminuir a latência para melhorar a performance.
- **Relação com Latência**: Quanto maior o tempo de resposta, menor será o throughput, pois mais conexões ficarão "presas", limitando a capacidade de lidar com novas requisições.

## Performance x Escalabilidade
- **Performance** e **escalabilidade** são conceitos diferentes:
  - Um sistema **performático** pode não ser escalável.
  - Um sistema **escalável** pode não ser performático.

## Melhorando a Performance

### 1. Diminuindo a Latência
- Otimizar o tempo de resposta da aplicação.
- Considerar todos os fatores que influenciam a latência: tempo de processamento, rede, e dependências externas.

### 2. Aumentando o Throughput
- Permitir que o software lide com mais requisições simultâneas.
- Aumentar o número de conexões que o software consegue processar sem reduzir o tempo de resposta.

## Observabilidade
- **Observabilidade** é essencial para detectar gargalos, como chamadas externas lentas ou problemas de rede, que podem estar impactando a latência e o throughput.

---

# Checklist de Performance

## Principais Razões para Baixa Performance

### 1. Processamento Ineficiente
- Algoritmos mal otimizados ou aplicação mal estruturada resultam em baixa performance.

### 2. Recursos Computacionais Limitados
- **Trade-off**: Hardware mais potente aumenta custo, mas oferece mais performance.
- Avalie sempre o equilíbrio entre custo e desempenho.

### 3. Operações Bloqueantes
- **Operações bloqueantes** (ex.: I/O serial) diminuem o throughput.
- Linguagens de programação que permitem **processamento não bloqueante** (ex.: Node.js, Go) aumentam a eficiência.

### 4. Acesso Serial a Recursos
- Evite acessar recursos de forma serial (uma requisição por vez). Utilize paralelismo e concorrência para aumentar a performance.

### 5. Utilização Ineficiente de Núcleos de Processamento
- Em sistemas com múltiplos núcleos, distribua a carga entre threads para aproveitar melhor os recursos de hardware.

## Soluções para Melhorar a Performance

### 1. Escalar a Capacidade Computacional
- Avaliar o "gargalo" do sistema:
  - **CPU**: Processamento insuficiente.
  - **Disco**: I/O lento.
  - **Memória**: Uso excessivo, resultando em swap.
  - **Rede**: Baixa largura de banda ou alta latência.

### 2. Otimização de Algoritmos e Queries
- Melhore algoritmos ineficientes (evite N+1 queries, otimize índices de banco de dados, selecione frameworks leves).

### 3. Concorrência e Paralelismo
- Utilizar linguagens e estratégias que suportem **concorrência e paralelismo**.
  - Exemplo: **Go** permite criar threads para cada requisição, aumentando throughput e performance.

### 4. Banco de Dados
- Otimize o banco de dados:
  - Escolha o **banco correto** para o tipo de aplicação.
  - Use **índices** adequadamente.
  - Monitore queries e otimize as mais lentas com ferramentas de APM.

### 5. Caching
- Utilize **caching** para melhorar a performance:
  - Cache em memória ou em disco.
  - Cachear resultados de queries ou dados frequentemente acessados.
  - Aplicações de alta performance raramente deixam de usar cache.