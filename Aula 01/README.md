# Aula 01 — Arquitetura Web e Protocolo HTTP

**Módulo:** Fundamentos de Arquitetura Web, Protocolo HTTP, Conceitos de API REST, HTML, CSS e JavaScript
**Carga horária da aula:** 4 horas
**Professor(a):** @karizeviecelli

---

## 🎯 Objetivos da Aula

Ao final desta aula, você será capaz de:

- Explicar o que acontece, passo a passo, quando você acessa um site.
- Diferenciar cliente, servidor, DNS e hospedagem.
- Entender o que é o protocolo HTTP e como funciona o ciclo requisição-resposta.
- Reconhecer os principais métodos HTTP (`GET`, `POST`, `PUT`, `DELETE`, `PATCH`).
- Interpretar códigos de status HTTP (2xx, 3xx, 4xx, 5xx).
- Usar o DevTools do navegador para observar requisições reais.

---

## 🍽️ Analogia Inicial: a Web é um restaurante

Imagine que você entra em um restaurante:

- **Você (o cliente)** senta à mesa e pede um prato ao garçom.
- **O garçom** leva seu pedido até a cozinha.
- **A cozinha (o servidor)** prepara o prato e entrega ao garçom.
- **O garçom** traz o prato de volta até você.

Na Web funciona quase igual:

- **Você (cliente/navegador)** faz um pedido — uma requisição — para um endereço (URL).
- **A internet** carrega esse pedido até o destino certo (com a ajuda do DNS, que funciona como a lista telefônica que descobre "onde fica" o restaurante).
- **O servidor** recebe o pedido, "prepara" a resposta (uma página, uma imagem, um dado) e devolve.
- **Você** recebe o prato — a resposta — e o navegador exibe na tela.

Esse "pedido e entrega" segue regras bem definidas. Essas regras são o **protocolo HTTP**.

---

## 📚 Conteúdo Teórico

### 1. Como a Web funciona: cliente-servidor

Toda aplicação web tem, no mínimo, dois lados:

- **Cliente:** o navegador (Chrome, Firefox etc.) que você usa para acessar sites. É quem *pede*.
- **Servidor:** um computador remoto, ligado 24h, que guarda os arquivos do site e *responde* aos pedidos.

```text
[ Cliente (navegador) ]  --- pedido (requisição) --->  [ Servidor ]
[ Cliente (navegador) ]  <--- resposta (dados) -------  [ Servidor ]
```

Uma aplicação web completa costuma ter três camadas:

- **Front-end:** o que o usuário vê e interage (HTML, CSS, JavaScript). É o "salão do restaurante".
- **Back-end:** a lógica que processa os pedidos (regras de negócio, autenticação). É a "cozinha".
- **Banco de dados:** onde as informações ficam armazenadas (cardápio, estoque). É a "despensa".

### 2. DNS, URLs, domínios e hospedagem

Ninguém decora o "endereço real" (IP) de um site, como `142.250.219.14`. Por isso existe o **DNS (Domain Name System)**: ele funciona como uma lista telefônica gigante, que traduz nomes fáceis de lembrar (`google.com`) para o endereço IP do servidor.

```text
Você digita:      www.exemplo.com
DNS traduz para:  200.150.10.5  (endereço IP do servidor)
Navegador acessa: 200.150.10.5
```

- **Domínio:** o "nome" do site (`exemplo.com`).
- **Hospedagem (hosting):** o "terreno" onde o servidor do site realmente mora — uma empresa que mantém o computador ligado e conectado à internet o tempo todo.

### 3. O Protocolo HTTP

**HTTP (HyperText Transfer Protocol)** é o conjunto de regras que cliente e servidor usam para "se entenderem" — como um idioma combinado entre o garçom e a cozinha.

Toda comunicação HTTP segue o ciclo:

