# Aula 10 — JavaScript: DOM e Eventos

**Módulo:** Fundamentos de Arquitetura Web, Protocolo HTTP, Conceitos de API REST, HTML, CSS e JavaScript
**Carga horária da aula:** 4 horas
**Professor(a):** @karizeviecelli

---

## 🎯 Objetivos da Aula

Ao final desta aula, você será capaz de:

- Selecionar elementos HTML com `querySelector` e `getElementById`.
- Manipular elementos existentes com `innerHTML`, `classList` e `style`.
- Escutar e reagir a eventos com `addEventListener` (`click`, `submit`, `input`).
- Validar dados de um formulário com JavaScript.
- Construir uma interface interativa completa com validação de formulário.

---

## 🖼️ Analogia Inicial: o cérebro ganha mãos e sentidos

Na aula passada, demos um cérebro à casa — ele já sabia guardar informações, tomar decisões e repetir tarefas. Mas um cérebro sozinho, sem mãos e sem sentidos, não consegue interagir com o mundo real.

Hoje, esse cérebro ganha:

- **Mãos** (`querySelector`, `getElementById`): a capacidade de "apontar" para um cômodo específico da casa e agarrá-lo — "pegue a luz da sala", "pegue o formulário de cadastro".
- **Capacidade de reorganizar** (`innerHTML`, `classList`, `style`): depois de pegar o elemento, o cérebro pode mudar o texto de uma placa, trocar a cor de uma parede, ou ligar/desligar uma luz.
- **Sentidos** (`addEventListener`): agora a casa "sente" quando alguém aperta um interruptor (`click`), preenche um formulário (`input`) ou aperta "enviar" (`submit`) — e reage automaticamente.

É essa combinação — apontar, alterar e reagir — que transforma uma página estática numa interface de verdade, que responde ao que o usuário faz.

---

## 📚 Conteúdo Teórico

### 1. Selecionando elementos: `querySelector` e `getElementById`

```javascript
// getElementById: busca por id (mais antigo, ainda muito usado)
const titulo = document.getElementById("titulo-pagina");

// querySelector: busca usando seletores CSS — muito mais flexível
const botao = document.querySelector(".botao-enviar"); // primeira ocorrência da classe
const primeiroInput = document.querySelector("input");  // primeira tag <input>
const campoEmail = document.querySelector("#email");    // igual ao getElementById, mas com #

// querySelectorAll: retorna TODOS os elementos que combinam com o seletor
const todosOsCards = document.querySelectorAll(".card"); // uma "lista" de elementos
```

`querySelector` usa exatamente a mesma sintaxe de seletores do CSS que já conhecemos — ponto para classe, `#` para id, espaço para descendente. Isso o torna a ferramenta mais versátil e recomendada no dia a dia.

### 2. Manipulando elementos: `innerHTML`, `classList`, `style`

```javascript
const titulo = document.querySelector("#titulo-pagina");

// innerHTML: lê ou substitui o conteúdo HTML de dentro do elemento
titulo.innerHTML = "Bem-vindo(a) de volta!";

// classList: adiciona, remove ou alterna classes CSS
const card = document.querySelector(".card");
card.classList.add("destaque");      // adiciona a classe "destaque"
card.classList.remove("oculto");     // remove a classe "oculto"
card.classList.toggle("ativo");      // adiciona se não tiver, remove se tiver

// style: altera uma propriedade CSS diretamente, em JavaScript (camelCase)
card.style.backgroundColor = "#eff6ff";
card.style.display = "none"; // esconde o elemento
```

Prefira `classList` a `style` sempre que possível: alternar uma classe CSS já preparada no arquivo `.css` mantém o JavaScript mais limpo (só decide QUANDO aplicar, o CSS decide COMO fica visualmente).

### 3. Eventos: `addEventListener`

```javascript
const botao = document.querySelector("#botao-salvar");

// addEventListener "escuta" um tipo de evento e executa uma função quando ele acontece
botao.addEventListener("click", function () {
  console.log("Botão foi clicado!");
});

// Eventos comuns:
// "click"  — clique do mouse
// "submit" — envio de um formulário
// "input"  — digitação em um campo de texto (a cada tecla)
// "change" — quando o valor de um campo muda e perde o foco
```

O `addEventListener` recebe dois argumentos: o nome do evento (string) e uma função que roda quando o evento acontece — essa função é chamada de **callback**. Diferente do `onclick` direto no HTML (que vimos em alguns materiais anteriores só como demonstração), `addEventListener` é a forma profissional de conectar JavaScript e HTML, mantendo os dois em arquivos separados.

### 4. O objeto `event` e `preventDefault()`

```javascript
const formulario = document.querySelector("#form-cadastro");

formulario.addEventListener("submit", function (event) {
  event.preventDefault(); // impede o comportamento padrão (recarregar a página)

  console.log("Formulário capturado, sem recarregar a página!");
});
```

Por padrão, um formulário HTML tenta recarregar a página ao ser enviado (`submit`). `event.preventDefault()` cancela esse comportamento, permitindo que o JavaScript assuma o controle total — essencial para validar os dados antes de decidir o que fazer com eles.

### 5. Validação de formulário

```javascript
const formulario = document.querySelector("#form-cadastro");
const campoEmail = document.querySelector("#email");
const mensagemErro = document.querySelector("#erro-email");

formulario.addEventListener("submit", function (event) {
  event.preventDefault();

  const email = campoEmail.value; // .value lê o texto digitado no campo

  if (email === "") {
    mensagemErro.innerHTML = "O e-mail é obrigatório.";
    mensagemErro.classList.add("visivel");
    return; // interrompe a função aqui — não deixa "passar" o cadastro inválido
  }

  if (!email.includes("@")) {
    mensagemErro.innerHTML = "Digite um e-mail válido.";
    mensagemErro.classList.add("visivel");
    return;
  }

  mensagemErro.classList.remove("visivel");
  console.log("Cadastro válido, pode enviar!");
});
```

