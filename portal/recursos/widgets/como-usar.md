# Como usar um widget

Para inserir em um widget basta usar a seguinte tag Liquid dentro de um template:

```
{% widget 'nome-do-widget' id:'id-do-widget' %}
```

O `nome-do-widget` é o nome da pasta do widget. Além disso, é obrigatório adicionar o campo `id`, já que apenas dessa forma é possível referenciar um widget na interface administrativa.

Um widget irá usar a configuração padrão determinada no arquivo `settings.json`.