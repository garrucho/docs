{% include 'portal/aviso.md' %}

# Models

Um model é um arquivo JSON que define uma chamada a uma API.

Exemplo de um model chamado `facet.json`:
```json
{
  "url": "http://catalog-facade.apphb.com/:account/facets",
  "cacheDuration": 60,
  "parameters": {
    "account": "{{ account.name }}"
  },
  "queryString": {
    "c": "{{ parameters.category }}",
    "b": "{{ parameters.brand }}",
    "q": "{{ parameters.search }}",
    "f": "{{ parameters.facets }}",
    "inStockOnly": "{% if parameters.inStockOnly %}{{ parameters.inStockOnly }}{% else %}false{% endif %}"
  }
}
```


A principal utilidade do model é a possibilidade de utiliza-lo em um template. Exemplo:

{% raw %}<pre>
{% model facets = 'facet' category:search.category brand:search.brand search:search.searchTerm facets:search.facets %}
</pre>{% endraw %}

Leia mais sobre a [tag Liquid `model`](./referencias/liquid/portal/tags.html#model).

## Formato

### url
O campo `url` é obrigatório na definição de um model. Ele define qual rota será usada.

Você pode definir parâmetros a serem interpolados prefixando-os com `:`, como `:account`, mostrado no exemplo acima. O parâmetro deve ser definido no objeto [`parameters`](#parameters), descrito abaixo.

### cacheDuration
Define a duração em segundos do cache para esse modelo. O valor deve ser um número.

### parameters
Define um objeto com os parâmetros a serem interpolados na `url`. Você pode usar tags Liquid para preencher o seu valor.

### queryString
Define um objeto a ser adicionado na query string da `url`. Você pode usar tags Liquid para preencher o seu valor.
