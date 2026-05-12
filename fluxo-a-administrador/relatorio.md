# Relatório — Laboratório de Inspeção HTTP/HTTPS — Fluxo A (Administrador)

> **Como usar este template.** Preencha cada campo `[...]` com sua resposta e arraste as capturas de tela diretamente para os locais indicados. Preserve a formatação Markdown.
>
> **Escopo:** este fluxo inclui HTTP em texto claro, HTTPS sem decriptação e HTTPS com decriptação TLS pelo Fiddler Classic.

---

## Como anexar capturas de tela

1. Faça a captura de tela e salve como PNG.
2. No editor do GitHub ou GitHub.dev, posicione o cursor no local indicado.
3. Arraste o PNG para o editor. O GitHub inserirá uma linha `![image](...)`.

---

## Identificação

| Campo | Valor |
|---|---|
| Nome | Júlia Vieira Santos e Maria Clara Santana Vianna |
| RA | 245056 e 243911 |
| Disciplina | Redes de Computadores |
| Turma | B |
| Data | 11/05/2026 |
| Fluxo | **A — Aluno com privilégio de administrador** |
| SO utilizado | Windows 11 |
| Ferramenta de proxy | Fiddler Classic |
| Navegador(es) | Chrome |
| Decriptação HTTPS habilitada? | Sim |
| Certificado Fiddler instalado durante a atividade? | Sim |

---

## Atividade 1 — Primeira captura

### Captura

<img width="1168" height="612" alt="cap1" src="https://github.com/user-attachments/assets/22898bbd-395e-41cb-83ab-a5190da4c299" />


**Request-line:**

```http
GET http://example.com/ HTTP/1.1

```

**Status-line:**

```http
HTTP/1.1 200 OK
```

**Cabeçalhos do request:**

| Cabeçalho | Função |
|---|---|
| Host | Destino do request. |
| Connection | Determina se a conexão permanece aberta para próximas solicitações. |
| Upgrade-Insecure-Requests | Um sinal enviado pelo navegador para indicar ao servidor que ele tem preferência por uma resposta criptografada e autenticada.  |

**Resposta:**

| Campo | Valor observado |
|---|---|
| `Content-Type` | text/html |
| `Content-Length` ou `Transfer-Encoding` | chunked |

---

## Atividade 2 — Anatomia de um GET

### Captura

<!-- arraste a captura aqui: Request Raw e Response JSON -->

**Request-line completa:**

Request raw:
<img width="1507" height="384" alt="cap2req" src="https://github.com/user-attachments/assets/4cf970c3-05c1-4bda-b828-cce5abb72009" />

Response JSON:
<img width="745" height="933" alt="cap2" src="https://github.com/user-attachments/assets/2668b067-b90e-4237-b64e-705587e3cb88" />


**Cabeçalhos-chave:**

| Cabeçalho | Valor |
|---|---|
| `Host` | httpbingo.org |
| `User-Agent` | Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36 |
| `Accept` | text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7 |

**Campos do JSON de resposta:**

```json
{
  "args": aluno": [
      "JULIA_E_CLARA"
    ],
    "curso": [
      "redes",
  "headers": {
    "Accept": [
      "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7"
    ],
    "Accept-Encoding": [
      "gzip, deflate, br, zstd"
    ],
    "Accept-Language": [
      "pt-BR,pt;q=0.9,en-US;q=0.8,en;q=0.7"
    ],
    "Cache-Control": [
      "no-cache"
    ],
    "Connection": [
      "keep-alive"
    ],
    "Host": [
      "httpbingo.org"
    ],
    "Pragma": [
      "no-cache"
    ],
    "Sec-Ch-Ua": [
      "\"Chromium\";v=\"142\", \"Google Chrome\";v=\"142\", \"Not_A Brand\";v=\"99\""
    ],
    "Sec-Ch-Ua-Mobile": [
      "?0"
    ],
    "Sec-Ch-Ua-Platform": [
      "\"Windows\""
    ],
    "Sec-Fetch-Dest": [
      "document"
    ],
    "Sec-Fetch-Mode": [
      "navigate"
    ],
    "Sec-Fetch-Site": [
      "none"
    ],
    "Sec-Fetch-User": [
      "?1"
    ],
    "User-Agent": [
      "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36"
    ],
    "Via": [
      "1.1 fly.io, 1.1 fly.io"
    ],
    "X-Forwarded-For": [
      "187.92.211.170, 66.241.125.232"
    ],
    "X-Forwarded-Port": [
      "443"
    ],
    "X-Forwarded-Proto": [
      "https"
    ],
    "X-Forwarded-Ssl": [
      "on"
    ],
    "X-Request-Start": [
      "t=1778542386420388"
    ]
  },
  "origin": "187.92.211.170"
}
```

