# Usando um widget

Para inserir em um widget basta usar a seguinte tag Liquid dentro de uma partial:

```
{% widget 'nome-do-widget' id:'id-do-widget' %}
```

O `nome-do-widget` é o nome da pasta do widget. Além disos, é obrigatório adicionar o campo `id`, já que apenas dessa forma é possível referenciar um widget na interface administrativa.

Um widget por padrão irá usar a configuração padrão determinada no arquivo `settings.json`.