# Documentação de APIs

É extremamente importante possuir uma documentação de API quando o projetos é desenvolvido em equipes com separação de frontend e backend.

Hoje, muitos desenvolvedores disponibilizam um arquivo do Postman para que o desenvolvedor frontend utilize como base para suas implementações. O grande problema disso é a dificuldade de identificar todas as possibilidades de um endpoint, tipagem de dados, obrigatoriedade ou método de autenticação.

Para padronizarmos isso, devemos elaborar a documentação de APIs. A biblioteca mais recomendada para isso hoje, que pode ser desenvolvida de forma simples, é o [Swagger](https://swagger.io/).

Com o Swagger, basicamente você deve ter um arquivo de [configuração yaml](https://swagger.io/docs/specification/basic-structure/) que descreve as funcionalidades de sua API, entidades, detalhes de autenticação, URLs e endpoints, e é gerado uma documentação visual e interativa. Para implantá-lo existem diversas possibilidades: geração de uma aplicação React, decorators que geram o yaml de configuração, plugins para o express e muito mais.

## Ferramentas interessantes

1. [Swagger Petstore](https://petstore.swagger.io/) - Exemplo na prática de uma API gerada com Swagger, inclusive com arquivo de configuração disponibilizado. Excelente para servir como base para sua implementação.
2. [Swagger Editor](https://editor.swagger.io/) - Edição e visualização em tempo real de configurações.
