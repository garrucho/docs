# Customizando resultado de busca

No canto superior direito temos a caixa de busca. A busca pode ser por uma palavra ou expressão.
Ao digitar um valor, o sistema procura os documentos que possuem aquela palavra ou expressão em campos configurados como buscáveis no VTEX Master Data.

O resultado dessa consulta é exibido em uma "aba" nova chamada de "Todos" com todos os resultados.

Como não customizamos esse resultado, aparece somente o ID precedido pela Sigla da Entidade de dados e ao lado, o formulário que corresponde essa busca.
<a href="../img/form-sheet-search-1.jpg" target="\_blank">
<img src="../img/form-sheet-search-1.jpg" alt="" />
</a>

Podemos configurar essa listagem fazendo com que apareça quaisquer campos do documento. Para isso, siga os passos a seguir:

1. Em "Configurações", clique em "Formulários";
<a href="../img/config-2.jpg" target="\_blank">
<img src="../img/config-2.jpg" alt="" />
</a>

2. Na listagem de formulários, clique em "Editar";
<a href="../img/form-edit-update-1.jpg" target="\_blank">
<img src="../img/form-edit-update-1.jpg" alt="" />
</a>

3. Preencha o campo "Destaques de busca" com os campos os quais deseja exibir. O primeiro campo será o título que ficará no lugar do id.
<a href="../img/form-edit-search-1.jpg" target="\_blank">
<img src="../img/form-edit-search-1.jpg" alt="" />
</a>
Obs: Os campos preenchidos foram ```firstName``` que representa o campo Nome, ```lastName``` que representa o campo Nome, ```dicument``` que representa o campo CPF/CNPJ, ```isNewsletterOptIn``` que representa o campo Recebe Newsletter;

4. Clique em "Salvar";

5. Agora, faça uma nova busca e a listagem terá o formato que desenhou para cada formulário.
<a href="../img/form-sheet-search-2.jpg" target="\_blank">
<img src="../img/form-sheet-search-2.jpg" alt="" />
</a>

### Buscando por uma expressão

Para se buscar por uma expressão, coloque o valor entre aspas.
Por exemplo:

```
"Ana Maria"
```
