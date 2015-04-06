# Como criar um widget

Basta criar uma nova pasta dentro da pasta `widgets` com o seu nome. Em seguida, crie os três arquivos necessários: 

- [`schema.json`](#-schema-json)
- [`settings.json`](#-settings-json)
- [`body.html`](#-body-html)

## `schema.json`
Define o schema das configurações que o widget recebe.

Exemplo:
```json
{
  "settings": [
    {
      "name": "title",
      "type": "string"
    },
    {
      "name": "description",
      "type": "string"
    }
  ]
}
```

Cada configuração é um objeto composto de duas propriedades: `name` e `type`, que define o nome e o tipo respectivamente. O nome será usado no template (leia sobre o `body.html` abaixo), enquanto o tipo restringe os valores do campo. Os tipos suportados são: "string" e "number".

## `settings.json`
Esse arquivo determina os valores default do arquivo de configuração, ele deve respeitar o schema definido no arquivo `schema.json`.

Exemplo:
```json
{
  "title": "Some image",
  "description": "Non descriptive description."
}
```

## `body.html`
Este arquivo de template será inserido no lugar da tag, como se fosse chamado através da tag [`include`](../../referencias/liquid/basic/tags.html). Porém, esse template tem acesso a uma variável nova, chamada `widget`.

A variável `widget` possui o objeto `settings` que tem os valores das configurações do widget.

Exemplo:
```
<h1>{{ widget.settings.title }}</h1>
<small>{{ widget.settings.description }}</small>
``` 



