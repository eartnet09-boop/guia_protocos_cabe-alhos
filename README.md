# Protocolos e Cabeçalhos — Guia de Segurança para Profissionais

[![Licença MIT](https://img.shields.io/badge/licença-MIT-green.svg)](LICENSE)
![Revisão](https://img.shields.io/badge/revisão-2026--11--10-blue.svg)
[![RFC Editor](https://img.shields.io/badge/referência-RFC%20Editor-005A9C.svg)](https://www.rfc-editor.org/)
[![MDN](https://img.shields.io/badge/referência-MDN-000000.svg)](https://developer.mozilla.org/)
[![OWASP](https://img.shields.io/badge/referência-OWASP-3A7A2A.svg)](https://owasp.org/)

Guia prático em português para entender protocolos de rede, web, e-mail e autenticação sob a perspectiva de segurança, inspeção e resposta. Use os exemplos somente em sistemas e redes que você administra ou tem autorização para avaliar.

## Respostas rápidas

| Pergunta do profissional | Onde ir | Fonte oficial |
|---|---|---|
| Como funciona o TLS handshake? | [TLS](docs/protocolos/tls.md) | [RFC 8446](https://www.rfc-editor.org/rfc/rfc8446) |
| Quais headers HTTP de segurança devo configurar? | [Headers de segurança](docs/cabecalhos/http-seguranca.md) | [MDN](https://developer.mozilla.org/docs/Web/HTTP/Headers) / [OWASP](https://owasp.org/www-project-secure-headers/) |
| Como detectar DNS tunneling? | [DNS](docs/protocolos/dns.md) | [RFC 1035](https://www.rfc-editor.org/rfc/rfc1035) + [IANA](https://www.iana.org/assignments/dns-parameters/dns-parameters.xhtml) |
| Como o CORS pode ser mal configurado? | [CORS](docs/cabecalhos/cors.md) | [Fetch Standard](https://fetch.spec.whatwg.org/) |
| Como inspecionar tráfego no Wireshark? | [Guia de inspeção](docs/como-inspecionar.md) | — |

## Mapa em camadas

```mermaid
flowchart TB
	app[Aplicação: HTTP, SMTP, IMAP, DNS, SSH, OAuth/OIDC]
	sec[TLS: handshake, certificado, SNI, ALPN]
	trans[Transporte: TCP, UDP, QUIC]
	net[Internet/rede: IPv4, IPv6, ICMP]
	link[Enlace: Ethernet, ARP, Wi-Fi]
	app --> sec --> trans --> net --> link
	h1[Headers HTTP: Host, Cookie, CSP, HSTS, CORS]
	h2[Headers de e-mail: Received, DKIM-Signature]
	h3[Campos IP: endereçamento e opções]
	h4[Flags TCP: SYN, ACK, FIN etc.]
	h5[Extensões TLS: SNI, ALPN]
	h1 -. pertencem à aplicação .-> app
	h2 -. pertencem à mensagem .-> app
	h3 -. campos da camada IP .-> net
	h4 -. campos da camada TCP .-> trans
	h5 -. extensões do handshake .-> sec
```

Headers HTTP e de e-mail pertencem aos respectivos protocolos de aplicação; não são headers da camada IP/TCP. Em HTTP/3, HTTP é transportado sobre QUIC/UDP. [Visão geral](docs/protocolos/camada-transporte.md).

## Quick Start

1. Consulte o [glossário](docs/glossario.md) para alinhar as siglas.
2. Prepare a [inspeção](docs/como-inspecionar.md) com ferramentas locais.
3. Abra o [protocolo](docs/protocolos/) ou [header](docs/cabecalhos/) de interesse.

## Índice

- [Protocolos](docs/protocolos/): [rede](docs/protocolos/camada-rede.md), [transporte](docs/protocolos/camada-transporte.md), [DNS](docs/protocolos/dns.md), [TLS](docs/protocolos/tls.md), [HTTP](docs/protocolos/http.md), [e-mail](docs/protocolos/email.md), [autenticação](docs/protocolos/autenticacao.md), [SSH](docs/protocolos/ssh.md).
- [Cabeçalhos](docs/cabecalhos/): [HTTP segurança](docs/cabecalhos/http-seguranca.md), [HTTP gerais](docs/cabecalhos/http-gerais.md), [CORS](docs/cabecalhos/cors.md), [e-mail](docs/cabecalhos/email-headers.md), [rede](docs/cabecalhos/rede.md).
- [Guia de inspeção](docs/como-inspecionar.md) · [glossário](docs/glossario.md) · [fontes oficiais](data/links-oficiais.md).

Os padrões evoluem: confirme sempre a versão vigente nas fontes primárias. Veja também as [orientações de contribuição](.github/copilot-instructions.md).
