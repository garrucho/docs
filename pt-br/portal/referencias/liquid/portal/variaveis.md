{% include 'portal/aviso.md' %}

# Variáveis

Uma variável é declarada da seguinte forma: `{{ name }}`.
Por exemplo, é possível exibir o 'account name' com o trecho:

```
Minha conta: {{ account.name }}
```

## Variáveis disponíveis

As variáveis abaixo estão disponíveis para usar em qualquer código HTML:

* `account` - informação sobre a conta
    * `name` - nome da conta (ex. "dreamstore")
* `activeRules` - lista de regras ativas
* `admin` - informação
    * `email` - email do usuário
    * `isStaff` - é colaborador VTEX (ex. true)
* `campaign` - informação sobre a campanha
    * `name` - nome da campanha (ex. Natal)
    * `source` - (ex. Google, Buscapé)
    * `internalName` - campanha interna. (ex. Páscoa)
* `categories` -  lista de templates de uma categoria
    * `templates` - lista de templates (ex. `categories.cool-category.templates`)
* `culture` - informação de datas, horas, números e moeda de uma cultura
    * `code` - código da cultura (ex. pt-BR)
    * `language` - prefixo da língua (ex. pt)
    * `country` - sigla do país (ex. BR)
    * `currency` - símbolo da moeda. (ex. R$)
* `date` - data levando em consideração a cultura
    * `day` - dia
    * `month` - mês
    * `year` - ano
    * `dayOfWeek` - dia da semana (ex. "thursday")
    * `iso` - ano apresentado no formato `yyyy-MM-dd` (ex. 1955-02-24)
* `request` - request enviada para o nossos servidores
  * `host` - o host da URL (ex. www.mysite.com)
  * `path` - o path da URL (ex. /categories/car-accessories)
  * `queryString` - valores da query string (ex. `request.queryString.utm_campaign`)
  * `url` - URL completa (ex. http://www.mysite.com/categories/car-accessories?utm_medium=social)
* `route` - informaçôes sobre a rota
  * `path` - o path da rota (ex. /)
  * `parameters` - (ex. )
