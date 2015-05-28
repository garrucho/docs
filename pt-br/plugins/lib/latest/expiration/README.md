# Script de expiração para sessão 

## session-expiration.js

Utilitário de expiração de sessão por tempo.  
Ao ser iniciado, um contador de 10 minutos é configurado.  
Qualquer interação do usuário com a página reinicia o contador.  
Após 10 minutos sem interação, o cookie do carrinho é removido e o navegador é redirecionado para uma página default.

## Versão 0.1.1

Script para desenvolvimento: http://io.vtex.com.br/portal-plugins/2.8.0-beta/js/expiration/session-expiration.js  
Script para produção: http://io.vtex.com.br/portal-plugins/2.8.0-beta/js/expiration/session-expiration.min.js

### Funções públicas

#### vtex.portal.startExpiration(url, millis, events)

Inicia timer.

Parâmetros e defaults:

- url = '/'
- millis = 10 * 60 * 1000 (10 minutes)
- events = ["mousemove", "keyup", "click", "scroll"]

#### vtex.portal.stopExpiration()

Pára contador em execução.