```text
1. Cliente monta uma REQUISIÇÃO (request)
2. Requisição viaja até o servidor
3. Servidor processa e monta uma RESPOSTA (response)
4. Resposta volta até o cliente
```

**Exemplo comentado de uma requisição HTTP** (o que o navegador envia, de forma simplificada):

```http
GET /index.html HTTP/1.1        # Método GET, pedindo o arquivo index.html
Host: www.exemplo.com           # Para qual servidor/domínio é o pedido
User-Agent: Mozilla/5.0         # Identifica o navegador que está pedindo
Accept: text/html               # Diz que espera receber um HTML de volta
```

**Exemplo comentado de uma resposta HTTP:**

```http
HTTP/1.1 200 OK                          # Status: deu tudo certo
Content-Type: text/html; charset=UTF-8   # Tipo do conteúdo devolvido
Content-Length: 1256                     # Tamanho da resposta em bytes

<html>...</html>                         # O "corpo" da resposta: o conteúdo pedido
```

### 4. Métodos HTTP

Cada método representa uma **intenção** do cliente — como diferentes tipos de pedido no restaurante:

| Método | Analogia | Uso típico |
|---|---|---|
| `GET` | "Quero ver o cardápio" | Buscar/ler dados |
| `POST` | "Quero fazer um pedido novo" | Criar um novo dado |
| `PUT` | "Quero trocar meu pedido inteiro" | Atualizar um dado por completo |
| `PATCH` | "Quero só trocar a bebida" | Atualizar parte de um dado |
| `DELETE` | "Quero cancelar meu pedido" | Remover um dado |

```javascript
// Exemplo ilustrativo (não é código para rodar ainda — só para visualizar a ideia)

// GET -> "me dá os dados"
fetch("https://api.exemplo.com/produtos");

// POST -> "aqui está um novo dado para você guardar"
fetch("https://api.exemplo.com/produtos", {
  method: "POST",          // método HTTP usado
  body: JSON.stringify({   // dado enviado, convertido para texto JSON
    nome: "Caneta"
  })
});
```

### 5. Códigos de Status HTTP

Depois que a cozinha (servidor) processa o pedido, ela avisa como foi. No HTTP, isso é feito com **códigos de status**, sempre de 3 dígitos:

| Faixa | Significado | Analogia |
|---|---|---|
| **2xx** | Sucesso | "Seu prato está pronto e correto" |
| **3xx** | Redirecionamento | "Esse prato agora é servido em outra mesa" |
| **4xx** | Erro do cliente | "Você pediu algo que não existe no cardápio" |
| **5xx** | Erro do servidor | "A cozinha pegou fogo" |

Alguns dos mais comuns:

- `200 OK` — deu tudo certo.
- `201 Created` — um novo recurso foi criado com sucesso.
- `301 Moved Permanently` — o conteúdo mudou de endereço definitivamente.
- `400 Bad Request` — o pedido foi feito de forma errada.
- `401 Unauthorized` — você precisa se identificar para acessar.
- `404 Not Found` — o que você pediu não existe.
- `500 Internal Server Error` — o servidor teve um problema interno.

### 6. Observando requisições reais com o DevTools

Todo navegador moderno tem ferramentas para desenvolvedores (**DevTools**), que mostram, em tempo real, todas as requisições HTTP feitas por uma página.

**Como abrir:**

- Chrome/Edge: `F12` ou `Ctrl + Shift + I` (Windows) / `Cmd + Option + I` (Mac)
- Vá até a aba **Network** (Rede)
- Recarregue a página (`F5`) para ver as requisições acontecendo

Lá você consegue ver, para cada requisição: o método usado, o status retornado, o tempo de resposta e o conteúdo recebido.

---

<a id="atividade"></a>
## 💻 Atividade Prática

**Duração sugerida:** 60 minutos
**Formato:** em duplas

### Parte 1 — Investigando com o DevTools

