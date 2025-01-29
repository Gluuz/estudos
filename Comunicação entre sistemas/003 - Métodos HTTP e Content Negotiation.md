
## **Parte 1: Métodos HTTP**

### **1. O Que São Métodos HTTP?**
Os métodos HTTP são fundamentais para definir a intenção de uma requisição em um sistema RESTful. Quando um cliente (ex: frontend ou outro sistema) faz uma requisição a um servidor, ele especifica o método para indicar qual ação deseja realizar.

| Método  | Ação                                          | Idempotência      | Cacheável?       |
|---------|-----------------------------------------------|-------------------|------------------|
| **GET**  | Recupera uma representação de um recurso     | Idempotente       | Sim              |
| **POST** | Envia dados ao servidor (criação de recurso) | Não idempotente   | Não              |
| **PUT**  | Atualiza um recurso existente completamente  | Idempotente       | Não              |
| **PATCH**| Atualiza parcialmente um recurso             | Idempotente (idealmente) | Não       |
| **DELETE** | Remove um recurso                         | Idempotente       | Não              |
| **OPTIONS** | Informa sobre os métodos disponíveis     | Idempotente       | Não              |

---

### **2. Detalhando Cada Método**
- **GET:** Usado para consultas. Não altera o estado no servidor. Deve ser cacheável.
  - Exemplo: `GET /api/users/1` retorna as informações do usuário de ID 1.
  
- **POST:** Utilizado para criar novos recursos. Pode resultar em efeitos colaterais, como persistência de dados.
  - Exemplo: `POST /api/users` com o corpo da requisição contendo os dados do usuário.

- **PUT:** Atualiza todo o recurso, mesmo que apenas um campo tenha mudado.
  - Exemplo: `PUT /api/users/1` atualiza todas as informações do usuário 1.
  
- **PATCH:** Atualiza partes específicas de um recurso.
  - Exemplo: `PATCH /api/users/1` apenas para modificar o e-mail.
  
- **DELETE:** Remove um recurso do servidor.
  - Exemplo: `DELETE /api/users/1` remove o usuário de ID 1.

- **OPTIONS:** Descobre quais métodos o servidor suporta.
  - Exemplo: `OPTIONS /api/users` retorna `GET`, `POST`, `DELETE`.

---

### **3. Boas Práticas no Uso dos Métodos HTTP**

- **Usar métodos corretamente:** Não usar `GET` para criar dados.
- **Respeitar idempotência:** *PUT*, *PATCH* e *DELETE* devem garantir que várias execuções tenham o mesmo resultado.
- **Versionamento da API:** Pode ser implementado através de caminhos (ex: `/v1/api/resource`) ou cabeçalhos.

---

## **Parte 2: Content Negotiation**

### **1. O Que é Content Negotiation?**

A *content negotiation* é o processo em que o cliente e o servidor concordam sobre qual formato de dados será usado na resposta. Isso é importante em sistemas distribuídos, onde diferentes clientes (navegadores, sistemas móveis, serviços de backend) podem precisar de formatos distintos.

---

### **2. Como Funciona o Content Negotiation?**

O processo de negociação é controlado através dos cabeçalhos da requisição HTTP:

- **Accept:** Especifica os formatos de resposta aceitos pelo cliente.
  - Exemplo: `Accept: application/json, application/xml`
  
- **Content-Type:** Define o formato do corpo da requisição enviado pelo cliente.
  - Exemplo: `Content-Type: application/json`
  
---
# **Exemplos Práticos de Content Negotiation**

## **1. Negociação de Formato de Dados**

### **Requisição para JSON**
```http
GET /api/products HTTP/1.1
Accept: application/json
```

### **Resposta (em JSON)**
```json
[
  {
    "id": 1,
    "name": "Produto A",
    "price": 50.0
  }
]
```

### **Requisição para XML**
```http
GET /api/products HTTP/1.1
Accept: application/xml
```

### **Resposta (em XML)**
```xml
<products>
  <product>
    <id>1</id>
    <name>Produto A</name>
    <price>50.0</price>
  </product>
</products>
```

---

## **2. Negociação de Linguagem**

### **Requisição**
```http
GET /api/products HTTP/1.1
Accept-Language: en-US
```

### **Resposta (em Inglês)**
```json
{
  "message": "Welcome to our API"
}
```

---

## **3. Implementando Content Negotiation em APIs RESTful**

No **FastAPI**, você pode implementar a *Content Negotiation* de forma simples usando cabeçalhos e dependências.

### **Exemplo de Implementação**
```python
from fastapi import FastAPI, Request, HTTPException
from fastapi.responses import JSONResponse, PlainTextResponse

app = FastAPI()

@app.get("/items")
async def get_items(request: Request):
    accept = request.headers.get("accept", "")

    if "application/json" in accept:
        return JSONResponse(content={"message": "Data in JSON"})
    elif "text/plain" in accept:
        return PlainTextResponse(content="Data in plain text")
    else:
        return JSONResponse(
            content={"error": "Unsupported format"},
            status_code=406
        )
```
# **Parte 3: Boas Práticas no Content Negotiation**

## **1. Retorne códigos de status adequados**
- Se o formato solicitado não for suportado, retorne **406 Not Acceptable**.

## **2. Documente os formatos suportados**
- Deixe claro na documentação da API quais formatos de resposta são suportados (ex: JSON, XML).

## **3. Defina um padrão de fallback**
- Se o cliente não especificar um formato, utilize um padrão, como **application/json**.

---

# **Parte 4: Cenário Prático**

Imagine que você está desenvolvendo uma API para um e-commerce que precisa atender diferentes tipos de clientes:

- **Aplicativos móveis:** Preferem respostas em JSON.
- **Parceiros de integração:** Podem exigir respostas em XML.

## **A API deve:**
- Utilizar os métodos HTTP corretos para cada operação.
- Responder no formato adequado para cada cliente.

---

## **Desafio Prático**

### **Objetivos:**
- Use **GET** para buscar produtos.
- Use **POST** para adicionar produtos.
- Suporte Content Negotiation entre **JSON** e **XML**.

### **Dica:**
- Para converter JSON em XML, utilize bibliotecas como `xmltodict`.

### **Exemplo de Implementação em Python (FastAPI)**
```python
from fastapi import FastAPI, HTTPException
from fastapi.responses import JSONResponse, Response
import xml.etree.ElementTree as ET

app = FastAPI()

# Dados de exemplo
products = [
    {"id": 1, "name": "Notebook", "price": 2500.0},
    {"id": 2, "name": "Mouse", "price": 150.0}
]

def dict_to_xml(data):
    root = ET.Element("products")
    for item in data:
        product = ET.SubElement(root, "product")
        for key, value in item.items():
            child = ET.SubElement(product, key)
            child.text = str(value)
    return ET.tostring(root, encoding="unicode")

@app.get("/products")
async def get_products(accept: str = "application/json"):
    if "application/json" in accept:
        return JSONResponse(content=products)
    elif "application/xml" in accept:
        xml_data = dict_to_xml(products)
        return Response(content=xml_data, media_type="application/xml")
    else:
        raise HTTPException(status_code=406, detail="Formato não suportado.")
```

---

# **Conclusão**

- **Métodos HTTP:** Padronizam a comunicação e definem a intenção das operações.
- **Content Negotiation:** Permite que sistemas distribuídos troquem dados no formato adequado.
- **Boas práticas:** Melhoram a escalabilidade, manutenção e usabilidade das APIs.
