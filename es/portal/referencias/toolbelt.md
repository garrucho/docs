{% include 'portal/aviso.md' %}

# VTEX Toolbelt

Para desenvolver uma app é essencial que você use o [VTEX Toolbet](https://github.com/vtex/toolbelt).

## Instalação

Primeiro instale o node e o npm ([Linux](https://gist.github.com/isaacs/579814), [Mac e Windows](https://nodejs.org/download/)).

Agora instale o VTEX Toolbelt, digite no terminal (não importa onde):
```
npm install -g vtex
```

## Comandos

### `login`
Você precisa estar logado para usar o Toolbelt, para isso, digite `vtex login` e se logue usando seu email e senha cadastrados no VTEX ID.

### `logout`
Caso você queira usar outra conta, você pode usar o comando `vtex logout` e fazer login novamente.

### `watch <sandbox>`
Esse é o comando que mais será usado. Ele faz com que você torne uma app disponível na sandbox especificada.

Por exemplo, você está desenvolvendo a app `minhaloja-theme`. Para ver as alterações que você está fazendo na app na sandbox ([leia mais sobre sandbox](../fundacao/sandbox.md)) `minhasandbox`, você deve rodar o comando:

```
vtex watch minhasandbox
```

Todos os arquivos que estão na pasta da app serão enviados para os servidores VTEX e com isso você consiguirá ver as alterações na URL: http://NOMEDALOJA.myvtex.com.br. Lembre-se que você tem que estar com o cookie da sandbox configurado corretamente ([leia mais sobre sandbox](../fundacao/sandbox.md)).

Toda alteração que você fizer na pasta da app, será ouvida pelo Toolbelt e ele irá enviar os arquivos alterados automáticamente para os servidores VTEX.

### `publish`

Quando estiver satisfeito com o seu app e quiser publicá-lo na Gallery. Basta digitar:

```
vtex publish
```

Pronto! Qualquer pessoa poderá instalar o seu app!

