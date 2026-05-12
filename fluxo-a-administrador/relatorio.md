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
| Nome | Danilo de Gois Neves |
| RA | 245555 |
| Disciplina | Redes de Computadores |
| Turma | B-Noturno |
| Data | 11/05/2026 |
| Fluxo | **A — Aluno com privilégio de administrador** |
| SO utilizado | [Windows 10 / Windows 11] |
| Ferramenta de proxy | Fiddler Classic |
| Navegador(es) | [Chrome / Edge / Firefox / ...] |
| Decriptação HTTPS habilitada? | sim |
| Certificado Fiddler instalado durante a atividade? | sim |

---

## Atividade 1 — Primeira captura

### Captura

<img width="1920" height="1008" alt="ex1" src="https://github.com/user-attachments/assets/5fe9acec-54ad-4f78-a4d0-b8207ac82649" />


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
| Host | informar o site |
| User-Agent | informar o navegador |
| Accept-Language | informar os idiomas suportados |

**Resposta:**

| Campo | Valor observado |
|---|---|
| `Content-Type` | text/html |
| `Content-Length` ou `Transfer-Encoding` | chunked |

---

## Atividade 2 — Anatomia de um GET

### Captura

<img width="1920" height="1013" alt="ex2" src="https://github.com/user-attachments/assets/08a809b2-a0fc-474a-b3a8-d050a2434db1" />


**Request-line completa:**

```http
GET https://httpbingo.org/get?aluno=Danilo&curso=redes HTTP/1.1
Host: httpbingo.org
Connection: keep-alive
sec-ch-ua: "Chromium";v="142", "Microsoft Edge";v="142", "Not_A Brand";v="99"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36 Edg/142.0.0.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br, zstd
Accept-Language: pt-BR,pt;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6
Pragma: no-cache

```

**Cabeçalhos-chave:**

| Cabeçalho | Valor |
|---|---|
| `Host` | httpbingo.org |
| `User-Agent` | Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36 Edg/142.0.0.0 |
| `Accept` | text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7 |

**Campos do JSON de resposta:**

```json
{
  "args": {
    "aluno": [
      "Danilo"
    ],
    "curso": [
      "redes"
    ]
  },
  "headers": {
    "Accept": [
      "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7"
    ],
    "Accept-Encoding": [
      "gzip, deflate, br, zstd"
    ],
    "Accept-Language": [
      "pt-BR,pt;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6"
    ],
  "origin": "187.92.211.170"
}
```

**Resposta curta:** o que o campo `origin` representa? O `User-Agent` retornado coincide com o enviado?

[resposta]
a origem da requisição, sim
---

## Atividade 3 — POST e envio de formulário

### Captura

<img width="1920" height="968" alt="ex3" src="https://github.com/user-attachments/assets/31117a7f-2977-47bb-a228-aab562682eb1" />


**Request-line do POST:**

```http
POST https://httpbingo.org/post HTTP/1.1
Host: httpbingo.org
Connection: keep-alive
Content-Length: 185
Cache-Control: max-age=0
sec-ch-ua: "Chromium";v="142", "Microsoft Edge";v="142", "Not_A Brand";v="99"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Origin: https://httpbingo.org
Content-Type: application/x-www-form-urlencoded
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36 Edg/142.0.0.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://httpbingo.org/forms/post
Accept-Encoding: gzip, deflate, br, zstd
Accept-Language: pt-BR,pt;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6
Pragma: no-cache

custname=Danilo&custtel=13999988855&custemail=marcosdomato%40gmail.com&size=medium&topping=bacon&topping=cheese&topping=onion&topping=mushroom&delivery=19%3A00&comments=bota+no+porteiro
```

| Cabeçalho | Valor |
|---|---|
| `Content-Type` | application/json; charset=utf-8 |
| `Content-Length` | 185 |

**Corpo do request:**

```text
POST https://httpbingo.org/post HTTP/1.1
Host: httpbingo.org
Connection: keep-alive
Content-Length: 185
Cache-Control: max-age=0
sec-ch-ua: "Chromium";v="142", "Microsoft Edge";v="142", "Not_A Brand";v="99"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Origin: https://httpbingo.org
Content-Type: application/x-www-form-urlencoded
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36 Edg/142.0.0.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://httpbingo.org/forms/post
Accept-Encoding: gzip, deflate, br, zstd
Accept-Language: pt-BR,pt;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6
Pragma: no-cache

custname=Danilo&custtel=13999988855&custemail=marcosdomato%40gmail.com&size=medium&topping=bacon&topping=cheese&topping=onion&topping=mushroom&delivery=19%3A00&comments=bota+no+porteiro
```

**Campo `form` da resposta:**

```json
{
    "comments": [
      "bota no porteiro"
    ],
    "custemail": [
      "marcosdomato@gmail.com"
    ],
    "custname": [
      "Danilo"
    ],
    "custtel": [
      "13999988855"
    ],
    "delivery": [
      "19:00"
    ],
    "size": [
      "medium"
    ],
    "topping": [
      "bacon",
      "cheese",
      "onion",
      "mushroom"
    ]
  },
  "json": null
}
```

