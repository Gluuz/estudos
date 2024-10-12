## Acessibilidade
- **Público-alvo**: Entender quem vai acessar sua aplicação é fundamental, especialmente em aplicações com foco no Front End.
- **Bibliotecas e Padrões**: Existem várias ferramentas que ajudam a tornar a aplicação mais acessível para pessoas com deficiências visuais, auditivas, ou outras comorbidades.
- **Movimento de Acessibilidade**: A acessibilidade está ganhando cada vez mais importância, e é um aprendizado necessário para todos os desenvolvedores.

## Retenção e Recuperação de Dados
- **Armazenamento**: Dados são críticos, mas o armazenamento pode ser caro. Soluções como Kafka permitem definir o tempo de retenção de dados (ex: 7 ou 30 dias).
- **Compactação e Storage Barato**: Dados antigos podem ser compactados e movidos para armazenamento mais barato (ex: ElasticSearch, Prometheus).
- **Dados Críticos**: Dados utilizados com frequência podem ficar em bancos mais acessíveis, enquanto dados antigos ou de compliance podem ser mantidos em locais mais baratos e com menor precisão.

## Autenticação e Autorização
- **Arquiteturas Distribuídas**: Utilizar um **Identity Provider** para gerenciar autenticação entre microsserviços.
- **API Gateway**: Lidar com autenticação, controle de requisições, e timeouts antes de as requisições chegarem aos microsserviços.
- **Autenticação em Monolíticos**: Sistemas monolíticos são mais simples, pois bons frameworks de autenticação e autorização já resolvem o problema.

## Conformidade Legal e Privacidade
- **LGPD**: É importante entender como minimizar riscos relacionados a vazamento de dados de usuários.
- **Isolamento de Dados Sensíveis**: Separar dados sensíveis em bancos diferentes, potencialmente criptografados, para evitar riscos de segurança.
- **Contratos de Privacidade**: Empresas estão cada vez mais exigindo conformidade com contratos rígidos de privacidade e respondendo legalmente por isso.

## Segurança
- **Segurança desde a Borda**: Utilize firewalls, regras de segurança e detecção de robôs para proteger a aplicação desde a borda.
- **Adoção de Padrões Abertos**: Sempre utilize padrões abertos para criptografia, autenticação, e segurança. Não tente reinventar a roda.
- **Separação de Bancos e Backups**: Mantenha o banco de dados separado da aplicação, e garanta que backups estejam em redes diferentes.

## Usabilidade
- **Não é só Front End**: A usabilidade vai além do Front End. API’s também devem ser bem documentadas, com padrões claros e fáceis de usar (ex: OpenAPI).
- **Experiência do Cliente**: Pense em quem vai utilizar sua aplicação, seja um usuário final ou outra aplicação. Ofereça a melhor experiência possível, seja no Front ou Back End.
