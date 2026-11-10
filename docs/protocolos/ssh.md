# SSH

## 1. O que é

SSH (Secure Shell) protege login remoto e canais encaminhados. Arquitetura e transporte estão nas [RFCs 4251](https://www.rfc-editor.org/rfc/rfc4251), [4252](https://www.rfc-editor.org/rfc/rfc4252), [4253](https://www.rfc-editor.org/rfc/rfc4253) e [4254](https://www.rfc-editor.org/rfc/rfc4254).

## 2. Como funciona

```mermaid
sequenceDiagram
  participant C as Cliente
  participant S as Servidor
  C->>S: versões e algoritmos
  C->>S: troca de chave
  S-->>C: host key; cliente valida known_hosts
  C->>S: autenticação (ex.: chave pública)
  S-->>C: canal de sessão/encaminhamento
```

SSH negocia algoritmos, estabelece chaves de sessão e autentica host/usuário. Em chave pública, servidor verifica prova criptográfica e autorização da chave (RFC 4252). Port forwarding cria canais, não torna automaticamente seguro o serviço encaminhado.

## 3. Relevância de segurança

Chaves privadas roubadas, host key não validada e túneis não monitorados permitem acesso persistente ou bypass de segmentação. A criptografia limita inspeção de comandos no caminho; auditoria deve ocorrer no endpoint/servidor.

## 4. Ataques comuns

- **MITM por host key não verificada**: usuário aceita chave inesperada; indicador é mudança de fingerprint sem rotação planejada.
- **Password guessing/credential reuse**: falhas repetidas e origens incomuns nos logs de autenticação.
- **Abuso de port forwarding**: canais não aprovados; monitore opções SSH, processos e destinos acessados.

## 5. O que monitorar

Usuário, origem, método, resultado, fingerprint, algoritmo, horário, comandos/sessões via auditoria autorizada, alterações em `authorized_keys` e encaminhamentos. Filtro Wireshark `tcp.port == 22` revela início/metadata, não comandos após cifra.

## 6. Como inspecionar na prática

```sh
ssh -vv -o BatchMode=yes usuario@host-autorizado
ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub
```

Use `ssh -G host` para ver configuração efetiva do cliente. Conecte somente a hosts autorizados; valide fingerprint por canal independente.

## 7. Erros comuns / más configurações

Desabilitar verificação de host; reutilizar chave privada; permitir login root/senha sem necessidade; chaves sem rotação; encaminhamento global sem justificativa; não limitar usuários e algoritmos conforme compatibilidade.

## 8. Links oficiais

[RFC 4251](https://www.rfc-editor.org/rfc/rfc4251) · [RFC 4252](https://www.rfc-editor.org/rfc/rfc4252) · [RFC 4253](https://www.rfc-editor.org/rfc/rfc4253) · [RFC 4254](https://www.rfc-editor.org/rfc/rfc4254) · [OpenSSH manual](https://man.openbsd.org/ssh).