1. Abra o navegador e acesse um site de sua escolha (ex.: `wikipedia.org`).
2. Abra o DevTools (`F12`) e vá até a aba **Network**.
3. Recarregue a página e observe a lista de requisições.
4. Escolha **5 requisições** diferentes e anote, para cada uma:
   - A URL solicitada
   - O método HTTP (`GET`, `POST`, etc.)
   - O código de status retornado
   - O tipo de conteúdo (`Content-Type`), como `text/html`, `image/png`, `application/json`

### Parte 2 — Relacionando conceitos

Responda, com suas palavras:

1. Se o cliente é o navegador, o que representa o servidor na analogia do restaurante?
2. Por que precisamos do DNS? O que aconteceria se ele não existisse?
3. Dê um exemplo do dia a dia (fora da internet) para cada um destes status: `200`, `404` e `500`.
4. Entre os métodos HTTP estudados, qual você usaria para **excluir sua conta** em um site? E qual usaria para **criar uma conta nova**?

### Parte 3 — Desafio em dupla

Monte, no caderno ou em um documento, o "ciclo completo" de uma ação simples do dia a dia (por exemplo: "curtir uma postagem em uma rede social"), indicando:

- Quem é o cliente
- Qual seria o método HTTP provável
- Qual status de sucesso o servidor devolveria
- Qual status seria devolvido se der erro

Quando terminar, confira sua resposta no gabarito.

[Ver Gabarito »](#gabarito)

---

<a id="gabarito"></a>
## ✅ Gabarito

### Parte 1 — Investigando com o DevTools

Não existe uma resposta única (depende do site escolhido), mas um exemplo esperado, acessando uma página inicial simples:

| URL | Método | Status | Content-Type |
|---|---|---|---|
| `/` (página principal) | GET | 200 | text/html |
| `/estilos.css` | GET | 200 | text/css |
| `/logo.png` | GET | 200 | image/png |
| `/script.js` | GET | 200 | application/javascript |
| `/api/dados` | GET | 200 ou 304 | application/json |

O importante é o aluno perceber que **uma única página carrega dezenas de requisições** — HTML, CSS, JS, imagens e, às vezes, dados de API — cada uma com seu próprio método e status.

### Parte 2 — Relacionando conceitos

1. O servidor representa a **cozinha**: recebe o pedido do garçom (a requisição), prepara e devolve a resposta.
2. O DNS funciona como uma **lista telefônica/agenda de contatos** da internet. Sem ele, precisaríamos decorar endereços IP numéricos (como `142.250.219.14`) em vez de nomes como `google.com`.
3. Exemplos possíveis:
   - `200`: pedir uma pizza e ela chegar certinha.
   - `404`: ir a uma loja física e o produto anunciado não existir na prateleira.
   - `500`: ligar para uma empresa e a central telefônica estar com defeito, impedindo qualquer atendimento.
4. Para **excluir a conta**, o método correto é `DELETE`. Para **criar uma conta nova**, o método correto é `POST`.

### Parte 3 — Desafio em dupla (exemplo de resposta)

**Ação:** curtir uma postagem.

```javascript
// Exemplo comentado de como essa ação poderia ser representada

// Cliente: o navegador do usuário, ao clicar no botão "curtir"
// Requisição enviada ao servidor:
fetch("https://api.rede-social.com/posts/123/curtir", {
  method: "POST" // POST porque estamos CRIANDO uma nova curtida
});

// Resposta de sucesso esperada do servidor:
// HTTP/1.1 201 Created  -> a curtida foi registrada com sucesso

// Resposta de erro possível:
// HTTP/1.1 404 Not Found -> a postagem "123" não existe
```

- **Cliente:** navegador/app do usuário
- **Método provável:** `POST` (está criando um novo registro de curtida)
- **Status de sucesso:** `201 Created` (ou `200 OK`, dependendo da API)
- **Status de erro:** `404 Not Found` (postagem inexistente) ou `401 Unauthorized` (usuário não logado)

[« Voltar para a Atividade](#atividade)
