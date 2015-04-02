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
