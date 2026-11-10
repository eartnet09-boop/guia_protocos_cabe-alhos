# Headers HTTP de segurança

Consulte também [headers gerais](http-gerais.md), [CORS](cors.md) e o [guia de inspeção](../como-inspecionar.md). Headers são políticas aplicadas pelo user agent; teste compatibilidade e escopo no sistema real.

## 1. O que é / para que serve

Campos de resposta que comunicam políticas de navegador. A sintaxe segue HTTP ([RFC 9110](https://www.rfc-editor.org/rfc/rfc9110)); diretivas e suporte específicos têm referências abaixo.

## 2. Sintaxe

| Header | Exemplo de sintaxe válida | Finalidade |
|---|---|---|
| `Strict-Transport-Security` | `max-age=31536000; includeSubDomains` | Impõe HTTPS após visita segura. |
| `Content-Security-Policy` | `default-src 'self'; object-src 'none'; base-uri 'self'` | Restringe origens de conteúdo. |
| `X-Frame-Options` | `DENY` | Controle legado de framing. |
| `X-Content-Type-Options` | `nosniff` | Desativa MIME sniffing em contextos suportados. |
| `Referrer-Policy` | `strict-origin-when-cross-origin` | Limita informação no Referer. |
| `Permissions-Policy` | `camera=(), microphone=()` | Desativa recursos não usados. |
| `Cross-Origin-Opener-Policy` | `same-origin` | Isola grupo de contexto de navegação. |
| `Cross-Origin-Embedder-Policy` | `require-corp` | Exige autorização para recursos cross-origin. |
| `Cross-Origin-Resource-Policy` | `same-origin` | Limita quem pode carregar o recurso. |
| `Clear-Site-Data` | `"cache", "cookies", "storage"` | Solicita limpeza de dados para a origem. |

## 3. Valor seguro recomendado

Comece com CSP restritiva em modo `Content-Security-Policy-Report-Only` e refine fontes; evite `unsafe-inline`/`unsafe-eval` sem necessidade. HSTS só após HTTPS completo e rollout; `includeSubDomains`/preload afetam todos os subdomínios. Para páginas que não devem ser embutidas, use CSP `frame-ancestors 'none'` ou `X-Frame-Options: DENY`. `nosniff`, `strict-origin-when-cross-origin` e permissões explicitamente desativadas são bons pontos de partida, ajustados ao produto.

COOP/COEP/CORP podem quebrar integrações e são requisitos de isolamento em casos específicos, não receita universal. `Clear-Site-Data` pode desconectar usuários e apagar dados; use em contexto apropriado.

## 4. Ausência ou má configuração

Sem CSP, um XSS tem menos barreiras; sem proteção de framing, pode haver clickjacking; sem HSTS, downgrade/visita inicial em HTTP continua possível. Políticas permissivas ou escopos excessivos dão falsa sensação de proteção. Headers não substituem correção de XSS, autorização nem configuração TLS.

## 5. Como verificar

```sh
curl -sS -D - -o /dev/null https://$HOST/
```

Inspecione cada origem, subdomínio e resposta relevante em DevTools. Ferramentas: [Security Headers](https://securityheaders.com/), [SSL Labs](https://www.ssllabs.com/ssltest/) e [HSTS Preload](https://hstspreload.org/); considere a divulgação do alvo.

## 6. Links oficiais

[MDN HTTP headers](https://developer.mozilla.org/docs/Web/HTTP/Headers) · [OWASP Secure Headers](https://owasp.org/www-project-secure-headers/) · [HSTS RFC 6797](https://www.rfc-editor.org/rfc/rfc6797) · [CSP Level 3](https://www.w3.org/TR/CSP3/) · [X-Frame-Options RFC 7034](https://www.rfc-editor.org/rfc/rfc7034) · [Referrer Policy](https://www.w3.org/TR/referrer-policy/) · [Permissions Policy](https://www.w3.org/TR/permissions-policy-1/).