**Resposta curta:** o que o campo `origin` representa? O `User-Agent` retornado coincide com o enviado?

o IP pertencente ao servidor. Sim, coincide.

---

## Atividade 3 — POST e envio de formulário

### Captura

<img width="1512" height="893" alt="cap3" src="https://github.com/user-attachments/assets/637408af-7e84-49df-9bf3-93685b1d3fb3" />

**Request-line do POST:**

```http
POST https://httpbingo.org/post HTTP/1.1
```

| Cabeçalho | Valor |
|---|---|
| `Content-Type` | application/x-www-form-urlencoded |
| `Content-Length` | 176 |

**Corpo do request:**

```text
custname=J%C3%BAlia&custtel=1340028922&custemail=julia%40edu.com&size=large&topping=bacon&topping=cheese&topping=onion&topping=mushroom&delivery=21%3A00&comments=de+gra%C3%A7a+

```

**Campo `form` da resposta:**

```json
"form": {
    "comments": [
      "de graça "
    ],
    "custemail": [
      "julia@edu.com"
    ],
    "custname": [
      "Júlia"
    ],
    "custtel": [
      "1340028922"
    ],
    "delivery": [
      "21:00"
    ],
    "size": [
      "large"
    ],
    "topping": [
      "bacon",
      "cheese",
      "onion",
      "mushroom"
    ]
  }
```

**Resposta curta:** qual formato codifica o corpo? Qual aba mostra literalmente os bytes enviados: `WebForms` ou `Raw`?

o formato application/x-www-form-urlencoded, a aba `Raw`.

---

## Atividade 4 — Status codes

### Captura

<img width="403" height="467" alt="cap4" src="https://github.com/user-attachments/assets/eca155c1-faa2-44a1-b694-4e2b15dbb3f6" />



| # | Método | URL | Status-line | Tamanho/body |
|---|---|---|---|---|
| 1 | GET | `https://httpbingo.org/status/200` | HTTP/1.1 200 OK | 0 |
| 2 | GET | `https://httpbingo.org/redirect-to?status_code=301&url=/get` | HTTP/1.1 301 Moved Permanently | 0 |
| 3 | GET | `https://httpbingo.org/status/404` | HTTP/1.1 404 Not Found| 0 |
| 4 | GET | `https://httpbingo.org/status/500` | HTTP/1.1 500 Internal Server Error | 0 |

**Resposta curta:** no `301`, qual cabeçalho informa o destino do redirecionamento?

O cabeçalho location

---

## Atividade 5 — Cabeçalhos essenciais

### Captura

<img width="865" height="815" alt="cap5200" src="https://github.com/user-attachments/assets/8aa56fe7-8198-4eef-b2e9-64e3651c51ab" />

<img width="1077" height="908" alt="cap5404" src="https://github.com/user-attachments/assets/d8599356-2c7f-407d-8088-d539fb727638" />


| Cabeçalho | Req/Resp | Valor capturado | Função |
|---|---|---|---|
| `Host` | Req | httpbingo.org
  | Indica ao servidor qual domínio o cliente quer acessar |
| `User-Agent` | Req | Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36
 | Identifica o navegador, sistema operacional e dispositivo do cliente. |
