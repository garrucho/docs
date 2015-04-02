# Models

Um model define uma chamada uma API, guardando a sua resposta em um objeto JSON para uso em templates.

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


Um exemplo de utilização de um model em um template:

{% raw %}<pre>
{% model facets = 'facet' category:search.category brand:search.brand search:search.searchTerm facets:search.facets %}
</pre>{% endraw %}

Ler mais sobre a [tag Liquid model](./referencias/liquid/portal/tags.html#model)
