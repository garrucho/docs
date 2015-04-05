# meta.json

Toda app deve ter em sua pasta raíz o arquivo `meta.json`. Exemplo:

```json
{
  "name": "dreamstore-theme",
  "version": "0.1.0",
  "owner": "vtex",
  "dependencies": {
    "vtex.dreamstore-breadcrumb": "0.1.0",
    "vtex.dreamstore-menu": "0.1.0",
    "vtex.dreamstore-search": "0.1.0"
  },
  "schemas": {
    "storefront": 1
  }
}
```

## name
> As propriedades `name`, `version` e `owner` são as mais importantes de sua app. Elas são obrigatórias e sem elas, sua app não irá funcionar. Juntas elas geram um identificador único.

Lembre-se que o nome da sua app será passado como parâmetro para o uso de algum recurso, então tente escolher um pequeno, mas que também seja descritivo. Após publicada, o nome não pode ser alterado.


## version
> As propriedades `name`, `version` e `owner` são as mais importantes de sua app. Elas são obrigatórias e sem elas, sua app não irá funcionar. Juntas elas geram um identificador único.

A versão deve respeitar o [semver](http://semver.org/).

## owner
> As propriedades `name`, `version` e `owner` são as mais importantes de sua app. Elas são obrigatórias e sem elas, sua app não irá funcionar. Juntas elas geram um identificador único.

O owner de sua app é a conta no qual seu usuário na VTEX está, podendo este ser uma loja, uma conta de agência, etc.

## dependencies
A sua app pode depender de outras apps. O valor dessa propriedade é definido por um objeto com chave e valor, sendo a chave o owner e o nome da app concatenado com `.` e o valor sendo a versão da app.

## schemas
Define quais schemas estão sendo utilizados.