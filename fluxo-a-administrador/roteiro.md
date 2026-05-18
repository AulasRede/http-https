# Roteiro Prático — Inspeção de HTTP/HTTPS com Fiddler Classic

> **Pré-requisito:** este roteiro deve ser executado em uma máquina na qual você possa abrir o Fiddler Classic como administrador e instalar/remover o certificado raiz temporário do Fiddler.
>
> **Escopo:** captura e inspeção de tráfego **HTTP e HTTPS**, incluindo decriptação TLS pelo Fiddler Classic.
>
> **Fundamentação teórica:** consulte o [`readme.md`](../readme.md) na raiz do repositório.
>
> **Tempo total previsto em sala:** ~85 minutos, além da entrega.

---

## Sumário

- [1. Setup do ambiente](#1-setup-do-ambiente)
- [2. Padrão de inspeção no Fiddler](#2-padrão-de-inspeção-no-fiddler)
- [3. Atividades práticas](#3-atividades-práticas)
- [4. Entrega](#4-entrega)
- [5. Encerramento pós-aula](#5-encerramento-pós-aula)

---

## 1. Setup do ambiente

### 1.1. Fiddler Classic

- **No laboratório:** use a instalação e as orientações fornecidas pelo professor. Não reinstale nem altere componentes do sistema sem instrução.
- **Em máquina pessoal:** instale o Fiddler Classic a partir do site oficial da Telerik (<https://www.telerik.com/fiddler/fiddler-classic>) ou, no Windows, pelo PowerShell:

```powershell
winget install Telerik.Fiddler.Classic
```

### 1.2. Ativar decriptação HTTPS

1. Abra o Fiddler Classic **como administrador**.
2. Acesse **Tools → Options → HTTPS**.
3. Marque **Capture HTTPS CONNECTs** e **Decrypt HTTPS traffic**.
4. Aceite a instalação do certificado raiz `DO_NOT_TRUST_FiddlerRoot`.
5. Se usar Firefox, importe o certificado em `about:preferences#privacy → Ver Certificados → Autoridades → Importar`.

> ⚠️ O certificado raiz do Fiddler permite decriptar conexões HTTPS desta máquina enquanto estiver instalado. Ele deve ser removido no encerramento da atividade.

### 1.3. Teste do ambiente

Com o Fiddler capturando, acesse `https://http.aulasrede.com.br/get`. A sessão deve aparecer decifrada. Para validar, selecione a sessão e veja o conteúdo em:

- **Inspectors → Request → Raw**
- **Inspectors → Response → Raw**

Se o request/response não aparecerem legíveis, confira se o Fiddler está capturando (**Capturing** no canto inferior esquerdo; F12 alterna) e se **Decrypt HTTPS traffic** está habilitado.

---

## 2. Padrão de inspeção no Fiddler

Durante todo o laboratório, use sempre o mesmo caminho no Fiddler:

1. Selecione a sessão desejada no painel esquerdo.
2. No painel direito, abra a aba **Inspectors**.
3. Para a mensagem enviada pelo navegador, use **Request → Raw**.
4. Para a mensagem recebida do servidor, use **Response → Raw**.

> 💡 Não use as abas `Headers`, `JSON`, `WebForms`, `Cookies` ou `TextView` para responder às atividades. Elas podem ajudar visualmente, mas todas as respostas devem ser retiradas do texto mostrado em **Raw**. Isso evita dúvidas sobre onde procurar cada informação.
>
> 💡 Limpe a lista entre atividades com **Edit → Remove → All Sessions** (`Ctrl+X`).

---

## 3. Atividades práticas

Baixe o arquivo [`relatorio.docx`](./relatorio.docx), preencha-o localmente no Word, LibreOffice ou editor compatível e insira as capturas nos espaços indicados. Não é necessário fazer fork do repositório.

### Atividade 1 — Primeira captura ⏱ ~8 min

1. Acesse `http://example.com`.
2. Selecione a sessão de `example.com`.
3. Abra **Inspectors → Request → Raw** e **Inspectors → Response → Raw**.

**Registrar no relatório:**

- Captura da sessão.
- *Request-line*.
- *Status-line*.
- Três cabeçalhos enviados pelo navegador e a função de cada um.
- `Content-Type` e `Content-Length` ou `Transfer-Encoding` da resposta.

---

### Atividade 2 — Anatomia de um GET ⏱ ~10 min

1. Acesse `https://http.aulasrede.com.br/get?aluno=SEU_NOME&curso=redes`, substituindo `SEU_NOME`.
2. Selecione a sessão correspondente.
3. Abra **Inspectors → Request → Raw** para encontrar a linha inicial e os cabeçalhos enviados.
4. Abra **Inspectors → Response → Raw** para encontrar o corpo JSON retornado pelo servidor.

**Registrar no relatório:**

- *Request-line* completa.
- Valores de `Host`, `User-Agent` e `Accept`.
- Campos `args`, `headers` e `origin` do corpo JSON exibido em **Response → Raw**.

**Pergunta:** o que o campo `origin` representa? O `User-Agent` no JSON coincide com o enviado no request?

---

### Atividade 3 — POST e envio de formulário ⏱ ~12 min

1. Acesse `https://http.aulasrede.com.br/forms/post`.
2. Preencha o formulário e envie.
3. Localize a sessão `POST` para `/post`.
4. Use **Inspectors → Request → Raw** para ver a linha inicial, os cabeçalhos e o corpo enviado.
5. Use **Inspectors → Response → Raw** para ver o corpo JSON retornado.

**Registrar no relatório:**

- *Request-line*.
- `Content-Type` e `Content-Length`.
- Corpo do request observado em **Request → Raw**.
- Campo `form` observado no JSON em **Response → Raw**.

**Pergunta:** qual formato codifica o corpo enviado? Por que **Raw** é a aba adequada para verificar literalmente o conteúdo transmitido?

---

### Atividade 4 — Status codes ⏱ ~10 min

Acesse e registre as sessões:

| # | URL | Classe esperada |
|---|---|---|
| 1 | `https://http.aulasrede.com.br/status/200` | 2xx |
| 2 | `https://http.aulasrede.com.br/redirect-to?status_code=301&url=/get` | 3xx |
| 3 | `https://http.aulasrede.com.br/status/404` | 4xx |
| 4 | `https://http.aulasrede.com.br/status/500` | 5xx |

Para cada sessão, use **Inspectors → Request → Raw** e **Inspectors → Response → Raw**.

**Registrar no relatório:** método, URL, *status-line*, tamanho do corpo quando visível e presença/ausência de body.

**Pergunta:** no `301`, qual cabeçalho informa o destino do redirecionamento?

---

### Atividade 5 — Cabeçalhos essenciais ⏱ ~10 min

1. Em janela anônima, acesse:
   `https://http.aulasrede.com.br/response-headers?Cache-Control=max-age%3D3600&Set-Cookie=teste%3D1&Strict-Transport-Security=max-age%3D31536000`
2. Acesse `https://http.aulasrede.com.br/gzip`.
3. Em cada sessão, use **Inspectors → Request → Raw** e **Inspectors → Response → Raw**.

**Registrar no relatório:** uma tabela consolidada com `Host`, `User-Agent`, `Accept`, `Content-Type`, `Content-Length` ou `Transfer-Encoding`, `Content-Encoding`, `Set-Cookie`, `Cache-Control` e `Strict-Transport-Security`.

**Pergunta:** qual é o papel de `Content-Encoding` e de `Strict-Transport-Security`?

---

### Atividade 6 — HTTP vs HTTPS ⏱ ~15 min

1. Desabilite temporariamente **Decrypt HTTPS traffic** em **Tools → Options → HTTPS**.
2. Acesse `http://http.aulasrede.com.br/get`. O site deve responder com redirecionamento `301` para HTTPS; registre essa sessão HTTP antes da sessão HTTPS seguinte.
3. Acesse `https://http.aulasrede.com.br/get`.
4. Compare as duas sessões usando **Inspectors → Request → Raw** e **Inspectors → Response → Raw** quando houver conteúdo legível.
5. Reabilite **Decrypt HTTPS traffic** e acesse novamente `https://http.aulasrede.com.br/get`.
6. Compare novamente usando **Request → Raw** e **Response → Raw**.

**Registrar no relatório:**

- Captura do HTTP puro/redirecionamento para HTTPS.
- Captura do HTTPS sem decriptação.
- Captura do HTTPS com decriptação.
- Tabela indicando o que fica visível em cada caso.

**Pergunta:** o que fica visível em HTTP, em HTTPS sem decriptação e em HTTPS com decriptação? Por que a decriptação exige instalar o certificado raiz?

---

### Atividade 7 — Cookies e sessão ⏱ ~10 min

1. Em janela anônima, acesse `https://http.aulasrede.com.br/cookies/set?disciplina=redes&professor=claudio`.
2. Na primeira resposta, procure `Set-Cookie` em **Inspectors → Response → Raw**.
3. Na requisição seguinte para `/cookies`, procure `Cookie` em **Inspectors → Request → Raw**.
4. Recarregue a página uma vez e observe novamente o request e o response em **Raw**.

**Registrar no relatório:** sequência das sessões, `Set-Cookie` recebido e `Cookie` enviado.

**Pergunta:** `Set-Cookie` aparece em toda requisição ou apenas quando o servidor define/atualiza cookies? Quais atributos do cookie foram observados?

---

### Atividade 8 — Manipulação simples com breakpoint ⏱ ~10 min *(Opcional)*

1. Clique na janela do Fiddler para garantir foco.
2. Ative **Rules → Automatic Breakpoints → Before Requests**.
3. Acesse `https://http.aulasrede.com.br/user-agent`.
4. Na sessão pausada, edite o cabeçalho `User-Agent` para:
   `User-Agent: LaboratorioRedes/1.0 (Aluno NOME)`
5. Clique em **Run to Completion**.
6. Desabilite breakpoints em **Rules → Automatic Breakpoints → Disabled**.
7. Use **Inspectors → Request → Raw** para confirmar o cabeçalho editado e **Inspectors → Response → Raw** para conferir o JSON de resposta.

**Registrar no relatório:** captura do request editado e campo `user-agent` do JSON de resposta.

**Pergunta:** o que este teste mostra sobre o papel ativo de um proxy?

---

## 4. Entrega

### O que entregar

Um único PDF chamado **`SOBRENOME_NOME_RA_LAB_HTTP_HTTPS.pdf`**, gerado a partir do arquivo `relatorio.docx` preenchido.

### Como preencher e gerar o PDF

1. Baixe [`relatorio.docx`](./relatorio.docx) para sua máquina.
2. Preencha o arquivo localmente.
3. Insira as capturas de tela nos espaços indicados.
4. Exporte/salve como PDF pelo Word, LibreOffice ou editor compatível.
5. Entregue o PDF no Microsoft Teams.

> Não é necessário fazer fork do repositório nem editar arquivos no GitHub.

---

## 5. Encerramento pós-aula

> ⚠️ **Obrigatório.**

1. Desabilite **Decrypt HTTPS traffic**.
2. Remova o certificado `DO_NOT_TRUST_FiddlerRoot`:
   - Windows: `certmgr.msc` → *Autoridades de Certificação Raiz Confiáveis → Certificados*.
   - Firefox, se usado: `about:preferences#privacy → Ver Certificados → Autoridades`.
3. Feche o Fiddler.
4. Anexe ao relatório duas capturas: uma antes e outra depois da remoção do certificado.

Essas capturas comprovam que o certificado existia e foi removido.
