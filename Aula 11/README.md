# Aula 11 — Conceitos de API REST

**Módulo:** Fundamentos de Arquitetura Web, Protocolo HTTP, Conceitos de API REST, HTML, CSS e JavaScript
**Carga horária da aula:** 4 horas
**Professor(a):** @karizeviecelli

---

## 🎯 Objetivos da Aula

Ao final desta aula, você será capaz de:

- Explicar o que é uma API REST e por que ela existe.
- Identificar recursos e endpoints em uma API.
- Relacionar os verbos HTTP (`GET`, `POST`, `PUT`, `DELETE`) às suas ações típicas.
- Ler e interpretar dados no formato JSON.
- Usar `JSON.parse` e `JSON.stringify` para converter entre texto e objetos JavaScript.
- Explicar o conceito de autenticação básica com API keys e tokens.
- Explorar uma API pública usando uma ferramenta de teste (Postman/Thunder Client).

---

## 🖼️ Analogia Inicial: o cérebro da casa liga para o restaurante

Voltamos à nossa primeira analogia (Aula 1): o restaurante e o garçom. Só que agora, quem faz o pedido não é você — é o **cérebro da casa** (o JavaScript), ligando para o restaurante e fazendo pedidos automaticamente, sem que ninguém precise sair de casa.

- **O restaurante** é a **API** — um sistema pronto para receber pedidos e devolver respostas, sem que você precise saber como a cozinha funciona por dentro.
- **Cada prato do cardápio** é um **recurso** — "usuários", "produtos", "pedidos" — algo que existe e pode ser consultado ou alterado.
- **O endereço de cada prato no cardápio** é o **endpoint** — a URL exata que você chama para pedir aquele prato específico (ex.: `/usuarios`, `/produtos/42`).
- **O jeito de fazer o pedido** são os **verbos HTTP**: `GET` é "eu quero ver o cardápio" ou "me mostre esse prato"; `POST` é "quero fazer um pedido novo"; `PUT` é "quero trocar meu pedido"; `DELETE` é "quero cancelar meu pedido".
- **A comanda escrita**, no formato que tanto a cozinha quanto o garçom entendem, é o **JSON** — um jeito padronizado de escrever dados que qualquer sistema, em qualquer linguagem, consegue ler.
- **O crachá do garçom ou o cartão de sócio** é a **autenticação** — a API key ou o token que prova que você tem permissão para fazer aquele pedido.

Com essa peça, o cérebro da casa finalmente consegue "sair de casa" — buscar dados reais, atualizados, de fora — em vez de só trabalhar com o que já tinha guardado.

---

## 📚 Conteúdo Teórico

### 1. O que é uma API REST

```
API = Application Programming Interface
      (Interface de Programação de Aplicações)

REST = um "estilo" de organizar APIs, baseado em:
  - Recursos identificados por URLs (endpoints)
  - Uso dos verbos HTTP para indicar a ação desejada
  - Respostas normalmente em formato JSON
```

Uma API é, na prática, um "menu de opções" que um sistema oferece para outros sistemas conversarem com ele — sem precisar expor como o sistema funciona por dentro. O termo REST descreve um padrão comum (mas não o único) de organizar esse menu de forma previsível.

### 2. Recursos e endpoints

```
Recurso: "usuários"

Endpoints comuns para esse recurso:
GET    /usuarios        → lista todos os usuários
GET    /usuarios/42     → detalhes do usuário com id 42
POST   /usuarios        → cria um novo usuário
PUT    /usuarios/42     → atualiza o usuário 42 por completo
DELETE /usuarios/42     → remove o usuário 42
```

Repare no padrão: o **mesmo endpoint base** (`/usuarios`) é reaproveitado, e é o **verbo HTTP** que muda a ação — não o endereço. Isso é o coração do estilo REST: URLs representam "coisas" (substantivos), e os verbos HTTP representam "ações" (verbos).

### 3. Verbos HTTP na prática de uma API

```
GET    — BUSCAR dados (não deveria alterar nada no servidor)
POST   — CRIAR um novo recurso
PUT    — ATUALIZAR um recurso existente por completo
DELETE — REMOVER um recurso

(PATCH também existe, para atualizações PARCIAIS — vimos na Aula 1)
```

Uma boa API REST segue essa convenção rigorosamente: um `GET` nunca deveria, por exemplo, apagar dados — isso quebraria a expectativa de quem está consumindo a API.

### 4. O formato JSON

```json
{
  "id": 42,
  "nome": "Maria Silva",
  "email": "maria@email.com",
  "ativo": true,
  "tags": ["cliente", "vip"],
  "endereco": {
    "cidade": "São Paulo",
    "cep": "01000-000"
  }
}
```

JSON (JavaScript Object Notation) é um formato de texto para representar dados estruturados — muito parecido com objetos JavaScript, mas com regras mais rígidas: chaves sempre entre aspas duplas, sem vírgula depois do último item, sem comentários. Ele virou o formato padrão de resposta da maioria das APIs REST porque é leve e fácil de ler tanto por humanos quanto por máquinas.

### 5. `JSON.parse` e `JSON.stringify`

