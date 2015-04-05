# libs.json

O arquivo `libs.json` é um arquivo em JSON que define quais as versões das bibliotecas front end estão sendo usadas.

Exemplo:
```json
{
  "twitter-bootstrap": "3.3.4",
  "jquery": "1.8.3",
  "font-awesome": "4.3.0"
}
```

Ao chamar a tag Liquid `lib` ([ler referência](../referencias/liquid/portal/tags.html#lib)) a versão específicada neste arquivo será usada.