# Configuração de linters

### TODO

1. [x] [ESLint (com TypeScript/React)](https://github.com/gabrieljmj/devio-dev-doc/blob/main/LINTERS.md#eslint-com-typescriptreact)
2. [x] [ESLint (com TypeScript)](https://github.com/gabrieljmj/devio-dev-doc/blob/main/LINTERS.md#eslint-com-typescript)
3. [x] [EditorConfig](https://github.com/gabrieljmj/devio-dev-doc/blob/main/LINTERS.md#editorconfig)
4. [ ] PHPCSFixer

## ESLint (com TypeScript/React)

Será usado, como base, o padrão criar pelo AirBnb, porém com modificações, desabilitando ou modificando algumas regras.

### Regras desabilitadas

1. Obrigatoriedade de passar dependências em hooks do React
2. Obrigatoriedade de utilização de ```PropTypes```. Quando se trabalha com TypeScript isso não se torna necessário.
3. Obrigatoriedade de um ```export default``` em arquivos.
4. Obrigatoriedade de declarar variáveis com nomes diferentes quando existe uma variável com mesmo nome no escopo que as contém.
5. Obrigatoriedade em não utilizar spreading operator em propriedades de componentes.
6. Não utilização de global importa. Necessário para alguns módulos como ```date-fns``` para i18n (internacionalização).

### Instalação

Instalação da configuração do AirBnb:

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

É interessante ter instalado no VSCode a
[extensão do ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) também
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

Segue os mesmos princípios da configuração com React, porém com algumas modificações para remover plugins do ESLint relacionadois ao framework:

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
        "ecmaFeatures": {
            "jsx": true
        },
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

Uma ferramente legal de se usar é o EditorConfig. É basicamente um arquivo que define algumas configurações do seu editor de código para lidar com formação dos arquivos. Excelente para projetos onde os desenvolvedores possuem configurações diferentes por padrões em seus editores ou utilizam editores diferentes. Hoje, a grande maioria dos editores possuem essa integração e, dependendo, não é necessário utilizar nenhum plugin/extensão.

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
