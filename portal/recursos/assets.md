# Assets

É a pasta onde ficam todos os arquivos estáticos: imagens, CSS, Javascript, etc.

## Scripts

Para inserir um script Javascript no template, basta usar a tag [`script`](../referencias/liquid/portal/tags.html#scripts).

### Exemplo

Pasta assets:
```
assets/
└── main.js
```

Arquivo de template qualquer:
```html
{% script 'main.js' %}
```

## CSS

Para inserir um arquivo CSS, use a tag [`style`](../referencias/liquid/portal/tags.html#style).

### Exemplo

Pasta assets:
```
assets/
├── main.js
└── style.js
```

Arquivo de template qualquer:
```html
{% style 'style.css' %}
```

### Outros

Para qualquer outro tipo de arquivo na pasta assets, use o filtro Liquid [`assetUrl`](../referencias/liquid/portal/filtros.html).

### Exemplo

Pasta assets:
```
assets/
├── main.js
├── style.css
└── logo.jpg
```

Arquivo de template qualquer:
```html
<img src="{{ 'logo.png' | assetUrl }}" title="Minha imagem"/>
```
