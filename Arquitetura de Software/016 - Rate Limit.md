## Introdução à Resiliência e Rate Limiting
- **Resiliência**: Estratégias para proteger o sistema e garantir sua continuidade.
- **Rate Limiting**: Técnica que protege o sistema controlando o número de requisições que ele pode processar.

## Funcionamento do Rate Limiting
- **Proteção Baseada na Capacidade do Sistema**:
  - Define o limite de requisições que o sistema pode suportar (exemplo: 100 requisições por segundo).
  - Requisições acima do limite são bloqueadas, retornando um erro (exemplo: código 500).
- **Importância de Conhecer a Capacidade**:
  - Realizar testes de stress para determinar quantas requisições o sistema suporta.
  - Evita surpresas quando o sistema atinge seu limite operacional.

## Desafios do Rate Limiting Genérico
- **Prioridades dos Sistemas**:
  - Sistemas críticos podem ser prejudicados por requisições de menor importância.
  - Exemplo: Um sistema importante pode ser afetado por requisições excessivas de um sistema menos crítico.

## Solução: Rate Limiting por Cliente e Prioridade
- **Configuração de Limites por Cliente**:
  - Definir limites específicos para diferentes tipos de clientes com base na criticidade.
  - Exemplo: Sistema principal com 60 requisições por segundo e sistemas menos críticos com 40.
- **Benefícios**:
  - Garante que sistemas prioritários sempre tenham acesso necessário.
  - Evita que sistemas não críticos sobrecarreguem o sistema principal.

## Conclusão
- **Implementação Eficiente**:
  - Não basta definir um limite geral; é essencial programar limites conforme a prioridade e a importância de cada cliente.
  - A estratégia correta de rate limiting assegura a continuidade e a qualidade do serviço para os sistemas mais críticos.

