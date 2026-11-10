# Campos e extensões de rede

## 1. O que é / para que serve

Campos de cabeçalho IP/TCP e extensões de handshake TLS ajudam a interpretar fluxos. Formatos: IPv4 [RFC 791](https://www.rfc-editor.org/rfc/rfc791), IPv6 [RFC 8200](https://www.rfc-editor.org/rfc/rfc8200), TCP [RFC 9293](https://www.rfc-editor.org/rfc/rfc9293), TLS [RFC 8446](https://www.rfc-editor.org/rfc/rfc8446).

## 2. Sintaxe

| Campo/flag | Significado observado |
|---|---|
| IPv4: Version, IHL, Total Length, TTL, Protocol, checksum, source/destination | Versão, tamanho, limite de saltos, protocolo encapsulado e endereços; opções são extensões variáveis. |
| IPv6: Version, Traffic Class, Flow Label, Payload Length, Next Header, Hop Limit, source/destination | Base fixa; opções/extensões seguem encadeadas via Next Header. |
| TCP SYN | Inicia estabelecimento/sincroniza sequência. |
| TCP ACK | Campo de reconhecimento é significativo. |
| TCP FIN | Solicita encerramento ordenado do fluxo. |
| TCP RST | Aborta/rejeita conexão. |
| TCP PSH | Indicação de push ao receptor; não marca “fim de mensagem” da aplicação. |
| TCP URG | Indica ponteiro urgente; raro em aplicações atuais. |
| TLS SNI | Nome solicitado no ClientHello, se não protegido por ECH (RFC 6066). |
| TLS ALPN | Protocolo de aplicação negociado (RFC 7301), ex.: `h2`. |

## 3. Valor seguro recomendado

Flags não são configuração de segurança isolada. Valide padrões com contexto do fluxo e endpoints. Para serviços TLS, habilite versões/ciphers suportados e corretos; restrinja protocolos de gestão pela arquitetura.

## 4. Ausência ou má configuração

Flags inconsistentes podem indicar varredura, falha ou captura assimétrica; não são prova de ataque. Opções IP inesperadas podem ser descartadas por middleboxes. SNI revela nome a observadores quando não há ECH; ALPN expõe o protocolo negociado em cenários comuns. TLS cifra payload, não necessariamente todo metadado.

## 5. Como verificar

No Wireshark use `tcp.flags`, `ip`/`ipv6` e `tls.handshake.extensions_server_name`; veja [como inspecionar](../como-inspecionar.md). Compare captura dos dois lados de um proxy quando autorizado; lembre-se de offload, NAT e captura assimétrica.

## 6. Links oficiais

[IPv4 RFC 791](https://www.rfc-editor.org/rfc/rfc791) · [IPv6 RFC 8200](https://www.rfc-editor.org/rfc/rfc8200) · [TCP RFC 9293](https://www.rfc-editor.org/rfc/rfc9293) · [TLS RFC 8446](https://www.rfc-editor.org/rfc/rfc8446) · [SNI RFC 6066](https://www.rfc-editor.org/rfc/rfc6066) · [ALPN RFC 7301](https://www.rfc-editor.org/rfc/rfc7301).