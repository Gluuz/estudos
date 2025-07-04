## A Origem do Docker

O Docker é uma ferramenta essencial no ecossistema de conteinerização, mas para entender completamente seu impacto, é importante conhecer sua história e origem. O Docker foi criado em 2013 pela empresa DotCloud, uma Platform as a Service (PaaS) que competia com soluções como o Heroku (posteriormente adquirido pela Salesforce). A proposta da DotCloud era oferecer aos desenvolvedores uma plataforma onde não precisavam se preocupar com a infraestrutura para rodar suas aplicações. Bastava fazer o upload e a aplicação começava a rodar.

Para viabilizar essa solução, a DotCloud desenvolveu o Docker. O lançamento do Docker Engine em 2013 causou grande repercussão, atraindo o interesse de empresas ao redor do mundo. O sucesso foi tão expressivo que a DotCloud decidiu pivotar suas operações, abandonando o modelo de PaaS e se renomeando como Docker Inc.

## Crescimento e Popularidade

O Docker nasceu para resolver uma necessidade interna da DotCloud, mas rapidamente se tornou uma ferramenta open source de relevância global. Hoje, com mais de uma década de história, o Docker se consolidou como uma solução madura e indispensável para conteinerização.

## Docker Community Edition vs Docker Desktop

Um ponto importante é a diferença entre Docker Community Edition (Docker Engine) e Docker Desktop:

- **Docker Community Edition (Docker Engine):** é open source, permitindo rodar containers sem custos.
- **Docker Desktop:** é um produto voltado para desenvolvimento, não sendo open source. Ele facilita o processo de desenvolvimento, mas não é voltado para DevOps, sendo uma das fontes de receita da Docker Inc.

## Arquitetura do Docker

O Docker aproveita recursos do próprio Linux para executar containers, integrando funcionalidades de forma eficiente. Componentes como RunC e ContainerD foram desmembrados e doados à Open Container Initiative (OCI). Esses componentes são runtimes de baixo nível para gerenciamento de containers, enquanto o Docker permanece como um runtime de alto nível, projetado para ser acessível e intuitivo.

## A Simplicidade do Docker

Antes do Docker, soluções como o LXC existiam, mas eram complexas. A grande sacada da DotCloud foi criar uma ferramenta de alto nível e fácil de usar. O Docker permite:

- Gerenciar containers;
- Trabalhar com redes e volumes;
- Criar builds de imagens reutilizáveis.

## Modelo Client-Server

O Docker opera no formato client-server. Quando você executa um comando Docker no terminal, está interagindo com a Command Line Interface (CLI), que age como cliente. Os comandos são enviados ao **docker-d**, o daemon responsável por gerenciar containers. Esse daemon centraliza o gerenciamento, mas também representa um **Single Point of Failure (SPOF)**. Se o daemon cair, todos os containers caem. Alternativas como o Podman resolvem essa questão ao permitir que cada container opere como um processo isolado.

## Considerações de Segurança

Por padrão, o Docker requer permissões de superusuário (root), o que pode expor vulnerabilidades. Para mitigar riscos, o Docker oferece um modo **rootless**, permitindo que containers sejam executados com o usuário local, eliminando a necessidade de privilégios elevados.
## Docker Inc. e Docker: Qual a Relação?

Embora o Docker seja uma ferramenta open source, ele está diretamente associado à Docker Inc. A Docker Inc. é uma empresa que tem como foco principal ajudar desenvolvedores a serem mais produtivos dentro do ecossistema de containers. A empresa divide esse ecossistema em dois círculos:

- **Inner Circle:** Focado no desenvolvimento de software.
    
- **Outer Circle:** Representa o momento em que algo é colocado em produção.
    

A Docker Inc. investe fortemente na melhoria do Docker Engine (Community Edition), além de desenvolver produtos como o Docker Desktop, que simplificam a vida do desenvolvedor e também são uma fonte de receita.

## O Ecossistema Docker

A Docker Inc. construiu um ecossistema robusto em torno do Docker, adquirindo empresas e projetos para expandir sua oferta de ferramentas e serviços. A empresa não está interessada apenas em rodar containers, mas em facilitar o gerenciamento e desenvolvimento de todo o ecossistema.

### Testcontainers

A Docker Inc. adquiriu a AtomicJar, responsável pelo Testcontainers, uma ferramenta que permite criar containers temporários para execução de testes end-to-end. Isso possibilita subir containers de dependências (como RabbitMQ ou bancos de dados) durante a execução dos testes, que são descartados ao final.

### Docker Desktop

O Docker Desktop é uma ferramenta essencial para rodar containers localmente, disponível em versão gratuita para uso pessoal. Para quem precisa de recursos avançados, há planos pagos (como o plano PRO) que oferecem funcionalidades adicionais, como sincronização de arquivos e armazenamento privado de imagens.

### Docker Hub

O Docker Hub é uma plataforma para armazenar e acessar imagens de containers. Além de imagens oficiais e verificadas, o Docker Hub oferece imagens mantidas por fornecedores confiáveis.

### Build Cloud

O Build Cloud permite o empacotamento rápido de aplicações em containers, integrando-se às esteiras de CI/CD e suportando multiplas arquiteturas (como ARM e AMD64).

### Docker Scout

Docker Scout é uma ferramenta focada em segurança, permitindo a análise de vulnerabilidades em imagens de containers. A ferramenta oferece insights detalhados sobre vulnerabilidades e orientações para corrigi-las.

### Docker AI

Uma iniciativa da Docker Inc. para rodar aplicações de AI de forma containerizada, facilitando a integração de workflows de machine learning.

## Aquisições Estratégicas

A Docker Inc. realizou diversas aquisições para expandir suas capacidades:

- **InfoSifter:** Especializada em segurança de imagens, contribuindo para o Docker Hub com Verified Publishers.
    
- **Tilt:** Focada em desenvolvimento com Kubernetes, permitindo deploys automáticos conforme o código é salvo.
    
- **AtomicJar (Testcontainers):** Permite rodar testes em containers de forma automatizada e integrada.
    
- **Mutagen.io:** Desenvolveu a tecnologia **Synchronized File Shares**, reduzindo latência no compartilhamento de arquivos entre containers e a máquina host.
    

## Kubernetes e Docker

Embora o Docker seja uma ferramenta poderosa, o Kubernetes se tornou o padrão para orquestração de containers em produção. A Docker Inc. continua investindo em ferramentas que facilitam o desenvolvimento com Kubernetes localmente.

---

A Docker Inc. não se limita a oferecer uma ferramenta de conteinerização. Ela está empenhada em construir um ecossistema que simplifique o desenvolvimento, testagem, empacotamento e segurança de aplicações containerizadas. Nos próximos passos, vamos abordar a instalação do Docker e configurações essenciais para iniciar na prática.