| `Accept` | Req | text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
 | Informa quais tipos de conteúdo o cliente aceita receber na resposta (HTML, XML, imagens, etc.). |
| `Content-Type` | Resp | application/json; charset=utf-8 | Define o tipo de dado enviado na resposta (neste caso JSON) e a codificação de caracteres (utf-8). |
| `Content-Length` / `Transfer-Encoding` | Resp | chunked | Controla como o corpo da resposta é transmitido.|
| `Content-Encoding` | Resp | zstd | Indica que o conteúdo foi comprimido usando o algoritmo Zstandard |
| `Set-Cookie` | Resp | teste=1 | Faz o servidor enviar um cookie para ser armazenado no navegador. |
| `Cache-Control` | Resp | max-age=3600 | Define regras de cache. |
| `Strict-Transport-Security` | Resp | max-age=31536000 | Cabeçalho de segurança HTTPS (HSTS). |

**Resposta curta:** qual é o papel de `Content-Encoding` e de `Strict-Transport-Security`?

o Content Encoding reduz o tráfefo e acelera a transferência, e o Strict-Transport-Security ajuda a evitar ataques de downgrade para HTTP.

---

## Atividade 6 — HTTP vs HTTPS

### Captura — HTTP puro

<img width="1919" height="987" alt="cap61" src="https://github.com/user-attachments/assets/e6357c20-68c8-421c-bdad-888cc901fcdb" />

### Captura — HTTPS sem decriptação

<img width="1574" height="812" alt="cap62" src="https://github.com/user-attachments/assets/162f1acf-1ace-4805-ae4d-1231b5564c10" />

### Captura — HTTPS com decriptação

<img width="1569" height="926" alt="cap63" src="https://github.com/user-attachments/assets/0d8bfc16-c464-44fe-a97e-894968973eca" />

| Situação | O que ficou visível? | O que ficou oculto? |
|---|---|---|
| HTTP puro | Todos os campos | Nenhum campo |
| HTTPS sem decriptação | Informações de conexões e transporte | Campos relacionados à segurança (sec-fetch) |
| HTTPS com decriptação | Campos relacionados à segurança (sec-fetch) | Nenhum campo |

**Resposta curta:** por que a decriptação HTTPS pelo Fiddler exige instalar um certificado raiz?

O Fiddler atua como um intermediário legítimo entre o seu navegador/aplicativo e o servidor destino, interceptando seu tráfego (um ataque "Man-in-the-Middle" técnico).

---

## Atividade 7 — Cookies e sessão

### Captura

<img width="448" height="185" alt="cap72" src="https://github.com/user-attachments/assets/37ccab9d-7f1e-41f3-a888-c2ba136ade64" />
<img width="343" height="346" alt="cap71" src="https://github.com/user-attachments/assets/8f813eb8-e661-4973-9576-108e50c3b35f" />
<img width="975" height="320" alt="cap73" src="https://github.com/user-attachments/assets/1397c810-29c9-43e5-857e-2a4b9a650f55" />


| # | URL | `Set-Cookie` recebido | `Cookie` enviado |
|---|---|---|---|
| 1 | `[/cookies/set?...](https://httpbingo.org/cookies/set?disciplina=redes&professor=claudio)` | set-cookie: disciplina=redes; HttpOnly set-cookie: professor=claudio; HttpOnly | Cookie: teste=1 |
| 2 | `[/cookies](https://httpbingo.org/cookies HTTP/1.1)` | Nenhum | Cookie: disciplina=redes; professor=claudio; teste=1 |
| 3 | `[/cookies](https://httpbingo.org/cookies HTTP/1.1)` após recarregar | Nenhum | Cookie: disciplina=redes; professor=claudio; teste=1 |

**Resposta curta:** `Set-Cookie` apareceu em toda requisição ou apenas quando o servidor definiu/atualizou cookies? Quais atributos foram observados?

Só apareceu quando definiu/atualizou os cookies. O 'Cookie' em Req e o 'set-cookie' em Resp

---
