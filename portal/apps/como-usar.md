{% include 'portal/aviso.md' %}

# Como usar uma app

Após [instalada](como-instalar.html) para usar um recurso da app siga os exemplos.

## Widget

Use o seguinte formato:
```
{% widget 'nomedowidget@owner.appname' id:'usadonoadmin' %}
```

Exemplo:
```
{% widget 'menu@vtex.dreamstore-menu' id:'mainmenu' %}
```

## Template

Use o seguinte formato:
```
{% include 'nomedapartial@owner.appname' %}
```

Exemplo:
```
{% include 'header@vtex.dreamstore-theme' %}
```

## Rotas
Caso a app tenha um arquivo `routes.json`, ele será anexado ao `routes.json` da sua loja.

Exemplo de uma app que define um arquivo `routes.json`:
```
{
  "/categories": {
    "page": "categories@vtex.dreamstore-menu"
  }
}
```

Note que foi usado o sufixo `@vtex.dreamstore-menu` da mesma maneira que é usado nos widgets e templates.

## Category
Uma app pode definir para qual categoria uma partial foi feita.

Exemplo de uma partial de uma app:
```
---
category: product
---
restante do conteúdo
```

Para incluir todas as partials da categoria "product" basta escrever no seu template:
```
{% for partial in categories.product.partials %}
  {% include partial %}
{% endfor %}
```
