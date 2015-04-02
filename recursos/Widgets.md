# Widgets

## O que é um widget

Um widget é uma forma fácil de fazer com que partes do seu site seja alterado através da interface administrativa.

Um widget é composto de três arquivos: `body.html`, `schema.json` e `settings.json`. Todos devem estar dentro de uma pasta com o seu nome dentro da pasta `widgets`. Ex:

```
widgets/
 nome-do-widget/
   body.html
   schema.json
   settings.json
```

## Como usar

Para inserir em um widget basta usar a seguinte tag Liquid dentro de uma partial:

```python
{% widget 'nome-do-widget' id:'id-do-widget' %}
```

O `nome-do-widget` é o nome da pasta do widget. Além disos, é obrigatório adicionar o campo `id`, já que apenas dessa forma é possível referenciar um widget na interface administrativa.

Um widget por padrão irá usar a configuração padrão determinada no arquivo `settings.json`.

## Como criar

Para criar um novo widget, basta criar uma nova pasta dentro da pasta `widgets` com o seu nome. Em seguida, crie os três arquivos necessários: 

- `body.html`: é a partial que será inserida no lugar da tag
- `schema.json`: determina como o formulário de configuração será exibido
- `settings.json`: determina as configurações padrão do widget

## Como configurar

Para configurar um widget basta acessar a interface administrativa, ir até a página onde o widget está inserido e editar o formulário.

Uma outra forma é alterar o arquivo:

 `settings/pages/nome-da-pagina/widgets/nome-do-widget/id-do-widget.settings.json` 
