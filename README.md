# VTEX Lab Docs

### Instalando as dependências:

```sh
$ npm install
$ npm install -g gitbook-cli
$ npm install -g grunt-cli
$ gitbook versions:install 2.0.1
```

### Desenvolvendo e escrevendo

Para rodar, use a versão 2.0.1. A versão 2.0.2 está com bug:

```sh
$ gitbook serve -v 2.0.1
```

Abra a URL: [localhost:4000](http://localhost:4000/)

Todos os segredos do Gitbook estão em: [help.gitbook.com](http://help.gitbook.com)

### Publicando

Para publicar, digite no terminal:

```sh
$ grunt
```

Esse processo irá compilar o livro e commitar o resultado na branch `gh-pages`.
