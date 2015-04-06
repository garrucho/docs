# Pages

As `pages` são templates mestres, ou seja, as rotas definidas no arquivo [`routes.json`](../routes.json.html) devem referenciar uma página.

Exemplo:

Arquivo `routes.json`:
```json
{
  "/": {
    "page": "home"
  }
}
```

Página `home.html`:
```html
<html>
  <head>
    <title>{{ accountName }}</title>
  </head>
  <body>
    Olá, mundo!
  </body>
</html>
```

Note que devido a página ser um template, ela pode utilizar a linguagem de template [Liquid (leia mais sobre Liquid)](../../referencias/liquid/index.html).
