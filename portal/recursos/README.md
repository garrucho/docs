{% include 'portal/aviso.md' %}

# Recursos

Uma loja e uma app tem a mesma e a seguinte estrutura de pastas:

```sh
.
├── storefront
│   ├── assets
│   ├── models
│   ├── pages
│   ├── partials
│   ├── widgets
│   ├── libs.json
│   ├── routes.json
│   └── rules.json
└── meta.json
```

## Assets
É a pasta onde ficam todos os arquivos estáticos: imagens, CSS, Javascript, etc.

[Leia mais sobre assets.](assets.html)

## Models
Um model é um arquivo JSON que define uma chamada a uma API.

[Leia mais sobre models.](models.html)

## Pages
As `pages` são templates mestres, ou seja, as rotas definidas no arquivo [`routes.json`](../routes.json.html) devem referenciar uma página.

[Leia mais sobre pages.](templates/pages.html)

## Partials
Uma `partial` é um trecho de template que pode ser reutilizado.

[Leia mais sobre partials.](templates/partials.html)

## Widgets
Um `widget` é uma forma fácil de fazer com que partes do seu site seja alterado através da interface administrativa.

[Leia mais sobre widgets.](widgets/index.html)

## `libs.json`
O arquivo `libs.json` é um arquivo em JSON que define quais as versões das bibliotecas front end estão sendo usadas.

[Leia mais sobre `libs.json`.](libs.json.html)

## `routes.json`
Este arquivo define para quais [pages](templates/pages.html) as URLs serão apontadas e fornece modelos, que utilizam a URL para serem gerados, para elas.

[Leia mais sobre `routes.json`.](routes.json.html)

## `rules.json`
[Leia mais sobre `rules.json`.](rules.json.html)