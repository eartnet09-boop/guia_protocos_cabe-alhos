# Headers de e-mail

## 1. O que é / para que serve

Campos RFC 5322 ([RFC 5322](https://www.rfc-editor.org/rfc/rfc5322)) e extensões de autenticação/roteamento. Um header visível não é automaticamente confiável: cada hop acrescenta informação e a confiança depende do servidor que a inseriu.

## 2. Sintaxe

| Campo | Exemplo sintético | Leitura defensiva |
|---|---|---|
| `Received` | `Received: from mx.example by inbound.example; Tue, 10 Nov 2026 10:00:00 +0000` | Hops são normalmente lidos de baixo para cima; os mais antigos podem ser forjados. |
| `Return-Path` | `Return-Path: <bounce@example.net>` | Endereço de envelope registrado na entrega. |
| `From` | `From: Equipe <aviso@example.com>` | Identidade visível; não prova origem. |
| `Reply-To` | `Reply-To: resposta@example.net` | Destino de resposta; divergência merece contexto. |
| `Authentication-Results` | `Authentication-Results: mx.example; spf=pass; dkim=pass; dmarc=pass` | Confie no campo inserido pelo seu receptor. |
| `DKIM-Signature` | `DKIM-Signature: v=1; a=rsa-sha256; d=example.com; s=mail; ...` | Assinatura; campos `h=` e `bh=` participam da validação. |
| `ARC-Seal` | `ARC-Seal: i=1; ...` | Sela conjunto ARC. |
| `ARC-Message-Signature` | `ARC-Message-Signature: i=1; ...` | Assinatura da mensagem numa cadeia ARC. |
| `ARC-Authentication-Results` | `ARC-Authentication-Results: i=1; mx.example; ...` | Resultado de autenticação preservado no ARC. |

Exemplos são ilustrativos, não assinaturas válidas. ARC é especificado na [RFC 8617](https://www.rfc-editor.org/rfc/rfc8617).

## 3. Valor seguro recomendado

Não existe valor configurável universal nos headers recebidos. Publique políticas SPF/DKIM/DMARC coerentes e valide Authentication-Results no MTA de confiança; ARC informa resultados intermediários, não substitui política local.

## 4. Ausência ou má configuração

Campos ausentes ou não confiáveis reduzem evidência; confiar em From ou em Authentication-Results recebido de fora facilita spoofing. `Received` inferior pode ser forjado antes do ingresso na infraestrutura confiável.

## 5. Como verificar

Exiba “mostrar original” no cliente e preserve fonte completa em sistema de evidências protegido. Leia hops de baixo para cima até o limite confiável; compare From, Reply-To, Return-Path, resultados SPF/DKIM/DMARC e domínio assinado. Analise URLs sem abri-las e redija endereços pessoais ao compartilhar.

## 6. Links oficiais

[RFC 5322](https://www.rfc-editor.org/rfc/rfc5322) · [SPF RFC 7208](https://www.rfc-editor.org/rfc/rfc7208) · [DKIM RFC 6376](https://www.rfc-editor.org/rfc/rfc6376) · [DMARC RFC 7489](https://www.rfc-editor.org/rfc/rfc7489) · [ARC RFC 8617](https://www.rfc-editor.org/rfc/rfc8617).