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
Define o nome amigável da página na interface administrativa.

### require
Algumas páginas exigem que sejam https ou que o usuário esteja autenticado.

Exemplo:
```
---
require: [ https, login ]
---
```
