# HTTP

## 1. O que é

HTTP (Hypertext Transfer Protocol) é protocolo de aplicação de requisição/resposta. Semântica: [RFC 9110](https://www.rfc-editor.org/rfc/rfc9110); HTTP/1.1: [RFC 9112](https://www.rfc-editor.org/rfc/rfc9112); HTTP/2: [RFC 9113](https://www.rfc-editor.org/rfc/rfc9113); HTTP/3: [RFC 9114](https://www.rfc-editor.org/rfc/rfc9114).

## 2. Como funciona

Cliente envia método, target e campos; servidor devolve status, campos e conteúdo. HTTP/1.1 usa mensagens textuais, HTTP/2 frames binários multiplexados sobre TCP, HTTP/3 streams sobre QUIC/UDP.

| Versão | Transporte e efeito operacional |
|---|---|
| HTTP/1.1 | TCP; framing por comprimento ou chunked exige parsing consistente entre intermediários. |
| HTTP/2 | TCP + TLS normalmente; multiplexação e compressão HPACK mudam análise, não a semântica HTTP. |
| HTTP/3 | QUIC; headers QPACK e transporte cifrado integrado reduzem visibilidade passiva. |

Métodos comuns: GET, HEAD, POST, PUT, PATCH, DELETE, OPTIONS. Status de interesse: 3xx redirecionamento, 401 autenticação, 403 autorização, 404 recurso, 429 limitação, 5xx falha. Semântica e cacheabilidade dependem da RFC 9110.

## 3. Relevância de segurança

Request smuggling pode surgir quando intermediários discordam sobre framing; host-header injection pode afetar links/reset/cache quando a aplicação confia em Host sem validação; cache poisoning explora chaves de cache incompletas. HTTP/2/3 não eliminam falhas de aplicação nem desentendimentos em gateways.

## 4. Ataques comuns

- **Request smuggling**: exige parsing/framing divergente entre hops; indicadores incluem discrepâncias de logs, respostas fora de ordem e erros de proxy. Corrija parsing e mantenha intermediários atualizados; não teste payloads em terceiros.
- **Host header injection**: valores Host não confiáveis influenciam lógica; monitore hosts inesperados e URLs absolutas geradas.
- **Cache poisoning**: conteúdo varia por campo ignorado na chave; procure cache hit de resposta incompatível e headers `Age`/`Vary` inconsistentes.

## 5. O que monitorar

Método, host validado, rota, status, tamanho, duração, redirect, cache status, user-agent e request-id; redija dados sensíveis. Filtros visíveis: `http.request.method == "POST"`, `http.response.code >= 400`; TLS impede visibilidade passiva sem decriptação autorizada.

## 6. Como inspecionar na prática

Consulte comandos curl e DevTools em [como inspecionar](../como-inspecionar.md). Compare logs do proxy e aplicação usando request-id; valide que o Host aceito está em allowlist.

## 7. Erros comuns / más configurações

Confiar em Host/X-Forwarded-Host arbitrário; aceitar framing contraditório; registrar Authorization/cookies; assumir que HTTPS corrige controle de acesso; publicar stack traces.

## 8. Links oficiais

[RFC 9110](https://www.rfc-editor.org/rfc/rfc9110) · [RFC 9112](https://www.rfc-editor.org/rfc/rfc9112) · [RFC 9113](https://www.rfc-editor.org/rfc/rfc9113) · [RFC 9114](https://www.rfc-editor.org/rfc/rfc9114) · [MDN HTTP](https://developer.mozilla.org/docs/Web/HTTP).