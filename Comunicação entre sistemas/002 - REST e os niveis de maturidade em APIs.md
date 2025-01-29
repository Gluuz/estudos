# REST e os Níveis de Maturidade em APIs

O tema central é o entendimento aprofundado do REST (Representational State Transfer) e seus níveis de maturidade em APIs. Embora muitos desenvolvedores estejam familiarizados com o uso básico do REST, que envolve organizar recursos e selecionar os verbos HTTP corretos, há aspectos mais avançados essenciais para a construção de APIs eficientes e padronizadas.

REST foi introduzido em 2000 por Roy Fielding em sua tese de doutorado. Sua principal proposta é a simplicidade e a padronização na comunicação entre sistemas, aproveitando o protocolo HTTP. Uma característica fundamental do REST é ser **stateless**, ou seja, não manter estado entre as requisições. Isso significa que cada requisição é independente, e informações de autenticação, como tokens, devem ser enviadas em cada chamada.

## Níveis de Maturidade do REST

O modelo de maturidade de Richardson divide o REST em quatro níveis, que refletem o grau de aderência aos princípios RESTful:

### Nível 0 – Swamp of POX

Neste nível básico, as requisições HTTP são utilizadas para realizar transações, mas não há padronização ou uso correto dos recursos e verbos HTTP. As operações podem ser procedurais, semelhantes a chamadas de procedimento remoto (RPC), sem seguir os padrões REST.

**Exemplo:**

- **Endpoint:** `POST /api`
- **Descrição:** Todas as operações são enviadas para um único endpoint, como `/api`, e a ação desejada é especificada no corpo da requisição.
- **Uso:** Para criar um usuário, atualizar ou deletar, o cliente envia uma requisição `POST` para `/api` com um payload como `{ "action": "createUser", "data": {...} }`.

### Nível 1 – Recursos (Resources)

Aqui, começa-se a utilizar recursos identificados por URIs, representando substantivos como "produtos" ou "usuários". As operações ainda podem não utilizar corretamente os verbos HTTP, mas há uma estruturação dos endpoints baseada em recursos.

**Exemplo:**

- **Endpoints:**
  - `POST /users/getUser`
  - `POST /users/createUser`
- **Descrição:** Os recursos são separados por URIs significativas, mas os verbos HTTP não são utilizados de forma apropriada. As ações são especificadas nos próprios endpoints.
- **Uso:** Para obter um usuário, o cliente faz uma requisição `POST` para `/users/getUser` com o ID do usuário no corpo.

### Nível 2 – Verbos HTTP e Códigos de Status

Neste nível, além dos recursos, os verbos HTTP são empregados corretamente: `GET` para recuperar dados, `POST` para criar, `PUT` para atualizar e `DELETE` para remover. Também são utilizados códigos de status HTTP apropriados, melhorando a comunicação entre cliente e servidor.

**Exemplo:**

- **Endpoints:**
  - `GET /users/123` – Recupera o usuário com ID 123.
  - `POST /users` – Cria um novo usuário.
  - `PUT /users/123` – Atualiza o usuário com ID 123.
  - `DELETE /users/123` – Remove o usuário com ID 123.
- **Descrição:** As operações são claramente definidas pelos verbos HTTP, e os recursos são identificados por URIs significativas.
- **Códigos de Status:** Uso adequado de códigos como 200 OK, 201 Created, 404 Not Found, etc.

### Nível 3 – HATEOAS (Hypermedia as the Engine of Application State)

É o nível mais avançado, onde as respostas da API incluem hipermídia (links) que descrevem as ações disponíveis. Isso torna a API autoexplicativa e navegável, permitindo que o cliente descubra dinamicamente as operações que pode realizar a seguir.

**Exemplo:**

- **Requisição:** `GET /accounts/123`
- **Resposta:**

  ```json
  {
    "accountId": 123,
    "balance": 1000.00,
    "links": [
      { "rel": "deposit", "href": "/accounts/123/deposit", "method": "POST" },
      { "rel": "withdraw", "href": "/accounts/123/withdraw", "method": "POST" },
      { "rel": "transfer", "href": "/accounts/123/transfer", "method": "POST" },
      { "rel": "close", "href": "/accounts/123", "method": "DELETE" }
    ]
  }
  