**Resposta curta:** qual formato codifica o corpo? Qual aba mostra literalmente os bytes enviados: `WebForms` ou `Raw`?

[resposta]
application/x-www-form-urlencoded, raw
---

## Atividade 4 — Status codes

### Captura

<img width="400" height="119" alt="ex4" src="https://github.com/user-attachments/assets/a5faad51-dc30-4da5-8b6d-93ce3f9f5f90" />


| # | Método | URL | Status-line | Tamanho/body |
|---|---|---|---|---|
| 1 | GET | `https://httpbingo.org/status/200` | HTTP/1.1 200 OK | 0 |
| 2 | GET | `https://httpbingo.org/redirect-to?status_code=301&url=/get` | HTTP/1.1 301 Moved Permanently | 0 |
| 3 | GET | `https://httpbingo.org/status/404` | HTTP/1.1 404 Not Found | 0 |
| 4 | GET | `https://httpbingo.org/status/500` | HTTP/1.1 500 Internal Server Error | 0 |

**Resposta curta:** no `301`, qual cabeçalho informa o destino do redirecionamento?

Location

---

## Atividade 5 — Cabeçalhos essenciais

### Captura

<!-- arraste a captura aqui: Inspectors → Headers -->

| Cabeçalho | Req/Resp | Valor capturado | Função |
|---|---|---|---|
| `Host` | httpbingo.org | [...] | [...] |
| `User-Agent` | [...] | [...] | [...] |
| `Accept` | [...] | [...] | [...] |
| `Content-Type` | [...] | [...] | [...] |
| `Content-Length` / `Transfer-Encoding` | [...] | [...] | [...] |
| `Content-Encoding` | [...] | [...] | [...] |
| `Set-Cookie` | [...] | [...] | [...] |
| `Cache-Control` | [...] | [...] | [...] |
| `Strict-Transport-Security` | [...] | [...] | [...] |

**Resposta curta:** qual é o papel de `Content-Encoding` e de `Strict-Transport-Security`?

[resposta]

---

## Atividade 6 — HTTP vs HTTPS

### Captura — HTTP puro

<!-- arraste a captura aqui: http://httpbingo.org/get -->

### Captura — HTTPS sem decriptação

<!-- arraste a captura aqui: https://httpbingo.org/get sem decriptação -->

### Captura — HTTPS com decriptação

<!-- arraste a captura aqui: https://httpbingo.org/get com decriptação -->

| Situação | O que ficou visível? | O que ficou oculto? |
|---|---|---|
| HTTP puro | [...] | [...] |
| HTTPS sem decriptação | [...] | [...] |
| HTTPS com decriptação | [...] | [...] |

**Resposta curta:** por que a decriptação HTTPS pelo Fiddler exige instalar um certificado raiz?

[resposta]

---

## Atividade 7 — Cookies e sessão

### Captura

<!-- arraste a captura aqui: sequência cookies/set e cookies -->

| # | URL | `Set-Cookie` recebido | `Cookie` enviado |
|---|---|---|---|
| 1 | `/cookies/set?...` | [...] | [...] |
| 2 | `/cookies` | [...] | [...] |
| 3 | `/cookies` após recarregar | [...] | [...] |

**Resposta curta:** `Set-Cookie` apareceu em toda requisição ou apenas quando o servidor definiu/atualizou cookies? Quais atributos foram observados?

[resposta]

---

## Atividade 8 — Manipulação simples com breakpoint *(Opcional)*

### Captura

<!-- arraste a captura aqui: breakpoint com User-Agent editado -->

**JSON de resposta:**

```json
{
  "user-agent": ["[valor observado]"]
}
```

**Resposta curta:** o que este teste mostra sobre o papel ativo de um proxy?

[resposta]

- [ ] Breakpoints desabilitados ao final

---

## Reflexão final (opcional)

[até 10 linhas]

---

## Encerramento — Higiene de segurança

### Captura antes da remoção

<!-- arraste aqui a captura do certmgr.msc mostrando DO_NOT_TRUST_FiddlerRoot presente -->

### Captura depois da remoção

<!-- arraste aqui a captura mostrando o certificado ausente -->

- [ ] `Decrypt HTTPS traffic` desabilitado no Fiddler
- [ ] Certificado `DO_NOT_TRUST_FiddlerRoot` removido do Windows
- [ ] Certificado `DO_NOT_TRUST_FiddlerRoot` removido do Firefox, se aplicável
- [ ] Fiddler fechado

**Por que esta etapa é importante?**

[resposta curta]

---

## Checklist de entrega

- [ ] Campos `[...]` substituídos
- [ ] Capturas inseridas
- [ ] Atividades 1 a 7 preenchidas; Atividade 8 preenchida se executada
- [ ] Encerramento com duas capturas concluído
- [ ] PDF gerado como `SOBRENOME_NOME_RA_LAB_HTTP_FLUXOA.pdf`
- [ ] PDF submetido no Microsoft Teams
