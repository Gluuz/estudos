## O que é Proxy Reverso?

O **proxy reverso** é um servidor que atua como intermediário entre clientes (como navegadores web) e servidores web. Ele recebe as solicitações dos clientes e, com base em regras predefinidas, redireciona essas solicitações para os servidores adequados que estão por trás dele.

## Diferença entre Proxy e Proxy Reverso

- **Proxy normal**:
  - Serve como intermediário entre o usuário e a internet.
  - A requisição sai do usuário, passa pelo proxy e vai para o servidor da internet (ex: um site).
  - O proxy, muitas vezes, tem o papel de **filtrar** requisições, bloquear sites indesejados ou monitorar o uso da rede.
  - Exemplo: Usuários de uma empresa usam um proxy para acessar a web, e esse proxy pode impedir o acesso a sites não permitidos.

- **Proxy reverso**:
  - Atua entre os **servidores web** e os clientes.
  - Quando um cliente faz uma requisição a um site, o proxy reverso a redireciona para o servidor correto com base em regras configuradas.
  - Exemplo: O cliente acessa um site, como **a.com.br**, e o proxy reverso decide qual servidor vai atender essa requisição, dependendo da URL ou outros fatores.

Em resumo:
- O **proxy** tradicional filtra e gerencia as requisições dos **clientes para os servidores**.
- O **proxy reverso** filtra e redireciona as requisições dos **clientes para diferentes servidores web**.

## Exemplo prático

Imagine que você esteja acessando o site **a.com.br**. O proxy reverso, ao receber essa requisição, identifica que a URL corresponde a um determinado servidor e redireciona a solicitação para o servidor correto. Se outra pessoa acessar **b.com.br**, o proxy reverso redirecionará para um servidor diferente, conforme configurado.

## Papel do Proxy Reverso

O proxy reverso desempenha várias funções:
1. **Distribuição de requisições**: Redireciona as requisições para diferentes servidores com base nas regras configuradas.
2. **Leitura de dados HTTP**: Analisa cabeçalhos e dados HTTP para tomar decisões sobre roteamento.
3. **Load Balancing**: Embora não seja o mesmo que um load balancer, um proxy reverso pode, em alguns casos, atuar como tal, redirecionando requisições para servidores com a mesma aplicação para balancear a carga.

## Soluções de Proxy Reverso

Aqui estão três soluções populares de proxy reverso, especialmente no mundo open source:

1. **Nginx**: Pronunciado como "engine X", é uma das soluções mais utilizadas e confiáveis para configurar proxy reverso. É amplamente adotado e pode servir como base para outras ferramentas.
   
2. **HAProxy**: Focado em **alta disponibilidade**, oferece um proxy robusto para lidar com grandes volumes de tráfego e garantir que as requisições sejam processadas mesmo em caso de falhas.

3. **Traefik**: Uma solução moderna que se integra bem em ambientes de microserviços, facilitando a configuração de proxy reverso.

## Considerações Finais

Aprender a trabalhar com o **Nginx** é uma habilidade essencial para quem lida com servidores web. Muitas outras soluções, como **Kong** e **API Gateways**, são baseadas no Nginx, então ter um bom entendimento dessa ferramenta pode facilitar a vida ao configurar proxies reversos.
O proxy reverso é uma peça-chave na arquitetura de escalabilidade e performance, ajudando a distribuir requisições de maneira eficiente e segura.
