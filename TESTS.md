# Testes automatizados

Aqui não me estenderei explicando a importância de testes, mas indicarei algumas ferramentas para o desenvolvimento utilizando-os.

[Indicação de artigo explicando a importância de testes](https://www.devmedia.com.br/a-importancia-dos-testes-para-a-qualidade-do-software/28439).

## Dicas

1. Tente desenvolver o máximo de testes possíveis, mas principalmente para regras de negócio específicas de um projeto.
2. Escreva os testes antes da implementação. Desenvolvendo o teste antes, e ele falhando, faz a solução ser desenvolvida. Do contrário, você pode acabar por desenvolver os testes baseados no que foi desenvolvido podendo conter falhas.
3. Tente manter os testes o mais limpos possíveis, assim podem servir também como uma documentação para outros desenvolvedores ou para você mesmo quando não se lembrar como utilizar uma parte do código.
4. Foque em testes relacionados ao backend de seu projeto. O QA já estará realizando testes utilizando o frontend e poderá identificar as falhas. No backend isso complica um pouco.

## Ferramentas

### JavaScript

Para JavaScript, a ferramenta padrão de testes será o [Jest](https://jestjs.io).

1. [Testando aplicações express com Jest + Supertest](https://www.albertgao.xyz/2017/05/24/how-to-test-expressjs-with-jest-and-supertest/)
2. [Testando aplicações NestJS com Jest + Supertest](https://docs.nestjs.com/fundamentals/testing)
3. [Testando aplicações AdonisJS com Jest + Supertest](https://docs.adonisjs.com/cookbooks/testing-adonisjs-apps)
4. [Testando aplicações React com Jest + React Testing Library](https://blog.rocketseat.com.br/introducao-a-testing-library-testando-componentes-react/)

### PHP

1. [Testando aplicações Laravel com PHPUnit](https://laravel.com/docs/9.x/testing)
2. [Testando aplicações Laravel com Pest](https://nunomaduro.com/using-pest-in-laravel/) - Excelente biblioteca construída em cima do PHPUnit que permite a utilização de uma sintaxe parecida a de frameworks de testes JS.
