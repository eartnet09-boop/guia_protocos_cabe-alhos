# Headers HTTP gerais

## 1. O que é / para que serve

Campos da mensagem HTTP que carregam roteamento, representação, credenciais e metadados; a semântica geral está na [RFC 9110](https://www.rfc-editor.org/rfc/rfc9110), e framing HTTP/1.1 na [RFC 9112](https://www.rfc-editor.org/rfc/rfc9112).

## 2. Sintaxe

| Campo | Exemplo / formato | Observação |
|---|---|---|
| `Host` | `Host: app.example.com` | Autoridade em HTTP/1.1; HTTP/2/3 usa `:authority`. |
| `User-Agent` | `User-Agent: ExampleClient/1.0` | Declarado pelo cliente, não prova identidade. |
| `Authorization` | `Authorization: Bearer <token>` | Credencial; valor não deve ser registrado. |
| `Cookie` | `Cookie: session=<redacted>` | Cliente envia cookies aplicáveis. |
| `Set-Cookie` | `Set-Cookie: session=<redacted>; Secure; HttpOnly; SameSite=Lax` | Um campo por cookie; atributos controlam escopo e envio. |
| `Content-Type` | `Content-Type: application/json` | Tipo da representação. |
| `Content-Length` | `Content-Length: 42` | Comprimento em octetos da mensagem HTTP/1.1. |
| `Transfer-Encoding` | `Transfer-Encoding: chunked` | Framing HTTP/1.1; não combine ambiguamente com Content-Length. |
| `X-Forwarded-For` | `X-Forwarded-For: 192.0.2.1` | Convenção de proxy, não campo confiável por si. |
| `X-Real-IP` | `X-Real-IP: 192.0.2.1` | Convenção não padronizada para IP observado. |
| `Server` | `Server: webserver` | Identifica software, se divulgado. |

## 3. Valor seguro recomendado

Use cookies de sessão com `Secure; HttpOnly; SameSite=Lax` como ponto de partida, ajustando SameSite ao fluxo. Marque domínio/path estritamente necessários e prefira prefixos `__Host-` quando compatível. A origem do cliente deve ser determinada por proxies explicitamente confiáveis, que removem/reescrevem X-Forwarded-For; não aceite valor arbitrário. Não há valor universal para Server; minimizar banner reduz exposição, sem substituir patching.

## 4. Ausência ou má configuração

Credenciais sem TLS podem vazar; cookie sem Secure/HttpOnly amplia exposição; Host não validado pode contaminar links/caches; X-Forwarded-For confiado cegamente permite falsificar IP de auditoria. Framing inconsistente entre Content-Length e Transfer-Encoding em hops HTTP/1.1 pode causar request smuggling (RFC 9112).

## 5. Como verificar

Use `curl -sS -D - -o /dev/null https://$HOST/` e DevTools Network. Não compartilhe cookies ou Authorization capturados. Confira a configuração do proxy e da aplicação, não somente a resposta vista externamente.

## 6. Links oficiais

[RFC 9110](https://www.rfc-editor.org/rfc/rfc9110) · [RFC 9112](https://www.rfc-editor.org/rfc/rfc9112) · [MDN Set-Cookie](https://developer.mozilla.org/docs/Web/HTTP/Headers/Set-Cookie) · [MDN Cookie](https://developer.mozilla.org/docs/Web/HTTP/Headers/Cookie) · [OWASP Session Management](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html).