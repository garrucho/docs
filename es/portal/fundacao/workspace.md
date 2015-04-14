{% include 'portal/aviso.md' %}

# Workspace

Um Workspace é um espaço de trabalho isolado para desenvolvimento e testes. As configurações do workspace são versionadas, isso signifca que caso algo quebre a loja, ela irá voltar para um ponto de restauração automaticamente.

Você pode criar quantos workspaces quiser, basta dar um nome único a ele.

## Como usar

Para visualizar uma loja em um determinado workspace, crie um cookie no domínio da sua loja.


**Nome do cookie:** `vtex_workspace`

**Valor do cookie:** `nome-do-workspace`

Exemplo:
```
vtex_workspace
meuworkspace
```

Você pode usar uma extensão do seu navegador para alterar os seus cookies ([algumas extensões para o Google Chrome](https://chrome.google.com/webstore/search/cookie%20inspector%20edit?_category=extensions)).
