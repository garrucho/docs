# Sandbox

A sandbox é usada para testar apps que não estão publicadas, ou seja, apps que estão na sua máquina.

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