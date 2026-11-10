# Autenticação e identidade

## 1. O que é

OAuth 2.0 delega autorização ([RFC 6749](https://www.rfc-editor.org/rfc/rfc6749)); OIDC adiciona identidade sobre OAuth ([OpenID Connect Core](https://openid.net/specs/openid-connect-core-1_0.html)). SAML 2.0 troca assertions XML ([OASIS](https://docs.oasis-open.org/security/saml/v2.0/)); Kerberos autentica com tickets ([RFC 4120](https://www.rfc-editor.org/rfc/rfc4120)); RADIUS transporta autenticação/autorização ([RFC 2865](https://www.rfc-editor.org/rfc/rfc2865)); LDAP acessa diretórios ([RFC 4511](https://www.rfc-editor.org/rfc/rfc4511)).

## 2. Como funciona

```mermaid
sequenceDiagram
  participant U as Usuário/navegador
  participant C as Cliente
  participant I as IdP/servidor autorização
  U->>C: inicia login
  C->>I: autorização + redirect URI + state + PKCE
  I-->>U: autentica e consente conforme política
  I-->>C: redirect com authorization code
  C->>I: troca code + verificador PKCE
  I-->>C: tokens; OIDC ID token contém claims de identidade
```

OAuth Authorization Code com PKCE é recomendado para clientes públicos; PKCE está na [RFC 7636](https://www.rfc-editor.org/rfc/rfc7636) e BCP de segurança OAuth na [RFC 9700](https://www.rfc-editor.org/rfc/rfc9700). JWT ([RFC 7519](https://www.rfc-editor.org/rfc/rfc7519)) é formato de token, não protocolo de login. SAML usa assertion assinada; Kerberos usa tickets emitidos por KDC; RADIUS e LDAP integram sistemas com controles próprios.

## 3. Relevância de segurança

Tokens são credenciais bearer: exposição permite uso até expiração/revogação. Valide assinatura, algoritmo permitido, emissor, audiência, expiração e nonce conforme fluxo. Claims não confiáveis não devem definir privilégios. LDAP requer escaping/contexto correto e canal protegido.

## 4. Ataques comuns

- **OAuth redirect/token misuse**: URI aberta, state/PKCE ausentes ou token em local exposto; monitore redirects e validações rejeitadas.
- **JWT validation errors**: issuer/audience/algoritmo não verificados; monitore falhas de assinatura e claims incompatíveis. Não aceite algoritmo indicado sem política.
- **SAML assertion replay/audience error**: controles de validade, recipient, audience e replay inconsistentes; audite logs do SP/IdP.
- **Kerberos ticket abuse**: credenciais/segredos privilegiados comprometidos; sinais incluem uso anômalo de contas de serviço e tickets incomuns. Golden/silver ticket são categorias de abuso de material de chave; proteja KDC e secrets.
- **LDAP injection**: entrada incorporada em filtro; monitore filtros anômalos/erros e use APIs parametrizadas/escaping.

## 5. O que monitorar

Falhas/sucessos, IdP, client_id, redirect URI, issuer, audience, MFA, origem e emissão/uso de tokens sem registrar seus valores. Para diretório, eventos de bind, alterações de grupo e consultas administrativas.

## 6. Como inspecionar na prática

Use logs do IdP/SP e decodificador JWT local aprovado sem enviar tokens a sites públicos. Verifique claims contra metadados confiáveis e relógio. Testes de OAuth/SAML/LDAP devem ocorrer em tenant/lab autorizado; nunca cole tokens ou assertions em ferramentas online.

## 7. Erros comuns / más configurações

Confundir OAuth com autenticação; redirect URI por prefixo; omitir state/PKCE; aceitar JWT sem audience/issuer; guardar tokens em logs; assertion sem audience/replay control; LDAP concatenado; expor bind simples sem TLS.

## 8. Links oficiais

[OAuth RFC 6749](https://www.rfc-editor.org/rfc/rfc6749) · [OAuth Security BCP RFC 9700](https://www.rfc-editor.org/rfc/rfc9700) · [PKCE RFC 7636](https://www.rfc-editor.org/rfc/rfc7636) · [OIDC Core](https://openid.net/specs/openid-connect-core-1_0.html) · [JWT RFC 7519](https://www.rfc-editor.org/rfc/rfc7519) · [Kerberos RFC 4120](https://www.rfc-editor.org/rfc/rfc4120) · [RADIUS RFC 2865](https://www.rfc-editor.org/rfc/rfc2865) · [LDAP RFC 4511](https://www.rfc-editor.org/rfc/rfc4511) · [SAML OASIS](https://docs.oasis-open.org/security/saml/v2.0/).