Esse é o padrão clássico de validação: capturar o `submit`, ler os valores dos campos com `.value`, testar cada regra com `if`, e mostrar mensagens de erro específicas usando exatamente as ferramentas desta aula: selecionar elemento, mudar `innerHTML`, alternar `classList`.

---

<a id="atividade"></a>
## 💻 Atividade Prática

**Duração sugerida:** 90 minutos
**Formato:** individual, com apoio em dupla
**⭐ Checkpoint 3 do projeto final**

### Parte 1 — Interface interativa com validação de formulário

Crie uma página `cadastro.html` com um formulário de cadastro (nome, e-mail e senha, por exemplo) e um arquivo `cadastro.js` conectado a ela, implementando:

1. Capture o evento `submit` do formulário e use `event.preventDefault()`.
2. Valide que o campo nome não está vazio.
3. Valide que o e-mail contém um `@`.
4. Valide que a senha tem pelo menos 6 caracteres.
5. Para cada erro, mostre uma mensagem específica usando `innerHTML`, e adicione uma classe CSS (`classList.add`) que deixe o campo com borda vermelha.
6. Se todos os campos forem válidos, esconda as mensagens de erro e mostre uma mensagem de sucesso (`"Cadastro realizado!"`).
7. Adicione um botão "Limpar" que, ao ser clicado (`addEventListener("click", ...)`), apaga o conteúdo de todos os campos.

### Parte 2 — Perguntas de fixação

Responda com suas palavras:

1. Qual a diferença entre `querySelector` e `querySelectorAll`?
2. Para que serve `event.preventDefault()` no evento `submit` de um formulário?
3. Por que `classList.toggle()` é útil para elementos que "ligam e desligam" (como um menu mobile)?
4. Qual a diferença entre o evento `input` e o evento `change` em um campo de texto?

Quando terminar, confira sua resposta no gabarito.

[Ver Gabarito »](#gabarito)

---

<a id="gabarito"></a>
## ✅ Gabarito

### Parte 1 — Interface interativa com validação de formulário

```html
<form id="form-cadastro">
  <input type="text" id="nome" placeholder="Nome">
  <span class="erro" id="erro-nome"></span>

  <input type="email" id="email" placeholder="E-mail">
  <span class="erro" id="erro-email"></span>

  <input type="password" id="senha" placeholder="Senha">
  <span class="erro" id="erro-senha"></span>

  <button type="submit">Cadastrar</button>
  <button type="button" id="btn-limpar">Limpar</button>
  <p id="sucesso" class="oculto">Cadastro realizado!</p>
</form>
```

```javascript
const formulario = document.querySelector("#form-cadastro");
const campoNome = document.querySelector("#nome");
const campoEmail = document.querySelector("#email");
const campoSenha = document.querySelector("#senha");

formulario.addEventListener("submit", function (event) {
  event.preventDefault();   // 1)
  let valido = true;

  if (campoNome.value.trim() === "") {                 // 2)
    document.querySelector("#erro-nome").innerHTML = "Nome é obrigatório.";
    campoNome.classList.add("campo-invalido");
    valido = false;
  }

  if (!campoEmail.value.includes("@")) {                 // 3)
    document.querySelector("#erro-email").innerHTML = "E-mail inválido.";
    campoEmail.classList.add("campo-invalido");
    valido = false;
  }

  if (campoSenha.value.length < 6) {                       // 4)
    document.querySelector("#erro-senha").innerHTML = "Senha deve ter 6+ caracteres.";
    campoSenha.classList.add("campo-invalido");
    valido = false;
  }

  if (valido) {                                              // 6)
    document.querySelector("#sucesso").classList.remove("oculto");
  }
});

document.querySelector("#btn-limpar").addEventListener("click", function () { // 7)
  campoNome.value = "";
  campoEmail.value = "";
  campoSenha.value = "";
});
```

### Parte 2 — Perguntas de fixação

1. `querySelector` retorna apenas o PRIMEIRO elemento que combina com o seletor, ou `null` se não encontrar nenhum. `querySelectorAll` retorna TODOS os elementos que combinam, em uma lista (NodeList) — mesmo que só exista um elemento, ele vem dentro de uma lista.
2. Por padrão, ao enviar um formulário (evento `submit`), o navegador recarrega a página inteira, o que interromperia qualquer script JavaScript em execução e faria a página "piscar". `event.preventDefault()` cancela esse comportamento padrão, permitindo que o JavaScript controle o que acontece a seguir (como validar os dados antes de decidir enviar ou não).
3. `classList.toggle()` alterna automaticamente entre adicionar e remover a classe — se ela não existe, adiciona; se já existe, remove. Isso é perfeito para elementos "liga/desliga" como um menu mobile, porque evita escrever manualmente um `if` verificando se a classe já está presente antes de decidir se adiciona ou remove.
4. O evento `input` dispara a CADA tecla digitada no campo — ideal para validação em tempo real, enquanto o usuário digita. O evento `change` dispara só quando o valor muda E o campo perde o foco (o usuário clica fora ou pressiona Tab) — mais adequado para ações que não precisam ser instantâneas, como salvar uma preferência.

[« Voltar para a Atividade](#atividade)
