# Front matter

O front matter define metadados de um template. Ele deve ser a primeira coisa no arquivo e deve ser escrito em [YAML](http://yaml.org/) válido entre linhas com três traços (`---`). 

Exemplo de um template qualquer com front matter:
```
---
title: Home
---

restante do conteúdo
```

## Metadados

### title
Define um nome amigável da página para ser usado na interface administrativa. Isso é importante para que pessoas sem conhecimento técnico possam usar sua app.

### require
Algumas páginas exigem que sejam https ou que o usuário esteja autenticado.

Exemplo:
```
---
require: [ https, login ]
---
```

### category
Determina a categoria que o template pertence, muito útil para facilitar o desenvolvimento de apps plug-and-play. [Leia como usar uma app.](../../apps/como-usar.html#category)
