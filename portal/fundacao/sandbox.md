{% include 'portal/aviso.md' %}

# Sandbox

A sandbox é usada para testar apps que não estão publicadas. Você irá usar uma sandbox apenas se você é um desenvolvedor de apps VTEX.

## Como usar

Para usar uma sandbox basta criar um cookie no domínio da sua loja.

**Nome do cookie:** `vtex_sandbox`

**Valor do cookie:** `<conta>/<nome-da-sandbox>=<nome-da-app>`

Exemplo:
```
vtex_sandbox
vtex/minhasandbox=dreamstore-theme
```

Caso você esteja testando mais de uma app, separe-as por `+`.

Exemplo:
```
vtex_sandbox
vtex/minhasandbox=dreamstore-theme+dreamstore-search
```

Você pode usar uma extensão do seu navegador para alterar os seus cookies ([algumas extensões para o Google Chrome](https://chrome.google.com/webstore/search/cookie%20inspector%20edit?_category=extensions)).