```javascript
// A API sempre devolve JSON como TEXTO (string) — é preciso "converter" para objeto JS
const textoRecebido = '{"nome": "Maria", "idade": 28}';

const objeto = JSON.parse(textoRecebido); // texto → objeto JavaScript
console.log(objeto.nome); // "Maria"
console.log(objeto.idade); // 28

// O caminho inverso: quando você quer ENVIAR dados para uma API
const meuObjeto = { nome: "João", idade: 30 };
const textoParaEnviar = JSON.stringify(meuObjeto); // objeto → texto
console.log(textoParaEnviar); // '{"nome":"João","idade":30}'
```

Pense em `JSON.parse` como "abrir a comanda e entender o pedido" e `JSON.stringify` como "escrever a comanda para mandar para a cozinha" — os dois sentidos da mesma tradução, entre texto puro e objetos que o JavaScript consegue manipular.

### 6. Autenticação básica: API keys e tokens

```
API Key: uma "senha" fixa, geralmente enviada junto com a requisição,
         que identifica QUEM está fazendo o pedido.

Exemplo de uso comum (no header da requisição):
Authorization: Bearer minha-chave-secreta-aqui

Token: parecido com a API key, mas geralmente TEMPORÁRIO,
       gerado depois de um login, e que expira após um tempo.
```

É como o crachá do garçom: sem ele, o restaurante não sabe se você é um cliente cadastrado, um funcionário, ou alguém sem permissão nenhuma. Nunca se deve expor uma API key publicamente (por exemplo, direto no código de um site) — isso é como deixar o crachá caído no chão para qualquer um usar.

---

<a id="atividade"></a>
## 💻 Atividade Prática

**Duração sugerida:** 90 minutos
**Formato:** individual, com apoio em dupla

### Parte 1 — Explorando uma API pública

Usando o Postman ou o Thunder Client (extensão do VS Code):

1. Instale e abra o Postman (ou Thunder Client).
2. Faça uma requisição `GET` para uma API pública gratuita, por exemplo `https://jsonplaceholder.typicode.com/users` (não exige autenticação).
3. Observe a resposta: identifique o **status code**, os **headers** e o **corpo (body)** em JSON.
4. Faça uma segunda requisição `GET` para um recurso específico, por exemplo `https://jsonplaceholder.typicode.com/users/3`, e compare com a lista completa.
5. Copie um trecho do JSON de resposta e cole no Console do navegador dentro de `JSON.parse("...")`, confirmando que você consegue acessar as propriedades (ex.: `objeto.name`).

### Parte 2 — Perguntas de fixação

Responda com suas palavras:

1. Qual a diferença entre "recurso" e "endpoint" no contexto de uma API REST?
2. Por que o mesmo endpoint (ex.: `/usuarios`) pode representar ações diferentes dependendo do verbo HTTP usado?
3. O que `JSON.parse` faz, e por que ele é necessário ao trabalhar com respostas de uma API?
4. Por que uma API key nunca deve ficar exposta publicamente no código de um site?

Quando terminar, confira sua resposta no gabarito.

[Ver Gabarito »](#gabarito)

---

<a id="gabarito"></a>
## ✅ Gabarito

### Parte 1 — Explorando uma API pública

Exemplo de resultado esperado ao acessar `https://jsonplaceholder.typicode.com/users/3`:

```json
{
  "id": 3,
  "name": "Clementine Bauch",
  "username": "Samantha",
  "email": "Nathan@yesenia.net",
  "address": { "..." : "..." },
  "phone": "1-463-123-4447",
  "website": "ramiro.info",
  "company": { "..." : "..." }
}
```

```javascript
const texto = '{"id":3,"name":"Clementine Bauch","email":"Nathan@yesenia.net"}';
const usuario = JSON.parse(texto);
console.log(usuario.name); // "Clementine Bauch"
```

Status code esperado: `200 OK` — indicando que a requisição foi bem-sucedida (revisão da Aula 1).

### Parte 2 — Perguntas de fixação

1. O "recurso" é o conceito/entidade em si (ex.: "usuários", "produtos") — o "quê" da API. O "endpoint" é a URL concreta usada para acessar aquele recurso (ex.: `/usuarios`, `/usuarios/42`) — o "endereço" que você efetivamente chama para interagir com o recurso.
2. Porque no estilo REST, a URL representa "o que" está sendo manipulado (um substantivo, como `/usuarios`), e o verbo HTTP representa "a ação" sendo realizada sobre aquele recurso (um verbo, como GET, POST, PUT, DELETE). Separar essas duas responsabilidades deixa a API previsível: quem conhece o padrão consegue adivinhar o comportamento sem precisar ler documentação extra.
3. `JSON.parse` converte um texto no formato JSON (uma string) em um objeto JavaScript real, que pode ter suas propriedades acessadas com `.` (ponto). É necessário porque toda resposta de uma API chega como texto puro — mesmo que o conteúdo "pareça" um objeto, o JavaScript só consegue acessar `objeto.propriedade` depois que esse texto é convertido de fato em objeto.
4. Porque qualquer pessoa que visualizar o código-fonte de um site (o que é trivial de fazer em qualquer navegador) teria acesso à chave, podendo usá-la para fazer requisições em nome daquela conta — gerando custos, exceder limites de uso, ou até acessar/alterar dados que não deveriam ser acessíveis publicamente. Chaves sensíveis devem ficar guardadas no servidor (back-end), nunca no código que roda no navegador do usuário.

[« Voltar para a Atividade](#atividade)
