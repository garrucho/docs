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

## widget

Adiciona um widget na página.

{% raw %}<pre>
{% widget 'nome-do-widget' id:'id-do-widget' %}
</pre>{% endraw %}

>### parâmetros
>
>String literal com o nome do widget
>
>id: identificador único do widget na página

[Ler mais sobre a tag `widget`](../../../recursos/widgets/usando.html)
