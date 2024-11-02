## Introdução à API Gateway
- **API Gateway**: Componente central que gerencia todas as requisições para a aplicação.
- **Função Principal**: Centralizar o recebimento das requisições e aplicar regras, políticas e plugins para autorizar ou negar o acesso.

## Funcionalidades da API Gateway
- **Centralização das Requisições**:
  - Todas as requisições passam pela API Gateway.
  - Aplicação de políticas como autenticação, autorização, rate limiting, entre outras.
- **Autenticação e Autorização**:
  - Valida se a requisição está autenticada antes de permitir o acesso aos serviços internos.
  - Uso de tokens JWT para autenticação.
  - Analogía: API Gateway funciona como a portaria de um condomínio, filtrando quem pode entrar.

## Benefícios para a Resiliência do Sistema
- **Proteção contra Abusos**:
  - Evita que robôs ou requisições maliciosas sobrecarreguem os servidores internos.
  - Rejeita requisições não autenticadas, poupando recursos do sistema.
- **Aplicação de Regras de Prioridade**:
  - Permite definir limites específicos para diferentes tipos de clientes ou serviços.
  - Exemplo: Limitar 100 requisições por segundo, dividindo entre clientes prioritários e não prioritários.

## Exemplos de API Gateway
- **Kong**:
  - Pode ser utilizada como stand-alone ou como Ingress Controller em Kubernetes.
  - Baseada no Nginx, oferecendo alta performance e flexibilidade.
  - Suporta diversos plugins, como rate limiting e health checks.

## Funcionalidades Avançadas
- **Rate Limiting**:
  - Controle do número de requisições por segundo para diferentes clientes ou serviços.
  - Implementação de prioridades para garantir que serviços críticos recebam as requisições necessárias.
- **Health Check**:
  - Monitora a saúde dos serviços backend.
  - Desvia as requisições para serviços saudáveis, evitando o envio para serviços com problemas.
- **Transformações de Requisições**:
  - Conversão de formatos de dados (ex.: XML para JSON).
  - Execução de funções específicas (ex.: Lambda) em resposta a determinadas URLs.
- **Manipulação de Headers e Logs**:
  - Adição ou remoção de informações nos headers das requisições.
  - Coleta de logs para monitoramento e análise.

## Implementação e Gerenciamento
- **Implementação via Código ou Service Mesh**:
  - Pode ser implementada diretamente no código da aplicação ou gerenciada por uma service mesh.
  - Service mesh permite a aplicação de políticas de forma centralizada na camada de rede.
- **Configuração e Fine Tuning**:
  - Ajuste das políticas e regras conforme as necessidades da aplicação e infraestrutura.
  - Utilização de plugins e recursos oferecidos pela API Gateway para otimizar o desempenho e a resiliência.

## Cuidados na Utilização
- **Evitar Regras de Negócio Involuntárias**:
  - Garantir que a API Gateway não aplique regras que interfiram na lógica de negócio da aplicação.
  - Exemplo: Transformar automaticamente formatos de dados apenas quando necessário.

## Conclusão
- **Importância da API Gateway**:
  - Fundamental para gerenciar, proteger e otimizar as requisições em arquiteturas de microservices.
  - Facilita a implementação de estratégias de resiliência, como autenticação, rate limiting e health checks.
- **Versatilidade e Recursos**:
  - Oferece uma ampla gama de funcionalidades que atendem às necessidades modernas de desenvolvimento e operação de aplicações.
- **Adaptação e Evolução**:
  - Políticas e configurações devem ser continuamente ajustadas para acompanhar a evolução da aplicação e garantir a máxima eficiência e resiliência.

