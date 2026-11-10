# Como inspecionar

Faça captura e testes apenas em redes e sistemas sob sua administração ou com autorização. Capturas podem conter credenciais, cookies e dados pessoais; minimize, proteja e descarte conforme a política aplicável.

Use `HOST` como domínio autorizado de laboratório. Filtros de exibição do Wireshark atuam sobre uma captura; filtros de captura limitam o que será coletado.

## Wireshark e tshark

Filtros de exibição úteis (a visibilidade depende de criptografia e da versão do dissector):

```text
dns
dns.qry.name contains "example"
tcp.flags.syn == 1 && tcp.flags.ack == 0
icmp || icmpv6
tls.handshake.type == 1
tls.handshake.extensions_server_name
http.request.method == "POST"
http.response.code >= 400
```

Filtro de captura BPF para limitar a tráfego de/para um host e portas de laboratório:

```text
host 192.0.2.10 and (tcp port 443 or udp port 53)
```

`192.0.2.0/24` é espaço reservado para documentação (RFC 5737); substitua por endereço autorizado. Em Wireshark: selecione um pacote e use **Analyze > Follow > TCP Stream**. Para exportar objetos HTTP sem abrir conteúdo de terceiros: **File > Export Objects > HTTP**; o menu só aparece se o dissector identificar HTTP visível.

```sh
tshark -i eth0 -f 'host 192.0.2.10 and port 443' -w captura.pcapng
tshark -r captura.pcapng -Y 'tls.handshake.extensions_server_name' -T fields -e frame.time -e ip.src -e tls.handshake.extensions_server_name
```

Não espere ver caminhos, headers HTTP ou payload dentro de TLS/QUIC sem decriptação autorizada e chaves apropriadas. SNI pode ser protegido por ECH, quando negociado.

## curl

```sh
curl -I https://$HOST/
curl -v --max-time 10 https://$HOST/ -o /dev/null
curl --http2 -I https://$HOST/
curl --tlsv1.3 --tls-max 1.3 -I https://$HOST/
curl -sS -D - -o /dev/null https://$HOST/
curl -sS -D - -o /dev/null -H 'Origin: https://untrusted.example' https://$HOST/
```

`-I` pede HEAD, não necessariamente reproduz a resposta a GET. `-v` mostra detalhes da negociação e headers, mas cuidado com dados sensíveis. Um `Origin` de teste só avalia a resposta do endpoint autorizado; CORS é aplicado pelo navegador, não por curl.

## OpenSSL

```sh
openssl s_client -connect "$HOST:443" -servername "$HOST" -brief </dev/null
openssl s_client -connect "$HOST:443" -servername "$HOST" -showcerts </dev/null
openssl s_client -connect "$HOST:443" -servername "$HOST" -tls1_3 </dev/null
openssl s_client -connect "$HOST:443" -servername "$HOST" -tls1_2 </dev/null
```

Verifique subject/SAN, emissor, validade, cadeia, protocolo e cipher. A aceitação de um certificado pelo cliente depende da cadeia de confiança e do nome de referência (RFC 9525); `s_client` é ferramenta de diagnóstico, não substitui a validação da aplicação.

## dig e nslookup

```sh
dig "$HOST" A
dig "$HOST" AAAA
dig "$HOST" MX
dig "$HOST" TXT
dig @1.1.1.1 "$HOST" A
dig +trace "$HOST" A
dig "$HOST" ANY
nslookup -type=NS "$HOST"
```

`ANY` não é um inventário confiável de todos os registros: servidores podem responder HINFO mínimo ou limitar a resposta (RFC 8482). `+trace` consulta a hierarquia DNS; consultas a resolvedores públicos revelam o nome consultado a terceiros.

## DevTools

Na aba **Network**, habilite preservação do log, escolha a requisição e confira **Headers**, **Payload**, **Timing** e redirecionamentos. **Security** mostra conexão/certificado em navegadores compatíveis. **Application/Storage** permite inspecionar cookies e armazenamento local. Não copie cookies/tokens para tickets ou logs; compare `Set-Cookie` com os atributos efetivamente armazenados.

## Serviços online

Consulte [Security Headers](https://securityheaders.com/), [SSL Labs](https://www.ssllabs.com/ssltest/), [HSTS Preload](https://hstspreload.org/) e [MXToolbox](https://mxtoolbox.com/). São terceiros: submeter um host pode divulgar o domínio e os resultados podem ser armazenados; use apenas ativos autorizados e interprete os achados no contexto.

## Checklist de auditoria

1. Em um host autorizado, rode `curl -I https://$HOST/` e também teste a resposta GET se necessário.
2. Avalie presença e escopo de HSTS, CSP, X-Frame-Options, X-Content-Type-Options, Referrer-Policy e Permissions-Policy; não use presença como único critério.
3. Envie `Origin: https://untrusted.example` ao endpoint autorizado e examine `Access-Control-Allow-Origin` e `Access-Control-Allow-Credentials`; confirme comportamento no navegador.
4. Valide protocolo, certificado e cadeia TLS; use SSL Labs apenas se a divulgação externa for aceitável.
5. Registre URL, horário, método, cadeia de redirecionamento e evidências redigidas, sem tokens/cookies.