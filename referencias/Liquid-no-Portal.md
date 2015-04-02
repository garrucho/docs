# Sumário

1. [Variáveis](#variables)
2. [Filters](#filters)

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

# Tags

## area

Reserva uma área da página HTML onde poderão ser inseridos widgets por configuração.

{% raw %}<pre>
{% area id:'name' %}
</pre>{% endraw %}

>### parâmetros
>
>**_id_**: Identificador da área

## lib

Insere uma lib (javascript, css, etc..) na página. A lib deve ser previamente declarada em `libs.json`.

{% raw %}<pre>
{% lib 'bootstrap' 'js/bootstrap.js' %}
</pre>{% endraw %}

>### parâmetros
>
>Dois parâmetros devem ser repassados para utilização da tag, na ordem:
>
>- O nome da lib de acordo com o que foi declarado em `libs.json`
>- O arquivo desejado dentro da lib

## model

Carrega dados de um modelo em uma variável. O modelo deve estar previamente declarado dentro da pasta `models`

{% raw %}<pre>
{% model product = 'product' %}
</pre>{% endraw %}

>### parâmetros
>
>É obrigatório fazer a atribuição de uma string literal para uma variável, onde será caregado o modelo.
>
>Qualquer outro parâmetro inserido será repassado para o modelo, que pode usá-lo na rota ou querystring.
>
> Por exemplo:
>{% raw %}<pre>
>{% model shelf = 'shelf' category:'10' search:'bola' %}
></pre>{% endraw %}

## record

Define um bloco cujo conteúdo é copiado e pode ser replicado através da tag `replay`

{% raw %}<pre>
{% record 'itemsList' %}
{% for item in items %}
- {{ item }}
{% endfor %}
{% endrecord %}
</pre>{% endraw %}

## replay

Replica o conteúdo armazenado pela tag `record`

{% raw %}<pre>
{% assign items = listOne %}
{% replay 'itemsList' %}
</pre>{% endraw %}

## script

Inclui um script da pasta `assets`. Deve ser usado para scripts de autoria própria, que não podem ser incluídos pela tag `lib`

{% raw %}<pre>
{% script 'vtex.js' %}
</pre>{% endraw %}

>### parâmetros
>
>String literal com o caminho do arquivo de script dentro da pasta assets

## style

Inclui um css da pasta `assets`. Deve ser usado para css's de autoria própria, que não podem ser incluídos pela tag `lib`

{% raw %}<pre>
{% style 'vtex.css' %}
</pre>{% endraw %}

>### parâmetros
>
>String literal com o caminho do arquivo css dentro da pasta assets

## with

Faz uma atribuição para uma variável, que estará disponível dentro do bloco definido.

Caso a atribuição gere uma variável [*falsey*](http://james.padolsey.com/javascript/truthy-falsey/) o conteúdo do bloco não é renderizado.

{% raw %}<pre>
{% with variable = "value" %}
Aqui eu posso usar {{ variable }}!
{% endwith %}
</pre>{% endraw %}

>### parâmetros
>
>É obrigatório fazer a atribuição para uma várivel

# Filtros

* `append` - anexa uma string a outra ou um elemento a um array
  {% raw %}<pre>
  {% assign array = "1,2,3" | split:"," %}

  {{ array | append:'4' }} #=> [1, 2, 3, 4]
  {{ 'foo' | append:'bar' }} #=> 'foobar'
  </pre>{% endraw %}
* `assetUrl` - gera a url para o asset

  {% raw %}<pre>
  {{ 'styles/some.css'| assetUrl }} #=> '/assets/styles/some.css'
  </pre>{% endraw %}
* `price` - formata e adiciona símbolo da moeda

  {% raw %}<pre>
  {{ 1.9 | price }} #=> 'R$ 1,90'
  </pre>{% endraw %}
* `money` - formata número

  {% raw %}<pre>
  {{ 1570 | money }} #=> '1,570.00'
  </pre>{% endraw %}
* `debug` - exibe estrutura do objeto

  {% raw %}<pre>
  {{ variable | debug }} #=> "Object { name: \"lala\", skus: Array(3), id: 5 }"
  </pre>{% endraw %}
* `padleft` - adiciona zeros a esquerda

  {% raw %}<pre>
  {{ 2016 | padleft: "7" }} #=> '0002016'
  </pre>{% endraw %}
* `padright` - adiciona zeros a direita

  {% raw %}<pre>
  {{ 2016 | padright: "7" }} #=> '2016000'
  </pre>{% endraw %}
* `app` - exibe a app da página

  {% raw %}<pre>
  {{ 'product@catalog' | app }}  #=> 'catalog'
  </pre>{% endraw %}
* `resource` - exibe a conta da página

  {% raw %}<pre>
  {{ 'product@catalog' | resource }} #=> 'product'
  </pre>{% endraw %}
