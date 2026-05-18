# Laboratório: Inspeção de HTTP/HTTPS com *Debugging Proxy*

> **Disciplina:** Redes de Computadores  
> **Professor:** Claudio Nunes  
> **Tópico:** Camada de Aplicação — Protocolos HTTP/1.1 e HTTP/2 sobre TLS

Este repositório contém a fundamentação teórica, o roteiro prático e a template de relatório da atividade de inspeção de HTTP/HTTPS com Fiddler Classic.

---

## Sumário

- [1. Objetivos de Aprendizagem](#1-objetivos-de-aprendizagem)
- [2. Pré-requisitos](#2-pré-requisitos)
- [3. Roteiro e entrega](#3-roteiro-e-entrega)
- [4. Fundamentação Teórica](#4-fundamentação-teórica)
  - [4.1. O protocolo HTTP](#41-o-protocolo-http)
  - [4.2. Anatomia de uma mensagem HTTP](#42-anatomia-de-uma-mensagem-http)
  - [4.3. Métodos (verbos) HTTP](#43-métodos-verbos-http)
  - [4.4. Códigos de status](#44-códigos-de-status)
  - [4.5. Principais cabeçalhos](#45-principais-cabeçalhos)
  - [4.6. O papel do *debugging proxy*](#46-o-papel-do-debugging-proxy)
- [5. Referências](#5-referências)
- [Anexo A — Tabela-resumo de cabeçalhos](#anexo-a--tabela-resumo-de-cabeçalhos)
- [Anexo B — Tabela-resumo de status codes](#anexo-b--tabela-resumo-de-status-codes)

---

## 1. Objetivos de Aprendizagem

Ao final desta atividade, o aluno será capaz de:

1. **Descrever** a estrutura de uma mensagem HTTP (linha inicial, cabeçalhos, corpo) em nível de bytes.
2. **Identificar** métodos, códigos de status e cabeçalhos de requests/responses reais.
3. **Utilizar** o Fiddler Classic para capturar e inspecionar tráfego HTTP/HTTPS.
4. **Diferenciar** o comportamento de HTTP e HTTPS na camada de transporte.
5. **Analisar** fluxos como navegação web, submissão de formulário e manutenção de sessão via cookies.

---

## 2. Pré-requisitos

### Conhecimentos prévios

- Modelo TCP/IP e camada de aplicação.
- Conceito de cliente/servidor e sockets TCP.
- Noções básicas de TLS/SSL.

### Ambiente técnico

- Sistema operacional: Windows 10/11.
- Privilégio de administrador para abrir o Fiddler Classic e instalar/remover temporariamente o certificado raiz do Fiddler.
- Navegador Chrome, Firefox ou Edge atualizado.
- Acesso à internet sem bloqueio ao domínio `http.aulasrede.com.br`.
- Fiddler Classic instalado conforme orientação do professor ou a partir do site oficial da Telerik.

> 💡 **Nota sobre o Fiddler Classic.** A Telerik declara que o Fiddler Classic está sem desenvolvimento ativo, recomendando o Fiddler Everywhere para uso profissional. Para o propósito didático deste laboratório o Fiddler Classic continua adequado.

### Material

- Roteiro prático: [`fluxo-a-administrador/roteiro.md`](./fluxo-a-administrador/roteiro.md).
- Template do relatório: [`fluxo-a-administrador/relatorio.docx`](./fluxo-a-administrador/relatorio.docx).
- Ferramenta de captura de tela do sistema.

---

## 3. Roteiro e entrega

A atividade possui **um único roteiro**, executado com privilégio de administrador, porque a inspeção de HTTPS exige a instalação de um certificado raiz temporário no armazenamento de confiança do sistema operacional.

1. Abra o [`roteiro.md`](./fluxo-a-administrador/roteiro.md).
2. Baixe o arquivo [`relatorio.docx`](./fluxo-a-administrador/relatorio.docx).
3. Preencha o relatório localmente, no Word, LibreOffice ou editor compatível.
4. Use sempre **Inspectors → Request → Raw** e **Inspectors → Response → Raw** no Fiddler para localizar as informações solicitadas.
5. Exporte o relatório preenchido como PDF e entregue no Microsoft Teams.

> Não é necessário fazer fork do repositório nem editar arquivos no GitHub.

---

## 4. Fundamentação Teórica

### 4.1. O protocolo HTTP

O **HyperText Transfer Protocol** (HTTP) é um protocolo de camada de aplicação, baseado em texto até HTTP/1.1, sem estado, do tipo request/response. Opera por padrão sobre TCP:

| Versão | Transporte | Porta padrão | Característica-chave |
|---|---|---|---|
| HTTP/1.0 | TCP | 80 | Uma conexão por request |
| HTTP/1.1 | TCP | 80 | *Keep-alive*, *pipelining*, chunked encoding |
| HTTP/2 | TCP + TLS (ALPN `h2`) | 443 | Binário, multiplexação, *header compression* (HPACK) |
| HTTP/3 | QUIC (UDP + TLS 1.3) | 443 | Sem *head-of-line blocking*, estabelecimento mais rápido |

**HTTPS** é HTTP encapsulado em TLS. Toda a mensagem HTTP — linha de status, headers e body — é cifrada. Sem decriptação, IP, porta, DNS e, em cenários típicos, SNI podem ficar visíveis, mas a mensagem HTTP em si não fica legível.

### 4.2. Anatomia de uma mensagem HTTP

Toda mensagem HTTP/1.x tem a mesma estrutura:

```text
<linha inicial>            ← request-line OU status-line
<Header-1>: <valor>        ← zero ou mais linhas de cabeçalho
<Header-2>: <valor>
...
<linha em branco>          ← CRLF separador
<corpo opcional>           ← body, quando presente
```

**Exemplo de request:**

```http
GET /get?id=42 HTTP/1.1
Host: http.aulasrede.com.br
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
Accept: text/html,application/xhtml+xml,application/xml;q=0.9
Accept-Encoding: gzip, deflate, br
Accept-Language: pt-BR,pt;q=0.9,en;q=0.8
Connection: keep-alive
```

**Exemplo de response:**

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 287
Server: go-httpbin
Access-Control-Allow-Origin: *

{
  "args": {"id": ["42"]},
  "headers": { ... },
  "origin": "200.100.50.30",
  "url": "https://http.aulasrede.com.br/get?id=42"
}
```

> 💡 O serviço usado no laboratório retorna valores de query string e cabeçalhos como arrays porque a mesma chave pode aparecer múltiplas vezes em HTTP.

### 4.3. Métodos (verbos) HTTP

| Método | Função | Possui body? | Idempotente? | Seguro? |
|---|---|---|---|---|
| `GET` | Recuperar representação de um recurso | Não | Sim | Sim |
| `HEAD` | Como GET, mas só retorna cabeçalhos | Não | Sim | Sim |
| `POST` | Submeter dados (formulários, APIs) | Sim | Não | Não |
| `PUT` | Criar/substituir recurso em URI conhecida | Sim | Sim | Não |
| `PATCH` | Atualização parcial | Sim | Não | Não |
| `DELETE` | Remover recurso | Opcional | Sim | Não |
| `OPTIONS` | Descobrir métodos/CORS suportados | Não | Sim | Sim |

> **Seguro** (*safe*): não altera estado no servidor. **Idempotente**: múltiplas execuções produzem o mesmo efeito que uma única.

### 4.4. Códigos de status

| Classe | Faixa | Significado | Exemplos frequentes |
|---|---|---|---|
| 1xx | 100–199 | Informacional | `100 Continue`, `101 Switching Protocols` |
| 2xx | 200–299 | Sucesso | `200 OK`, `201 Created`, `204 No Content` |
| 3xx | 300–399 | Redirecionamento | `301 Moved Permanently`, `302 Found`, `304 Not Modified` |
| 4xx | 400–499 | Erro do cliente | `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `429 Too Many Requests` |
| 5xx | 500–599 | Erro do servidor | `500 Internal Server Error`, `502 Bad Gateway`, `503 Service Unavailable`, `504 Gateway Timeout` |

### 4.5. Principais cabeçalhos

**De request:**

| Cabeçalho | Propósito |
|---|---|
| `Host` | Nome de domínio do servidor, obrigatório em HTTP/1.1 |
| `User-Agent` | Identificação do cliente/navegador |
| `Accept` | Tipos MIME aceitos na resposta |
| `Accept-Encoding` | Algoritmos de compressão suportados |
| `Accept-Language` | Idiomas preferidos |
| `Cookie` | Cookies previamente recebidos |
| `Authorization` | Credenciais |
| `Referer` | URL de origem da requisição |
| `Content-Type` | MIME do corpo enviado |
| `Content-Length` | Tamanho do corpo em bytes |
| `If-None-Match` | Validação de cache por ETag |

**De response:**

| Cabeçalho | Propósito |
|---|---|
| `Server` | Identificação do software servidor |
| `Content-Type` | MIME do corpo retornado |
| `Content-Length` | Tamanho do corpo em bytes |
| `Content-Encoding` | Compressão aplicada ao corpo |
| `Set-Cookie` | Cookie a ser armazenado pelo cliente |
| `Cache-Control` | Política de cache |
| `ETag` | Identificador de versão do recurso |
| `Location` | URL de redirecionamento |
| `WWW-Authenticate` | Esquema de autenticação exigido |
| `Strict-Transport-Security` | Força uso de HTTPS em acessos futuros |

### 4.6. O papel do *debugging proxy*

Um *debugging proxy* é um **man-in-the-middle controlado** usado pelo próprio usuário:

```text
[Navegador] ──► [Proxy local] ──► [Internet] ──► [Servidor]
                    │
                    ▼
             [Interface de inspeção]
```

Para tráfego HTTPS, o proxy:

1. Gera um certificado raiz próprio, instalado pelo usuário no armazenamento de confiança.
2. Emite certificados temporários assinados por essa raiz para cada site visitado.
3. Decifra a comunicação no meio e reencripta antes de enviar.

> ⚠️ **Atenção ética.** O certificado raiz do Fiddler deve permanecer apenas na máquina do estudante e ser removido ao final do laboratório.

---

## 5. Referências

- **RFC 9110** — HTTP Semantics. <https://datatracker.ietf.org/doc/html/rfc9110>
- **RFC 9111** — HTTP Caching. <https://datatracker.ietf.org/doc/html/rfc9111>
- **RFC 9112** — HTTP/1.1. <https://datatracker.ietf.org/doc/html/rfc9112>
- **RFC 9113** — HTTP/2. <https://datatracker.ietf.org/doc/html/rfc9113>
- **RFC 6265** — HTTP State Management Mechanism (Cookies). <https://datatracker.ietf.org/doc/html/rfc6265>
- **RFC 6797** — HTTP Strict Transport Security (HSTS). <https://datatracker.ietf.org/doc/html/rfc6797>
- MDN Web Docs — HTTP. <https://developer.mozilla.org/en-US/docs/Web/HTTP>
- Fiddler Classic — Documentação oficial. <https://docs.telerik.com/fiddler/>
- go-httpbin. <https://github.com/mccutchen/go-httpbin>
- KUROSE, J.; ROSS, K. *Redes de Computadores e a Internet*, 8ª ed., Cap. 2.
- TANENBAUM, A. S.; FEAMSTER, N.; WETHERALL, D. *Redes de Computadores*, 6ª ed., Cap. 7.

---

## Anexo A — Tabela-resumo de cabeçalhos

| Cabeçalho | Direção | Exemplo de valor |
|---|---|---|
| `Host` | Req | `http.aulasrede.com.br` |
| `User-Agent` | Req | `Mozilla/5.0 (...)` |
| `Accept` | Req | `text/html, */*; q=0.1` |
| `Accept-Encoding` | Req | `gzip, deflate, br` |
| `Accept-Language` | Req | `pt-BR, pt; q=0.9, en; q=0.5` |
| `Cookie` | Req | `sess=abc123; lang=pt` |
| `Authorization` | Req | `Bearer eyJhbGci...` |
| `Referer` | Req | `https://origem.com/pagina` |
| `Content-Type` | Ambos | `application/json; charset=utf-8` |
| `Content-Length` | Ambos | `1342` |
| `If-None-Match` | Req | `"686897696a7c876b7e"` |
| `Server` | Resp | `go-httpbin` |
| `Set-Cookie` | Resp | `sess=xyz; Path=/; Secure; HttpOnly` |
| `Cache-Control` | Resp | `max-age=3600, public` |
| `ETag` | Resp | `"686897696a7c876b7e"` |
| `Location` | Resp | `https://destino.com/novo` |
| `Strict-Transport-Security` | Resp | `max-age=31536000; includeSubDomains` |
| `Content-Encoding` | Resp | `gzip` |
| `WWW-Authenticate` | Resp | `Basic realm="Admin"` |

---

## Anexo B — Tabela-resumo de status codes

| Código | Frase de razão | Quando ocorre |
|---|---|---|
| 100 | Continue | Resposta intermediária a `Expect: 100-continue` |
| 200 | OK | Requisição bem-sucedida com corpo |
| 201 | Created | Recurso criado |
| 204 | No Content | Sucesso sem corpo de resposta |
| 301 | Moved Permanently | Recurso migrou definitivamente |
| 302 | Found | Redirecionamento temporário |
| 304 | Not Modified | Cache do cliente ainda válido |
| 400 | Bad Request | Sintaxe inválida no request |
| 401 | Unauthorized | Autenticação ausente ou inválida |
| 403 | Forbidden | Autenticado mas sem permissão |
| 404 | Not Found | Recurso inexistente |
| 405 | Method Not Allowed | Método não suportado no recurso |
| 408 | Request Timeout | Cliente demorou a enviar |
| 418 | I'm a teapot | RFC 2324 |
| 429 | Too Many Requests | Rate limiting |
| 500 | Internal Server Error | Erro genérico do servidor |
| 502 | Bad Gateway | Gateway/proxy recebeu resposta inválida |
| 503 | Service Unavailable | Sobrecarga ou manutenção |
| 504 | Gateway Timeout | Gateway não obteve resposta do upstream |

---

**Versão:** 4.0 — Mai/2026  
**Licença:** CC BY-NC-SA 4.0
