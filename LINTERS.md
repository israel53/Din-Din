# Configuração de linters

### Índice

1. [ESLint (com TypeScript/React)](https://github.com/deviobr/code-patterns/blob/main/LINTERS.md#eslint-com-typescriptreact)
2. [ESLint (com TypeScript)](https://github.com/deviobr/code-patterns/blob/main/LINTERS.md#eslint-com-typescript)
3. [EditorConfig](https://github.com/deviobr/code-patterns/blob/main/LINTERS.md#editorconfig)
4. [PHPCSFixer](https://github.com/deviobr/code-patterns/blob/main/LINTERS.md#php-coding-standards-fixer)
5. [CommitLint](https://github.com/deviobr/code-patterns/blob/main/LINTERS.md#commitlint)

## ESLint (com TypeScript/React)

Será usado, como base, o padrão criado pelo Airbnb, porém com modificações, desabilitando e modificando algumas regras.

### Regras desabilitadas

1. Obrigatoriedade de passar dependências em hooks do React
2. Obrigatoriedade de utilização de ```PropTypes```. Quando se trabalha com TypeScript isso não se torna necessário.
3. Obrigatoriedade de um ```export default``` em arquivos.
4. Obrigatoriedade de declarar variáveis com nomes diferentes quando existe uma variável com mesmo nome no escopo que as contém.
5. Obrigatoriedade em não utilizar spreading operator em propriedades de componentes.
6. Não utilização de global imports. Necessário para alguns módulos como ```date-fns``` para i18n (internacionalização).

### Instalação

Instalação da configuração do Airbnb:

```console
npx install-peerdeps --dev eslint-config-airbnb
```

Instalação do restante das dependências:
```console
npm i typescript @typescript-eslint/eslint-plugin @typescript-eslint/parser prettier eslint-config-prettier eslint-plugin-prettier -D
```

### Configuração ESLint

```.eslintrc.json```
```json
{
    "extends": [
        "airbnb",
        "airbnb/hooks",
        "plugin:@typescript-eslint/recommended",
        "prettier",
        "plugin:prettier/recommended"
    ],
    "plugins": ["@typescript-eslint", "react", "prettier"],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": 2018,
        "sourceType": "module",
        "project": "./tsconfig.json"
    },
    "rules": {
        "import/no-unresolved": 0,
        "react/jsx-filename-extension": [1, {
        "extensions": [
          ".ts",
          ".tsx"
        ]
        }],
        "prettier/prettier": [
            "error",
            {
                "singleQuote": true,
                "trailingComma": "all",
                "arrowParens": "avoid",
                "endOfLine": "auto"
            }
        ],
        "no-use-before-define": "off",
        "@typescript-eslint/no-use-before-define": ["error"],
        "import/extensions": ["error", "never"],
        "react/prop-types": 0,
        "no-shadow": "off",
        "@typescript-eslint/no-shadow": ["error"],
        "react/require-default-props": 0,
        "react/jsx-props-no-spreading": 0,
        "@typescript-eslint/explicit-module-boundary-types": 0,
        "react-hooks/exhaustive-deps": 0,
        "import/prefer-default-export": 0,
        "global-require": 0,
        "react/style-prop-object": 0
    }
}
```

### Extensão de VSCode

É necessário também ter instalado no VSCode a
[extensão do ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
e habilitar essas configurações para que, ao salvar um arquivo, o ESLint já o corrija:

```json
{
    "eslint.format.enable": true,
    "eslint.packageManager": "yarn",
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "dbaeumer.vscode-eslint",
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true,
        "source.organizeImports": true
    },
    "javascript.updateImportsOnFileMove.enabled": "always",
    "eslint.alwaysShowStatus": true
}
```

## ESlint (com TypeScript)

Segue os mesmos princípios da configuração com React, porém com algumas modificações para remover plugins do ESLint relacionados ao framework:

```json
{
    "extends": [
        "airbnb",
        "airbnb/hooks",
        "plugin:@typescript-eslint/recommended",
        "prettier",
        "plugin:prettier/recommended"
    ],
    "plugins": ["@typescript-eslint", "prettier"],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaVersion": 2018,
        "sourceType": "module",
        "project": "./tsconfig.json"
    },
    "rules": {
        "import/no-unresolved": 0,
        "prettier/prettier": [
            "error",
            {
                "singleQuote": true,
                "trailingComma": "all",
                "arrowParens": "avoid",
                "endOfLine": "auto"
            }
        ],
        "no-use-before-define": "off",
        "@typescript-eslint/no-use-before-define": ["error"],
        "import/extensions": ["error", "never"],
        "no-shadow": "off",
        "@typescript-eslint/no-shadow": ["error"],
        "@typescript-eslint/explicit-module-boundary-types": 0,
        "import/prefer-default-export": 0,
        "global-require": 0
    }
}
```

## [EditorConfig](https://editorconfig.org/)

Uma ferramenta legal de se usar é o EditorConfig. É basicamente um arquivo que define algumas configurações do seu editor de código para lidar com formação dos arquivos. Excelente para projetos onde os desenvolvedores possuem configurações diferentes por padrões em seus editores ou utilizam editores diferentes. Hoje, a grande maioria dos editores possuem essa integração e, dependendo, não é necessário utilizar nenhum plugin/extensão.

### Configuração padrão

```yaml
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
indent_style = space
trim_trailing_whitespace = true

[*.md]
trim_trailing_whitespace = false

[*.{tsx,jsx,ts,js}]
indent_size = 2

[*.php]
indent_size = 4
```

## PHP Coding Standards Fixer

Esta ferramenta é utilizada para manter um padrão nos códigos PHP. O bacana é que possui algumas configurações já predefinidas que permitem utilizar os padrões como os de PSRs.

### Instalação

Instale-a via [Composer](https://getcomposer.org):

```console
composer require friendsofphp/php-cs-fixer --dev
```

### Configuração padrão

Crie um arquivo, no diretório ```tools/php-cs-fixer``` chamado ```.php-cs-fixer.php```:

```php
<?php

$finder = PhpCsFixer\Finder::create()
    ->exclude(['vendor'])
    ->in(__DIR__)
;

$config = new PhpCsFixer\Config();
return $config->setRules([
        '@PSR12' => true,
        'array_syntax' => ['syntax' => 'short'],
        'function_typehint_space' => true,
        'magic_method_casing' => true,
        'magic_constant_casing' => true,
        'native_function_casing' => true,
        'no_blank_lines_after_phpdoc' => true,
        'no_unneeded_curly_braces' => true,
        'no_useless_return' => true,
        'standardize_not_equals' => true,
        'trim_array_spaces' => true,
        'visibility_required' => ['elements' => [
            'property',
            'method',
        ]],
        'yoda_style' => true,
        'whitespace_after_comma_in_array' => true,
        'trailing_comma_in_multiline' => true,
    ])
    ->setFinder($finder)
;
```

Após isso, configure, em seu ```composer.json```, comandos para rodá-lo em seu projeto:

```json
{
    "scripts": {
        "check-style": "php-cs-fixer fix --diff --verbose --dry-run .",
        "fix-style": "php-cs-fixer fix ." 
    }
}
```
1. ```check-style```: é descrito as verificações que foram feitas, mas nenhuma correção é aplicada
2. ```fix-style```: aplica correções

Para rodar os comandos, utilize o Composer:

```console
composer check-style
composer fix-style
```

## CommitLint

O CommitLint é uma ferramente para validarmos a estrutura de nosso commit. Para isso se tornar automático, utilizaremos um hook em nosso git para, assim que tentarmos criar um commit, essa validação seja feita de forma automática.

### Instalação do CommitLint

Primeiro é necessário instalar as dependências:

```console
yarn add @commitlint/config-conventional @commitlint/cli -D
```

e depois criar o arquivo de configuração (```commitlint.config.js```) com esse conteúdo:

```js
module.exports = {
  extends: ['@commitlint/config-conventional'],
};
```

### Instalação do Husky

```console
yarn add husky -D
```

Após isso, é necessário rodar o script de instalação do hook:

```console
yarn husky install
```

e, para que isso ocorra de forma automática após a instalação do projeto, adicione o seguinte script no ```package.json```:

```json
"scripts": {
  "prepare": "husky install"
}
```

agora, para o hook ser instalado:

```console
yarn husky add .husky/commit-msg 'yarn commitlint --edit $1'
```

Com isso, assim que um ```git commit -m "..."``` for rodado, a mensagem será validada e indicado os pontos que não seguem o padrão exigido.
