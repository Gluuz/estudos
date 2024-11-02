## Introdução ao Circuit Breaker
- **Circuit Breaker**: Padrão de design que protege sistemas distribuídos de falhas em cascata.
- **Objetivo**: Evitar que falhas em um sistema afetem outros sistemas interconectados, garantindo resiliência.

## Funcionamento do Circuit Breaker
- **Analogía com Disjuntor Elétrico**:
  - **Disjuntor**: Protege a casa contra sobrecargas elétricas interrompendo o circuito.
  - **Circuit Breaker de Software**: Interrompe requisições a um sistema problemático para evitar falhas maiores.
- **Operação Básica**:
  - **Circuito Fechado**: Requisições fluem normalmente entre os sistemas.
  - **Circuito Aberto**: Requisições são negadas e retornam erro 500 quando o sistema está sobrecarregado.
  - **Circuito Meio Aberto**: Permite um número limitado de requisições para verificar se o sistema se recuperou.

## Estados do Circuit Breaker
1. **Fechado (Closed)**:
   - Requisições são processadas normalmente.
   - Monitoramento contínuo das requisições e respostas.
2. **Aberto (Open)**:
   - Requisições são imediatamente negadas com erro 500.
   - Evita sobrecarga do sistema problemático.
3. **Meio Aberto (Half-Open)**:
   - Permite algumas requisições de teste.
   - Se as requisições forem bem-sucedidas, o circuito fecha novamente.
   - Caso contrário, o circuito permanece aberto.

## Implementação do Circuit Breaker
- **Autonomia no Código**:
  - Implementável diretamente no sistema com código personalizado.
  - Uso de bibliotecas específicas que facilitam a implementação.
- **Service Mesh**:
  - Implementação moderna onde o circuit breaker é gerenciado na camada de rede.
  - **Vantagens**:
    - Menor preocupação para desenvolvedores.
    - Configuração e ajuste via políticas de rede.

## Estratégia de Recuperação
- **Self Healing**:
  - O sistema tenta se recuperar automaticamente após falhas.
  - **Exemplo de Recuperação**:
    - Após um período de abertura, o circuito permite tentativas graduais de requisições.
    - A cada tentativa bem-sucedida, o tempo de espera para a próxima tentativa aumenta até que o sistema esteja completamente recuperado.

## Benefícios do Circuit Breaker
- **Prevenção de Efeito Dominó**:
  - Evita que falhas em um sistema se propaguem para outros sistemas.
- **Resiliência e Disponibilidade**:
  - Mantém a estabilidade do sistema durante picos de falhas.
- **Resposta Rápida a Falhas**:
  - Requisições não ficam esperando indefinidamente, melhorando a experiência do usuário.

## Conclusão
- **Importância**:
  - Essencial para arquiteturas de sistemas distribuídos e microservices.
- **Implementação Adequada**:
  - Pode ser realizada via código ou service mesh, dependendo das necessidades e infraestrutura.
- **Melhoria Contínua**:
  - Políticas de circuit breaker devem ser ajustadas conforme o sistema evolui para garantir máxima eficiência e resiliência.

