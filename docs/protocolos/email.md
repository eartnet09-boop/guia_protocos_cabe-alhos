# E-mail

## 1. O que é

SMTP transporta mensagens entre agentes ([RFC 5321](https://www.rfc-editor.org/rfc/rfc5321)); IMAP acessa e sincroniza caixas ([RFC 9051](https://www.rfc-editor.org/rfc/rfc9051)); POP3 recupera mensagens ([RFC 1939](https://www.rfc-editor.org/rfc/rfc1939)). SPF ([RFC 7208](https://www.rfc-editor.org/rfc/rfc7208)), DKIM ([RFC 6376](https://www.rfc-editor.org/rfc/rfc6376)) e DMARC ([RFC 7489](https://www.rfc-editor.org/rfc/rfc7489)) ajudam a avaliar identidade de domínio, mas não garantem que mensagem seja benigna.

## 2. Como funciona

```mermaid
sequenceDiagram
  participant M as MUA remetente
  participant A as MTA remetente
  participant B as MTA destinatário
  participant U as Caixa/MUA destinatário
  M->>A: submissão SMTP autenticada
  A->>B: SMTP via MX (STARTTLS quando negociado)
  B->>B: SPF/DKIM/DMARC conforme política
  B-->>U: entrega; IMAP ou POP3 para acesso
```

SPF verifica autorização do IP remetente para domínio do envelope; DKIM verifica assinatura e domínio `d=`; DMARC verifica alinhamento com o domínio visível em From e aplica política/reporting. O resultado depende de DNS, encaminhamento e implementação.

| Mecanismo | Evidência | Limite |
|---|---|---|
| SPF | IP autorizado pelo domínio do envelope | Encaminhamento pode quebrar SPF; não autentica o From visível isoladamente. |
| DKIM | Assinatura de conteúdo/campos selecionados | Conteúdo pode estar assinado por domínio que não é o esperado. |
| DMARC | Alinhamento + política e relatórios | Não detecta phishing enviado por domínio legítimo comprometido. |

## 3. Relevância de segurança

Spoofing, phishing e roubo de sessão são riscos centrais. STARTTLS oportunístico cifra o hop quando negociado, mas não é garantia de criptografia ponta a ponta. Cabeçalhos podem ser adicionados por hops diferentes e a confiança depende do limite administrativo.

## 4. Ataques comuns

- **Spoofing de domínio**: política ausente/fraca ou falha de alinhamento; examine Authentication-Results inserido pelo receptor confiável.
- **Phishing/BEC**: mensagem pode autenticar corretamente; indicadores incluem Reply-To divergente, urgência incomum, domínio semelhante e solicitação fora de processo.
- **Roubo de credenciais**: links/anexos suspeitos; correlacione detecção de endpoint, sandbox corporativo e relato do usuário.

## 5. O que monitorar

Resultados SPF/DKIM/DMARC, alinhamento, remetente de envelope, From/Reply-To, Received hops, URL/domínio, anexos e tráfego de submissão. Preserve a mensagem original e seus headers; não clique em links para “testar”.

## 6. Como inspecionar na prática

Veja MX/TXT com `dig` em [como inspecionar](../como-inspecionar.md). Em cliente de e-mail, exiba “original”/“fonte da mensagem” e leia `Received` de baixo para cima; confie apenas nos resultados adicionados por sua infraestrutura receptora. Veja [headers de e-mail](../cabecalhos/email-headers.md).

## 7. Erros comuns / más configurações

Tratar SPF pass como selo de legitimidade; publicar `+all`; definir DMARC sem inventariar remetentes; confiar em um Authentication-Results externo; expor mensagem original com dados pessoais.

## 8. Links oficiais

[SMTP RFC 5321](https://www.rfc-editor.org/rfc/rfc5321) · [IMAP RFC 9051](https://www.rfc-editor.org/rfc/rfc9051) · [POP3 RFC 1939](https://www.rfc-editor.org/rfc/rfc1939) · [SPF RFC 7208](https://www.rfc-editor.org/rfc/rfc7208) · [DKIM RFC 6376](https://www.rfc-editor.org/rfc/rfc6376) · [DMARC RFC 7489](https://www.rfc-editor.org/rfc/rfc7489) · [ARC RFC 8617](https://www.rfc-editor.org/rfc/rfc8617).