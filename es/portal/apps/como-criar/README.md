{% include 'portal/aviso.md' %}

# Como criar uma app

## VTEX Toolbelt
Para desenvolver uma app é essencial que você use o [VTEX Toolbet](https://github.com/vtex/toolbelt).

Primeiro instale o node e o npm ([Linux](https://gist.github.com/isaacs/579814), [Mac e Windows](https://nodejs.org/download/)).

Agora instale o VTEX Toolbelt, digite no terminal (não importa onde):
```
npm install -g vtex
```

[Leia mais sobre como usar o VTEX Toolbelt!](../referencias/toolbelt.html)

## generator-vtex
Uma ferramenta não essencial, mas que irá lhe auxiliar é o [generator-vtex](https://github.com/vtex/generator-vtex). Com ela você pode criar a estrutura de pastas básica de uma app nova.

Para isso, instale o Yeoman e o generator-vtex, digite no terminal (não importa onde):
```
npm install -g yo
npm install -g generator-vtex
```

Após instalado, basta chamar o gerador na pasta onde você deseja desenvolver a nova app. O comando abaixo irá criar toda a estrutura de pastas necessária para que você começe a desenvolver uma app do zero.
```
yo vtex
```


<hr/>


Caso você prefira seguir sem o `generator-vtex`, você pode criar a estrutura de pastas na mão.

A estrutura de pasta de uma app é:

```
.
├── storefront
│   ├── assets
│   ├── models
│   ├── pages
│   ├── partials
│   ├── widgets
│   ├── libs.json
│   ├── routes.json
│   └── rules.json
└── meta.json
```

Para mais informações sobre cada arquivo e pasta, leia o capítulo [Recursos](../../recursos/).

<hr/>

Agora temos todas as ferramentas e estruturas necessárias para começar a desenvolver uma app!
