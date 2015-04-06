# routes.json

Este arquivo define para quais [pages](templates/pages.html) as URLs serão apontadas e fornece modelos, que utilizam a URL para serem gerados, para elas.

## Roteamento simples

Aponta uma página a uma rota específica.

### Exemplo

Arquivo `routes.json`:
```json
{
  "/": {
    "page": "home"
  },
  "/contato/": {
    "page": "contato"
  }
}
```

Ao entrar na URL http://www.endereco-da-sua-loja.com/contato/ a [page](templates/pages.html) `contato` será exibida.

## Roteamento parametrizado

Caso a URL tenha um valor variável, o roteamento parametrizado é o mais recomendado.

### Exemplo

Arquivo `routes.json`:
```json
{
  "/": {
    "page": "home"
  },
  "/contato/": {
    "page": "contato"
  },
  "/:slug/p": {
    "page": "product",
    "models": {
      "product": {
        "provider": "product",
        "parameters": {
          "slug": "route.parameters.slug"
        }
      }
    }
  }
}
```

Qualquer texto no lugar de `:slug` será atribuido a variável `route.parameters.slug`. No exemplo acima, essa variável foi utilizada para preencher o [model](models.html) product, que será disponiblizado para ser utilizado nos templates.

O valor do parâmetro é arbitrário, poderia ter sido utilizado qualquer outro nome no lugar de "slug". O importante é prefixar a variável com ":".

## Roteamento condicional

Recomenda-se o roteamento condicional caso você queira determinar a rota dinamicamente.

### Exemplo

Arquivo `routes.json`:
```json
{
  "/": {
    "page": "home"
  },
  "/contato/": {
    "page": "contato"
  },
  "/:slug/p": {
    "page": "product",
    "models": {
      "product": {
        "provider": "product",
        "parameters": {
          "slug": "route.parameters.slug"
        }
      }
    }
  },
  "/*term": {
    "models": {
      "search": {
        "provider": "search",
        "parameters": {
          "term": "route.parameters.term",
          "map": "request.queryString.map"
        }
      }
    },
    "page": [
      {
        "if": "search.type == 'brand'",
        "then": {
          "page": "brand",
          "models": {
            "brand": {
              "provider": "brand",
              "parameters": {
                "id": "search.id"
              }
            }
          }
        }
      },
      {
        "if": "search.type == 'category'",
        "then": {
          "page": "category",
          "models": {
            "category": {
              "provider": "category",
              "parameters": {
                "id": "search.id"
              }
            }
          }
        }
      },
      { "then": "search" }
    ]
  }
```

Se a URL digitada não se encaixar nas regras anteriores ele irá para a rota "/\*term/". O "*" significa qualquer coisa digitada irá para a variável `term`. O modelo search foi preenchido com esse valor.

A página será definida de acordo com o modelo gerado. Caso a condição `search.type == 'brand'` seja verdade, ele irá usar a página `brand` e irá prover modelo brand. Do contrário, ele irá verificar a condição `search.type == 'category'`, se ela também não for atendida, em último caso a página `search` será exibida.
