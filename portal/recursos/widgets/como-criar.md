{% include 'portal/aviso.md' %}

# Como criar um widget

Basta criar uma nova pasta dentro da pasta `widgets` com o seu nome. Em seguida, crie os quatro arquivos necessários:

- [`schema.json`](#-schema-json)
- [`settings.json`](#-settings-json)
- [`contents.json`](#-contents-json)
- [`body.html`](#-body-html)

## `schema.json`
Define o schema das configurações que o widget recebe.

Exemplo:
```json
{
  "settings": [
    {
      "name": "caption",
      "type": "string"
    },
    {
      "name": "description",
      "type": "string"
    }
  ],
  "contents": [
    {
      "name": "size",
      "type": "int"
    }
  ]
}
```

Cada configuração é definida no objeto `settings` composto de duas propriedades: `name` e `type`, que define o nome e o tipo respectivamente. O nome será usado no template (leia sobre o `body.html` abaixo), enquanto o tipo restringe os valores do campo. Os tipos suportados são: "string" e "int".

O objeto `contents` define o nome e o tipo dos valores que serão determinados pelo arquivo [`rules.json`](../rules.json.html)

## `settings.json`
Este arquivo determina os valores padrões da configuração do widget. Ele deve respeitar o schema definido no arquivo `schema.json`.

Exemplo:
```json
{
  "caption": "Some image",
  "description": "Non descriptive description."
}
```

## `contents.json`
Define os valores padrões do conteúdo do widget. Ele deve respeitar o schema definido no arquivo `schema.json`. O conteúdo do widget pode variar de acordo com as regras e condições que foram definidas no arquivo [`rules.json`](../rules.json.html).

```json
{
  "google": {
    "size": 3
  },
  "default": {
    "size": 2
  }
}
```

## `body.html`
Este arquivo de template será inserido no lugar da tag, como se fosse chamado através da tag [`include`](../../referencias/liquid/basic/tags.html). Porém, esse template tem acesso a uma variável nova, chamada `widget`.

A variável `widget` possui o objeto `settings` que tem os valores das configurações do widget.

Exemplo do arquivo `body.html`:
```
<h1>{{ widget.settings.caption }}</h1>
<small>{{ widget.settings.description }}</small>
```

Ela também possui o objeto contents com as chaves das regras aplicadas (regras que foram determinadas pelo arquivo [`rules.json`](../rules.json.html)).

Exemplo do arquivo `body.html`:
{% raw %}<pre>
{% assign rules = activeRules | append: "default" %}
{% with content = widget.contents[rules[0]] %}
Em até {{ content.size }}x sem juros!
{% endwith %}
</pre>{% endraw %}

É importante definir um `title` ao widget, ele será usado como um nome amigável na interface administrativa. Para isso, basta inserir um [front matter](../templates/front-matter.html) com a propriedade `title`.

Exemplo do arquivo `body.html`:
```
---
title: Legenda de imagem
---
<h1>{{ widget.settings.caption }}</h1>
<small>{{ widget.settings.description }}</small>
```
