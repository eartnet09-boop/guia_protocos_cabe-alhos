# TLS

## 1. O que é

TLS (Transport Layer Security) negocia chaves e protege confidencialidade/integridade; TLS 1.3 está na [RFC 8446](https://www.rfc-editor.org/rfc/rfc8446), TLS 1.2 na [RFC 5246](https://www.rfc-editor.org/rfc/rfc5246). Requisitos para autenticação de serviço: [RFC 9525](https://www.rfc-editor.org/rfc/rfc9525).

## 2. Como funciona

```mermaid
sequenceDiagram
  participant C as Cliente
  participant S as Servidor
  C->>S: ClientHello (versões, suites, SNI, ALPN)
  S-->>C: ServerHello e parâmetros de chave
  S-->>C: certificado e prova de posse (TLS 1.3)
  C->>C: valida cadeia, nome e período
  S-->>C: Finished
  C->>S: Finished; dados de aplicação cifrados
```

TLS 1.2 pode usar diferentes fluxos conforme cipher suite; TLS 1.3 remove algoritmos legados e cifra mensagens de handshake após ServerHello (RFC 8446). SNI ([RFC 6066](https://www.rfc-editor.org/rfc/rfc6066)) indica o nome virtual; ALPN ([RFC 7301](https://www.rfc-editor.org/rfc/rfc7301)) negocia protocolo. ECH pode proteger ClientHello/SNI, conforme suporte.

| Versão | Segurança/observabilidade |
|---|---|
| TLS 1.2 | Ainda pode ser seguro com configuração moderna; mais opções e mensagens de handshake visíveis. |
| TLS 1.3 | Handshake mais simples e seguro por padrão; parte maior do handshake fica cifrada após ServerHello. |

## 3. Relevância de segurança

Certificado válido não garante aplicação segura; cliente precisa validar cadeia e nome. TLS esconde payload, headers e caminhos do SOC passivo; SNI/endereços podem ainda ser visíveis, salvo ECH. Pinning pode reduzir confiança em CAs, mas aumenta risco operacional de rotação. Proxy TLS corporativo termina TLS e exige governança, confiança e transparência.

## 4. Ataques comuns

- **Downgrade**: negociação forçada a opção fraca; indícios em telemetria de versões antigas e falhas de handshake. TLS 1.3 usa mecanismos de proteção previstos na RFC 8446.
- **MITM por certificado/proxy indevido**: cadeia/nome não confiável; investigue emissor, SAN e trust store efetivamente usado.
- **Certificado expirado/mal emitido**: alertas de validade, nome ou cadeia; correlacione com inventário e CT quando aplicável.

## 5. O que monitorar

Versão, cipher, SNI/ALPN quando visíveis, alertas, emissor/serial/validade do certificado, falhas e mudanças de JA3/JA4 (fingerprints não são identidade). Capture `tls.handshake.type == 1`; QUIC usa TLS integrado, não um handshake TLS sobre TCP.

## 6. Como inspecionar na prática

Use `openssl s_client` e opções de curl em [como inspecionar](../como-inspecionar.md). A validação deve incluir hostname e cadeia; evite `-k`/`--insecure` como evidência de segurança.

## 7. Erros comuns / más configurações

Aceitar certificados sem validar nome; habilitar versões/ciphers obsoletos; não renovar certificados; interpretar SNI como sempre visível; confundir certificado wildcard com autorização para qualquer serviço.

## 8. Links oficiais

[TLS 1.3 RFC 8446](https://www.rfc-editor.org/rfc/rfc8446) · [TLS 1.2 RFC 5246](https://www.rfc-editor.org/rfc/rfc5246) · [SNI RFC 6066](https://www.rfc-editor.org/rfc/rfc6066) · [ALPN RFC 7301](https://www.rfc-editor.org/rfc/rfc7301) · [RFC 9525](https://www.rfc-editor.org/rfc/rfc9525).