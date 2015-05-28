{% include 'portal/aviso.md' %}

# rules.json

O arquivo rules.json define regras que afetam o conteúdo do site.

Um exemplo seria exibir a prateleira principal com um conteúdo diferente caso o usuário venha do Google ou do Buscapé.

Exemplo de um arquivo de `rules.json`:
```json
[
  {
    "name": "google",
    "conditions": [
        "campaign.source == 'google'"
    ]
  },
  {
    "name": "buscape",
    "conditions": [
        "campaign.source == 'buscape'"
    ]
  }
]
```
