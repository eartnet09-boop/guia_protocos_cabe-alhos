# Glossário

Cada definição resume o uso neste guia; consulte o padrão vinculado para semântica normativa.

| Sigla | Nome completo (inglês) | Em uma linha | Guia |
|---|---|---|---|
| TCP | Transmission Control Protocol | Transporte confiável, orientado a conexão e ordenado. | [Transporte](protocolos/camada-transporte.md) |
| UDP | User Datagram Protocol | Transporte sem conexão e sem garantia de entrega. | [Transporte](protocolos/camada-transporte.md) |
| IP | Internet Protocol | Endereçamento e encaminhamento de datagramas IPv4/IPv6. | [Rede](protocolos/camada-rede.md) |
| ICMP | Internet Control Message Protocol | Mensagens de erro e diagnóstico da camada IP. | [Rede](protocolos/camada-rede.md) |
| ARP | Address Resolution Protocol | Mapeia endereço IPv4 para endereço de enlace em uma rede local. | [Rede](protocolos/camada-rede.md) |
| DHCP | Dynamic Host Configuration Protocol | Distribui parâmetros de configuração de rede a clientes. | [Rede](protocolos/camada-rede.md) |
| DNS | Domain Name System | Sistema distribuído de nomes e registros. | [DNS](protocolos/dns.md) |
| DoH | DNS over HTTPS | Transporta consultas DNS em HTTPS. | [DNS](protocolos/dns.md) |
| DoT | DNS over TLS | Transporta DNS sobre TLS, normalmente na porta 853. | [DNS](protocolos/dns.md) |
| TLS | Transport Layer Security | Protege comunicações com autenticação e criptografia. | [TLS](protocolos/tls.md) |
| SNI | Server Name Indication | Extensão TLS que informa o nome pretendido no ClientHello. | [TLS](protocolos/tls.md) |
| QUIC | Quick UDP Internet Connections | Transporte seguro baseado em UDP usado pelo HTTP/3. | [Transporte](protocolos/camada-transporte.md) |
| HTTP/1.1 | Hypertext Transfer Protocol version 1.1 | Protocolo web textual definido principalmente pela RFC 9112. | [HTTP](protocolos/http.md) |
| HTTP/2 | Hypertext Transfer Protocol version 2 | HTTP multiplexado em frames binários sobre uma conexão. | [HTTP](protocolos/http.md) |
| HTTP/3 | Hypertext Transfer Protocol version 3 | HTTP sobre QUIC, com transporte criptografado. | [HTTP](protocolos/http.md) |
| SMTP | Simple Mail Transfer Protocol | Envio e retransmissão de mensagens de e-mail. | [E-mail](protocolos/email.md) |
| SPF | Sender Policy Framework | Política DNS que autoriza hosts para uso de domínio no envelope SMTP. | [E-mail](protocolos/email.md) |
| DKIM | DomainKeys Identified Mail | Assinatura criptográfica associada a domínios em mensagens. | [E-mail](protocolos/email.md) |
| DMARC | Domain-based Message Authentication, Reporting and Conformance | Política e relatórios que alinham identificadores SPF/DKIM com o domínio From. | [E-mail](protocolos/email.md) |
| OAuth | OAuth 2.0 Authorization Framework | Delegação de autorização por tokens, não é protocolo de autenticação por si só. | [Autenticação](protocolos/autenticacao.md) |
| OIDC | OpenID Connect | Camada de identidade/autenticação sobre OAuth 2.0. | [Autenticação](protocolos/autenticacao.md) |
| SAML | Security Assertion Markup Language | Padrão XML de troca de afirmações de identidade e autorização. | [Autenticação](protocolos/autenticacao.md) |
| JWT | JSON Web Token | Formato compacto de claims; pode ser assinado/MACado ou criptografado. | [Autenticação](protocolos/autenticacao.md) |
| CORS | Cross-Origin Resource Sharing | Mecanismo do navegador que controla acesso cross-origin a respostas. | [CORS](cabecalhos/cors.md) |
| CSP | Content Security Policy | Política de navegador para restringir fontes e comportamentos de conteúdo. | [HTTP segurança](cabecalhos/http-seguranca.md) |
| HSTS | HTTP Strict Transport Security | Política que instrui navegadores a usar HTTPS para um host. | [HTTP segurança](cabecalhos/http-seguranca.md) |
| XFO | X-Frame-Options | Header legado para restringir renderização em frames. | [HTTP segurança](cabecalhos/http-seguranca.md) |
| XCTO | X-Content-Type-Options | Header que, com `nosniff`, evita inferência MIME em contextos cobertos. | [HTTP segurança](cabecalhos/http-seguranca.md) |
| COOP | Cross-Origin-Opener-Policy | Isola contextos de navegação em grupos de browsing context. | [HTTP segurança](cabecalhos/http-seguranca.md) |
| COEP | Cross-Origin-Embedder-Policy | Controla requisitos para carregar recursos cross-origin em contexto isolado. | [HTTP segurança](cabecalhos/http-seguranca.md) |
| CORP | Cross-Origin-Resource-Policy | Permite ao recurso limitar carregamento cross-origin. | [HTTP segurança](cabecalhos/http-seguranca.md) |
| Referrer-Policy | Referrer Policy | Controla quanto do URL de origem é enviado no header Referer. | [HTTP segurança](cabecalhos/http-seguranca.md) |
| Permissions-Policy | Permissions Policy | Controla disponibilidade de recursos de plataforma web. | [HTTP segurança](cabecalhos/http-seguranca.md) |

## Expansões relacionadas

- **LDAP** (*Lightweight Directory Access Protocol*), **RADIUS** (*Remote Authentication Dial-In User Service*) e **Kerberos** são tecnologias distintas de diretório/autenticação; veja [autenticação](protocolos/autenticacao.md).
- **IMAP** (*Internet Message Access Protocol*) e **POP3** (*Post Office Protocol version 3*) acessam caixas postais; veja [e-mail](protocolos/email.md).
- **ALPN** (*Application-Layer Protocol Negotiation*) negocia protocolo de aplicação no TLS; veja [TLS](protocolos/tls.md).