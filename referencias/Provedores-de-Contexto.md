## Formato do provedor

Arquivo: `storefront/contexts/{{name}}.json`

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| **url** | _string_ | URL a ser consultada para obtenção do contexto |
| **bindings** | _object_ | Descreve as informações relevantes para que o provedor seja consultado |

**bindings**:

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| **user** | _bool_ | Usuário autenticado |
| **query** | _string[]_ | Lista de parâmetros de query string |


## Requisição enviada ao endpoint

```
Content-Type: application/json
```

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| **account** | _string_ | Nome da conta atual |
| **workspace** | _string_ | Nome do workspace sendo usado |
| **sandbox** | _object_ | Descrição da sandbox sendo usada, caso alguma |
| **queryString** | _map_ | Mapa de valores dos parâmetros de _query string_ pedidos |
| **user** | _string_ | Login do usuário autenticado. _null_ caso não esteja autenticado. |


## Resposta esperada do endpoint

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| **inject** | _map_ | Mapa de valores a ser disponibilizado na variável de mesmo nome que o provedor |
| **hash** | string | _Hash_ dos valores retornados |