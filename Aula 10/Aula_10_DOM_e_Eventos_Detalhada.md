# Aula 10 — JavaScript: DOM e Eventos

**Módulo:** Fundamentos de Arquitetura Web, Protocolo HTTP, Conceitos de API REST, HTML, CSS e JavaScript  
**Carga horária:** 4 horas (240 minutos)  
**Professora:** @karizeviecelli  
**Público:** alunos iniciantes que já tiveram contato com HTML, CSS, variáveis, funções e condições em JavaScript.

## 1. Ponto de partida: como uma página responde ao usuário?

Uma página interativa é uma página que muda em resposta a uma ação: clicar, digitar, escolher uma opção ou enviar um formulário.

Até aqui, o HTML organizou o conteúdo e o CSS definiu sua aparência. Nesta aula, o JavaScript vai localizar partes da página, ler informações e modificar o que aparece na tela.

**Pense antes de começar:** quando você digita seu nome e uma saudação aparece na tela, o navegador precisa criar outra página ou pode modificar um elemento que já existe?

Ele pode modificar a página atual. Para isso, usamos o **DOM**, a representação do documento que o navegador disponibiliza para programação.

### A analogia da casa interativa

Imagine uma casa com placas, lâmpadas, interruptores e uma ficha na portaria:

| Na casa | Na página |
| --- | --- |
| Estrutura dos cômodos | HTML |
| Cores e decoração | CSS |
| Sistema que controla a casa | JavaScript |
| Representação dos cômodos e objetos que o sistema pode acessar | DOM |
| Localizar a placa da entrada | Selecionar um elemento |
| Trocar o texto da placa | Alterar `textContent` |
| Acionar um interruptor | Disparar um evento |
| Definir a reação ao interruptor | Registrar uma função com `addEventListener` |
| Conferir a ficha na portaria | Validar os campos de um formulário |

Nesta aula, repetiremos quatro ações: **selecionar → escutar → ler → alterar**.

### Objetivos de aprendizagem

Ao final, o aluno deverá conseguir:

- Explicar o DOM com suas palavras.
- Conectar um arquivo JavaScript ao HTML usando `defer`.
- Selecionar um ou vários elementos.
- Distinguir `textContent`, `innerHTML` e `value`.
- Alterar classes CSS e reconhecer o uso de `style`.
- Reagir a `click`, `input`, `change` e `submit`.
- Explicar o objeto do evento e `preventDefault()`.
- Validar um formulário e atualizar mensagens sem deixar erros antigos na tela.
- Testar cenários válidos, inválidos, de correção e de limpeza.

### Roteiro sugerido — 240 minutos

| Tempo | Etapa | Evidência de aprendizagem |
| --- | --- | --- |
| 0–15 min | Pergunta inicial e analogia | Aluno explica o que torna uma página interativa |
| 15–40 min | DOM, arquivos e `defer` | HTML e JavaScript conectados |
| 40–65 min | Seleção de elementos | Aluno seleciona por ID, classe e tag |
| 65–90 min | Conteúdo, classes e estilos | Saudação e painel de destaque funcionando |
| 90–120 min | Eventos e formulário mínimo | Aluno prevê quando cada função será executada |
| 120–130 min | Intervalo | — |
| 130–145 min | Planejamento das regras | Regras e casos de teste registrados |
| 145–210 min | Construção do formulário | Interface implementada e comentada |
| 210–235 min | Testes em dupla e correções | Casos da tabela de testes executados |
| 235–240 min | Fechamento | Aluno explica o fluxo completo |

A prática ocupa 90 minutos, somando construção e testes. As extensões ao final podem ficar para alunos que concluírem antes ou para uma próxima aula.

---

## 2. [BÁSICO] O que é o DOM?

**DOM** significa *Document Object Model*, ou Modelo de Objetos do Documento. É uma estrutura em árvore que representa o documento carregado no navegador.

Observe este trecho:

```html
<!-- O main agrupa o conteúdo principal da página. -->
<main>
  <!-- O título e o parágrafo são elementos filhos do main. -->
  <h1 id="titulo">Bem-vindo!</h1>
  <p class="descricao">Vamos aprender JavaScript.</p>
</main>
```

O `main` contém o `h1` e o `p`. Esses dois elementos são irmãos, pois compartilham o mesmo elemento pai. O navegador também representa textos e comentários como nós da árvore; nesta aula, nosso foco estará nos elementos.

O JavaScript acessa o documento atual por meio de `document`:

```javascript
// Exibe o documento atual no console do navegador.
console.log(document);

// Exibe o elemento body do documento.
console.log(document.body);
```

**Uma distinção importante:** alterar o DOM muda a página em execução. Isso não reescreve automaticamente o arquivo `.html` salvo no computador. Ao recarregar, o navegador carrega os arquivos novamente.

**Verificação:** se você alterar um título pelo JavaScript, o texto original do arquivo HTML necessariamente será substituído no editor? Explique.

## 3. [BÁSICO] Preparando um laboratório pequeno

Antes do formulário completo, crie três arquivos na mesma pasta: `laboratorio.html`, `laboratorio.css` e `laboratorio.js`.

Os exemplos das seções 3 a 7 usam esses arquivos. **Acrescente os trechos JavaScript na ordem apresentada, sem repetir declarações já escritas.** Trechos identificados como demonstração isolada devem ser estudados separadamente.

### 3.1. HTML inicial — `laboratorio.html`

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <!-- Permite exibir corretamente acentos e outros caracteres. -->
  <meta charset="UTF-8">
  <!-- Adapta a área de visualização à largura do dispositivo. -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Laboratório de DOM e eventos</title>

  <!-- O CSS cuida da aparência. -->
  <link rel="stylesheet" href="laboratorio.css">
  <!-- defer faz este script externo executar após a análise do HTML. -->
  <script src="laboratorio.js" defer></script>
</head>
<body>
  <main>
    <h1 id="titulo">Minha casa interativa</h1>
    <p class="descricao">Use os controles para modificar a página.</p>

    <!-- O for do label corresponde ao id do input. -->
    <label for="visitante">Seu nome:</label>
    <input type="text" id="visitante">

    <!-- type="button" indica um botão de ação comum. -->
    <button type="button" id="btn-saudacao">Mostrar saudação</button>
    <p id="saudacao" aria-live="polite"></p>
    <p id="contador">Caracteres digitados: 0</p>

    <button type="button" id="btn-destaque" aria-pressed="false">
      Alternar destaque
    </button>
    <section id="painel" class="card">
      <h2>Painel da casa</h2>
      <p>Este painel pode receber destaque.</p>
    </section>
    <section class="card">
      <h2>Segundo painel</h2>
      <p>Usaremos este elemento para comparar seletores.</p>
    </section>
  </main>
</body>
</html>
```

`aria-live="polite"` identifica uma região cujas mudanças podem ser anunciadas por leitores de tela, sem interromper imediatamente a leitura atual. O aluno pode usar a interface visualmente; esse atributo também comunica alterações a tecnologias assistivas.

### 3.2. CSS inicial — `laboratorio.css`

```css
/* Faz largura e altura incluírem bordas e preenchimento. */
* {
  box-sizing: border-box;
}

/* Define a aparência geral do laboratório. */
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 24px;
  background-color: #f1f5f9;
  color: #172033;
}

/* Limita o conteúdo e o centraliza horizontalmente. */
main {
  max-width: 720px;
  margin: 0 auto;
}

/* Dá espaço aos controles. */
input, button {
  padding: 10px;
  margin: 6px 0;
}

/* Define a aparência normal dos painéis. */
.card {
  background-color: white;
  border: 2px solid #64748b;
  padding: 16px;
  margin-top: 16px;
}

/* Esta classe só produz efeito quando estiver no elemento. */
.destaque {
  background-color: #fef3c7;
  border-color: #92400e;
}
```

### 3.3. Por que usamos `defer`?

O navegador precisa reconhecer os elementos antes de o script tentar selecioná-los. Para este script externo, `defer` permite baixar o arquivo durante a leitura do HTML e executá-lo depois que o documento foi analisado.

É como instalar os interruptores antes de tentar conectá-los ao sistema da casa.

Sem esse cuidado, um script no `head` pode procurar um botão que o navegador ainda não encontrou. O resultado da seleção será `null`.

Para começar `laboratorio.js`, escreva:

```javascript
// Esta mensagem confirma que o arquivo foi carregado e executado.
console.log("JavaScript conectado ao laboratório!");
```

Abra `laboratorio.html` no navegador e consulte o Console nas ferramentas de desenvolvimento (geralmente F12). Se a mensagem não aparecer, confira o nome e o caminho do arquivo.

**Verificação:** `defer` serve para mudar a aparência da página ou para controlar o momento de execução do script?

---

## 4. [BÁSICO] Selecionar: localizar o elemento certo

Selecionar é obter uma referência para um elemento. Essa referência permite consultar ou modificar o elemento depois.

### 4.1. `getElementById`: busca por um ID

Acrescente em `laboratorio.js`:

```javascript
// Busca o elemento cujo atributo id é "titulo".
// Não colocamos #, pois este método já recebe especificamente um ID.
const titulo = document.getElementById("titulo");

// Exibe o elemento encontrado, não apenas seu texto.
console.log(titulo);

// Modifica o texto do elemento encontrado.
titulo.textContent = "Bem-vindo à casa interativa!";
```

Leitura da primeira instrução:

| Parte | Significado |
| --- | --- |
| `const` | Declara uma variável que não será reatribuída |
| `titulo` | Nome escolhido para guardar a referência |
| `document` | Documento atual |
| `getElementById(...)` | Método que procura um elemento pelo ID |
| `"titulo"` | Valor do atributo `id` procurado |

`const` não impede alterações no elemento. Impede que a variável receba outra referência. Assim, `titulo.textContent = "Outro texto"` é permitido.

### 4.2. `querySelector`: busca por seletor CSS

Acrescente:

```javascript
// O # indica um seletor de ID.
const campoVisitante = document.querySelector("#visitante");

// O ponto indica um seletor de classe: retorna a primeira correspondência.
const primeiroCard = document.querySelector(".card");

// Sem # ou ponto, estamos buscando pelo nome da tag.
const primeiroParagrafo = document.querySelector("p");

// O espaço indica descendência: procura um h2 dentro de #painel.
const tituloPainel = document.querySelector("#painel h2");

console.log(campoVisitante, primeiroCard, primeiroParagrafo, tituloPainel);
```

O ID deve ser único na página. A classe pode ser compartilhada por vários elementos.

### 4.3. `querySelectorAll`: selecionar todos

```javascript
// Guarda uma NodeList com todos os elementos de classe card.
const cards = document.querySelectorAll(".card");

// length informa quantos elementos foram encontrados.
console.log("Quantidade de cards:", cards.length);

// forEach executa a função uma vez para cada elemento da coleção.
// A cada repetição, card representa um dos elementos encontrados.
cards.forEach(function (card) {
  // Aqui apenas inspecionamos cada elemento no console.
  console.log(card);
});
```

A `NodeList` é uma coleção de nós; não é um único elemento nem exatamente um Array. A coleção devolvida por `querySelectorAll` é estática: não inclui automaticamente novos elementos criados depois da seleção.

| Busca | Retorno quando encontra | Quando não encontra |
| --- | --- | --- |
| `getElementById("titulo")` | Um elemento | `null` |
| `querySelector(".card")` | Primeiro elemento correspondente | `null` |
| `querySelectorAll(".card")` | Coleção de elementos | Coleção vazia, com `length` igual a 0 |

### 4.4. O significado de `null`

Demonstração isolada:

```javascript
// Este ID não existe no laboratório.
const aviso = document.querySelector("#aviso-inexistente");

// null significa que a busca não encontrou um elemento.
if (aviso === null) {
  console.log("Confira o ID no HTML e o momento em que o script executa.");
} else {
  // Só alteramos o texto se houver um elemento encontrado.
  aviso.textContent = "Elemento localizado!";
}
```

**Desafio:** há dois elementos com a classe `card`. Quantos são devolvidos por `querySelector(".card")`? E por `querySelectorAll(".card")`?

---

## 5. [BÁSICO] Ler e alterar conteúdo

### 5.1. `textContent`, `innerHTML` e `value`

As três propriedades têm finalidades diferentes:

| Propriedade | Uso nesta aula | Exemplo |
| --- | --- | --- |
| `textContent` | Texto de títulos, parágrafos e mensagens | `saida.textContent = "Olá!"` |
| `innerHTML` | Conteúdo interpretado como HTML | `saida.innerHTML = "<strong>Olá!</strong>"` |
| `value` | Valor atual de um campo | `campo.value` |

Acrescente ao laboratório:

```javascript
// Seleciona o parágrafo onde mostraremos a resposta.
const saida = document.querySelector("#saudacao");

// Mostra texto comum; não cria tags HTML.
saida.textContent = "Aguardando seu nome...";

// Consulta o valor atual do campo. No carregamento, ele costuma estar vazio.
console.log(campoVisitante.value);
```

Se o aluno digitar depois, aquele `console.log` não será repetido sozinho. Precisaremos ler o campo dentro de uma função executada em resposta a um evento.

### 5.2. Quando o texto contém tags

Demonstração isolada; execute uma atribuição por vez:

```javascript
// Mostra literalmente <strong>Olá!</strong>, com os sinais < e >.
saida.textContent = "<strong>Olá!</strong>";

// Interpreta a marcação e cria um elemento strong dentro da saída.
// Aqui a string é fixa e foi escrita pelo próprio programador.
saida.innerHTML = "<strong>Olá!</strong>";
```

Para mensagens e dados digitados pelo usuário, usaremos `textContent`: precisamos de texto, sem interpretar marcação. `innerHTML` não deve receber diretamente conteúdo não confiável, pois pode introduzir HTML indesejado e vulnerabilidades. Atribuir `innerHTML` também substitui o conteúdo interno existente.

### 5.3. Ler, tratar e mostrar

```javascript
// Lê o valor e remove espaços apenas das extremidades.
const nomeInicial = campoVisitante.value.trim();

// Mostra o resultado da leitura no console.
console.log("Nome lido no carregamento:", nomeInicial);
```

`trim()` devolve uma nova string. Não altera automaticamente o campo nem remove os espaços entre palavras. Por exemplo, `"  Ana Maria  ".trim()` resulta em `"Ana Maria"`.

**Verificação:** para descobrir o que foi digitado em um `input`, você usaria `textContent` ou `value`? Por quê?

---

## 6. [INTERMEDIÁRIO] Reagir a um clique

Um **evento** informa que algo aconteceu. Um **ouvinte de evento** é uma função registrada para reagir a essa ocorrência.

Acrescente em `laboratorio.js`:

```javascript
// Localiza o botão uma vez, durante a preparação da página.
const botaoSaudacao = document.querySelector("#btn-saudacao");

// Registra a reação ao evento click.
// A função abaixo não executa agora: será chamada a cada clique.
botaoSaudacao.addEventListener("click", function () {
  // Lê o valor atualizado do campo no momento do clique.
  const nome = campoVisitante.value.trim();

  // Se o campo estiver vazio ou contiver apenas espaços, orienta o usuário.
  if (nome === "") {
    saida.textContent = "Digite seu nome antes de continuar.";

    // Coloca o foco no campo, facilitando a digitação.
    campoVisitante.focus();

    // Encerra esta execução da função; não encerra todo o JavaScript.
    return;
  }

  // Só chegamos aqui quando o nome não está vazio.
  saida.textContent = "Olá, " + nome + "! Seja bem-vindo(a).";
});
```

### Entendendo a função de resposta

Nosso `addEventListener` recebe o nome do evento e a função que deverá reagir. Chamamos essa função de **callback**: ela é entregue a outra parte do programa para ser chamada no momento adequado.

Também podemos usar uma função com nome. Demonstração alternativa, não acrescente junto ao ouvinte anterior:

```javascript
// Define a ação, mas ainda não a executa.
function mostrarSaudacao() {
  saida.textContent = "Olá, visitante!";
}

// Passa a referência da função. Não coloque mostrarSaudacao() aqui.
botaoSaudacao.addEventListener("click", mostrarSaudacao);
```

Escrever `mostrarSaudacao()` nessa posição chamaria a função imediatamente e passaria seu retorno ao método, em vez de registrar a função desejada.

### Teste guiado

1. Clique com o campo vazio: deve aparecer uma orientação.
2. Digite apenas espaços: o comportamento deve ser o mesmo.
3. Digite `Ana`: deve aparecer a saudação.
4. Troque para `Pedro` e clique de novo: deve aparecer o nome atualizado.

**Verificação:** por que a leitura de `campoVisitante.value` foi colocada dentro da função do evento?

## 7. [INTERMEDIÁRIO] Classes, estilos e eventos de edição

### 7.1. `classList`: controlar a aparência com classes

Na analogia da casa, a decoração já está definida. O JavaScript decide quando ativá-la.

Acrescente:

```javascript
// Seleciona o painel e seu botão de controle.
const painel = document.querySelector("#painel");
const botaoDestaque = document.querySelector("#btn-destaque");

botaoDestaque.addEventListener("click", function () {
  // toggle adiciona a classe se ela não existir e remove se já existir.
  // O retorno informa se a classe ficou presente após a operação.
  const destacado = painel.classList.toggle("destaque");

  // Comunica também o estado do botão às tecnologias assistivas.
  botaoDestaque.setAttribute("aria-pressed", String(destacado));
});
```

`setAttribute` define um atributo do elemento. `String(destacado)` converte `true` ou `false` para o texto correspondente.

Compare as operações, em uma demonstração isolada:

```javascript
// Adiciona destaque; se a classe já existir, não a duplica.
painel.classList.add("destaque");

// Verifica a presença da classe e devolve true ou false.
console.log(painel.classList.contains("destaque"));

// Remove somente destaque, preservando outras classes, como card.
painel.classList.remove("destaque");
```

### 7.2. `style`: alterar uma propriedade diretamente

Demonstração isolada:

```javascript
// background-color, do CSS, vira backgroundColor no JavaScript.
painel.style.backgroundColor = "#dbeafe";

// A string vazia remove esta declaração inline.
// Assim, as regras da folha CSS voltam a definir o fundo normalmente.
painel.style.backgroundColor = "";
```

Usaremos classes para os estados visuais recorrentes. `style` é útil para valores pontuais ou calculados. Misturar um fundo inline com uma classe que também altera o fundo pode dificultar a percepção da mudança, pois o estilo inline geralmente prevalece sobre regras comuns da folha CSS.

### 7.3. `input`: acompanhar a edição

Acrescente:

```javascript
const contador = document.querySelector("#contador");

// Reage às alterações realizadas pelo usuário, inclusive colar e apagar.
campoVisitante.addEventListener("input", function () {
  // Lê o tamanho do valor atual.
  const quantidade = campoVisitante.value.length;

  // Atualiza o texto a cada edição.
  contador.textContent = "Caracteres digitados: " + quantidade;
});
```

Para textos simples, `length` funciona como a contagem esperada nesta atividade. Tecnicamente, strings JavaScript contam unidades UTF-16; alguns emojis podem ocupar mais de uma unidade. Não precisamos tratar isso neste exercício introdutório.

`input` não significa “qualquer tecla”: setas e Shift, por exemplo, não alteram o valor. Alterar `.value` pelo código também não dispara automaticamente esse evento.

### 7.4. `change`: acompanhar a confirmação de uma alteração

```javascript
// Em um campo de texto, costuma ocorrer ao sair do campo após alterá-lo.
campoVisitante.addEventListener("change", function () {
  console.log("Nome confirmado:", campoVisitante.value);
});
```

| Evento | Quando observar nesta aula | Uso |
| --- | --- | --- |
| `click` | Ativação do botão, inclusive por teclado | Mostrar saudação, limpar |
| `input` | Edição do valor pelo usuário | Contador, atualização durante digitação |
| `change` | Confirmação da mudança; em texto, ao perder foco após editar | Reagir ao valor concluído |
| `submit` | Tentativa de envio do formulário | Validar o conjunto de campos |

Em `select` e checkbox, `change` tem outro momento de confirmação, como selecionar uma opção ou marcar a caixa. Não é uma regra universal de “esperar perder o foco”.

**Experimento:** digite três letras, cole uma palavra, apague um caractere e pressione Tab. Observe a tela e o Console. Qual evento acompanhou a edição e qual indicou a confirmação?

---

## 8. [INTERMEDIÁRIO] Formulários, evento e `preventDefault()`

Um formulário reúne campos relacionados e oferece uma ação de envio. Por padrão, o envio pode navegar para o endereço indicado por `action`, levando os dados conforme o método configurado. Dependendo da configuração, isso pode carregar novamente a página atual ou abrir outra resposta.

Nesta aula, queremos conferir os dados e mostrar a resposta na própria interface, sem efetuar um envio real.

### Exemplo mínimo independente

Crie `formulario-minimo.html` e `formulario-minimo.js` na mesma pasta.

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Formulário mínimo</title>
  <!-- Executa o código depois de o formulário existir no DOM. -->
  <script src="formulario-minimo.js" defer></script>
</head>
<body>
  <!-- Este exemplo não exige regras nativas de validação. -->
  <form id="form-minimo">
    <label for="apelido">Apelido:</label>
    <input id="apelido" name="apelido" type="text">
    <button type="submit">Enviar</button>
  </form>
  <p id="resultado" role="status"></p>
</body>
</html>
```

```javascript
// Seleciona os três elementos usados neste exemplo independente.
const formulario = document.querySelector("#form-minimo");
const campoApelido = document.querySelector("#apelido");
const resultado = document.querySelector("#resultado");

// Escuta o formulário: isso contempla também o envio pelo teclado.
formulario.addEventListener("submit", function (evento) {
  // Cancela a ação padrão de envio deste evento.
  evento.preventDefault();

  // Exibe o valor sem enviá-lo a um servidor.
  resultado.textContent = "Apelido informado: " + campoApelido.value;
});
```

O navegador entrega o objeto do evento à função. O parâmetro poderia se chamar `evento`, `event` ou `e`; o importante é usar o mesmo nome dentro da função.

| Recurso | Papel |
| --- | --- |
| `evento.type` | Informa o tipo do evento |
| `evento.target` | Elemento em que o evento se originou |
| `evento.currentTarget` | Elemento cujo ouvinte está sendo executado |
| `evento.preventDefault()` | Cancela a ação padrão, quando o evento é cancelável |
| `return` | Encerra a execução da função naquele ponto |

`preventDefault()` não encerra a função, não valida campos e não impede por si só a propagação do evento. As instruções seguintes continuam executando.

**Verificação:** se a função chamar apenas `preventDefault()`, o navegador já saberá que a senha precisa ter seis caracteres?

---

## 9. [APLICAÇÃO] Planejar o formulário antes de programar

**Contexto:** a escola quer um protótipo de inscrição em uma oficina. O aluno deverá preencher nome, e-mail e uma senha de demonstração.

O objetivo é validar a interface. **Nenhuma conta será criada, nenhum dado será enviado e nada será salvo.** Use dados fictícios nos testes.

### Regras da atividade

| Campo | Regra | Mensagem esperada |
| --- | --- | --- |
| Nome | Não pode ficar vazio nem conter apenas espaços | Informe seu nome. |
| E-mail | Obrigatório e compatível com a validação de `type="email"` | Informe seu e-mail. / Digite um e-mail em formato válido. |
| Senha | Pelo menos seis unidades de comprimento, usando textos simples no exercício | Use pelo menos 6 caracteres na senha. |

Seis caracteres é uma regra didática herdada da atividade, não uma recomendação de política de senhas para sistemas reais. Não usaremos `trim()` na senha, pois isso mudaria silenciosamente o valor informado.

### Por que melhorar o teste do e-mail?

Verificar apenas `email.includes("@")` rejeita alguns erros, mas aceita textos como `@`. No projeto completo, consultaremos `campoEmail.validity.typeMismatch`, que informa se o valor não combina com o formato de um campo de e-mail.

Essa checagem não comprova que a caixa postal existe nem que pertence à pessoa. O navegador pode aceitar endereços como `a@b`; não estamos impondo uma regra adicional de domínio com ponto.

### Validação nativa e validação controlada pelo JavaScript

O HTML pode validar campos com `required`, `type="email"` e `minlength`. Quando a validação nativa interativa bloqueia o envio, o evento `submit` normalmente não é disparado.

No nosso formulário, `novalidate` desativa esse bloqueio automático no envio. Assim, o ouvinte de `submit` executa e mostra as mensagens da atividade. As restrições continuam disponíveis para consulta por `validity`.

A validação no navegador ajuda a orientar o usuário. Um sistema real também precisa validar no servidor, pois o código da interface pode ser contornado.

### Algoritmo em linguagem natural

1. Interceptar o envio.
2. Apagar os avisos da tentativa anterior.
3. Ler os valores atuais.
4. Verificar cada regra e mostrar os erros encontrados.
5. Se houver erro, focar o primeiro campo inválido.
6. Se tudo estiver correto, mostrar que a validação passou.
7. Ao limpar, apagar valores, mensagens e estados visuais.

**Pergunta para a turma:** por que apagar as mensagens anteriores antes de avaliar os dados novamente?

---

## 10. [APLICAÇÃO] Atividade prática — Checkpoint 3

**Formato:** individual, com revisão em dupla. **Tempo:** 90 minutos para construção e testes.

Crie `cadastro.html`, `cadastro.css` e `cadastro.js` na mesma pasta. Esta é uma atividade independente: não carregue `laboratorio.js` junto com ela.

Implemente:

1. Formulário com nome, e-mail e senha, cada campo com seu `label`.
2. Mensagem específica próxima de cada campo.
3. Captura de `submit` com `addEventListener` e `preventDefault()`.
4. As regras da seção anterior.
5. Borda de erro por meio de uma classe CSS.
6. Exibição de todos os erros da tentativa, não apenas do primeiro.
7. Remoção dos erros antigos quando a validação for refeita.
8. Mensagem de sucesso somente quando todas as regras passarem.
9. Botão Limpar que restaure a interface.
10. Comentários que expliquem a finalidade das partes do código.

Antes de consultar o gabarito, escreva em português o que acontecerá no envio e no clique em Limpar. Depois transforme cada ação em código.

---

## 11. Gabarito completo e comentado

### 11.1. `cadastro.html`

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <!-- Define a codificação dos caracteres. -->
  <meta charset="UTF-8">
  <!-- Permite que o layout se adapte a telas menores. -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Inscrição na oficina</title>

  <!-- Liga a folha de estilos ao documento. -->
  <link rel="stylesheet" href="cadastro.css">
  <!-- Garante a execução após a análise do HTML. -->
  <script src="cadastro.js" defer></script>
</head>
<body>
  <main class="container">
    <h1>Inscrição na oficina</h1>
    <p>Preencha os campos para testar a validação.</p>
    <p id="orientacao">Protótipo didático: os dados não serão enviados ou salvos.</p>

    <!-- novalidate permite que o JS controle as mensagens no submit. -->
    <form id="form-cadastro" novalidate aria-describedby="orientacao">
      <div class="grupo">
        <!-- for associa o rótulo ao campo que tem esse id. -->
        <label for="nome">Nome (obrigatório)</label>
        <!-- name identifica o campo em um eventual envio real. -->
        <input type="text" id="nome" name="nome" required
               autocomplete="name" aria-describedby="erro-nome">
        <!-- O parágrafo começa vazio e receberá a mensagem de erro. -->
        <p id="erro-nome" class="erro" aria-live="polite"></p>
      </div>

      <div class="grupo">
        <label for="email">E-mail (obrigatório)</label>
        <!-- type=email disponibiliza a regra de formato do navegador. -->
        <input type="email" id="email" name="email" required
               autocomplete="email" aria-describedby="erro-email">
        <p id="erro-email" class="erro" aria-live="polite"></p>
      </div>

      <div class="grupo">
        <label for="senha">Senha de demonstração (obrigatória)</label>
        <!-- password mascara a exibição; isso não criptografa o valor. -->
        <input type="password" id="senha" name="senha" required minlength="6"
               autocomplete="new-password"
               aria-describedby="ajuda-senha erro-senha">
        <small id="ajuda-senha">Use pelo menos 6 caracteres fictícios.</small>
        <p id="erro-senha" class="erro" aria-live="polite"></p>
      </div>

      <div class="acoes">
        <!-- Dispara a tentativa de envio do formulário. -->
        <button type="submit">Validar inscrição</button>
        <!-- Não envia o formulário; sua ação será definida no JS. -->
        <button type="button" id="btn-limpar" class="secundario">Limpar</button>
      </div>

      <!-- Região persistente para anunciar o resultado geral. -->
      <p id="sucesso" role="status" aria-live="polite"></p>
    </form>
  </main>
</body>
</html>
```

`id` permite selecionar e associar rótulos. `name` é a identificação utilizada na construção dos dados de um envio. Eles têm papéis diferentes, mesmo quando usamos valores iguais.

`aria-describedby` liga o campo ao texto de ajuda ou erro. Mais adiante, `aria-invalid` comunicará se identificamos um erro. A mensagem escrita acompanha a borda vermelha, para não depender somente de cor.

### 11.2. `cadastro.css`

```css
/* Inclui padding e borda no cálculo de tamanho dos elementos. */
* {
  box-sizing: border-box;
}

/* Estabelece a aparência geral da página. */
body {
  margin: 0;
  padding: 24px;
  font-family: Arial, sans-serif;
  line-height: 1.5;
  color: #172033;
  background-color: #f1f5f9;
}

/* Centraliza o formulário sem fixar uma largura maior que a tela. */
.container {
  width: 100%;
  max-width: 580px;
  margin: 0 auto;
  padding: 24px;
  background-color: #ffffff;
  border-radius: 12px;
}

/* Separa visualmente os grupos de campos. */
.grupo {
  margin-bottom: 16px;
}

/* Coloca o rótulo acima do campo. */
label {
  display: block;
  margin-bottom: 6px;
  font-weight: bold;
}

/* Mantém os campos dentro da largura disponível. */
input {
  width: 100%;
  padding: 10px;
  border: 2px solid #64748b;
  border-radius: 6px;
  font: inherit;
}

/* Mantém uma indicação visível de foco para navegação por teclado. */
input:focus-visible, button:focus-visible {
  outline: 3px solid #2563eb;
  outline-offset: 3px;
}

/* O JavaScript aplica esta classe quando encontra um erro. */
.campo-invalido {
  border-color: #b91c1c;
  background-color: #fff7f7;
}

/* As mensagens são texto, além da indicação visual por cor. */
.erro {
  color: #b91c1c;
  margin: 4px 0 0;
  font-size: 0.9rem;
}

/* Organiza os botões e permite quebra em telas estreitas. */
.acoes {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

/* Estilo padrão dos botões. */
button {
  border: 0;
  border-radius: 6px;
  padding: 12px 16px;
  font: inherit;
  cursor: pointer;
  background-color: #1d4ed8;
  color: #ffffff;
}

/* Diferencia a ação secundária. */
.secundario {
  background-color: #334155;
}

/* Só é aplicada quando a validação passa. */
.sucesso {
  padding: 12px;
  background-color: #dcfce7;
  color: #14532d;
  border-radius: 6px;
}
```

### 11.3. `cadastro.js`

```javascript
// ============================================================
// 1. SELEÇÃO: guarda referências aos elementos usados na página.
// Estas instruções executam quando o script é carregado.
// ============================================================
const formulario = document.querySelector("#form-cadastro");
const campoNome = document.querySelector("#nome");
const campoEmail = document.querySelector("#email");
const campoSenha = document.querySelector("#senha");

const erroNome = document.querySelector("#erro-nome");
const erroEmail = document.querySelector("#erro-email");
const erroSenha = document.querySelector("#erro-senha");

const mensagemSucesso = document.querySelector("#sucesso");
const botaoLimpar = document.querySelector("#btn-limpar");

// ============================================================
// 2. FUNÇÕES AUXILIARES: agrupam tarefas que serão repetidas.
// Declarar uma função não significa executá-la agora.
// ============================================================

function mostrarErro(campo, elementoMensagem, texto) {
  // Escreve texto simples no parágrafo correspondente.
  elementoMensagem.textContent = texto;

  // Ativa a aparência de erro definida no CSS.
  campo.classList.add("campo-invalido");

  // Comunica o estado inválido às tecnologias assistivas.
  campo.setAttribute("aria-invalid", "true");
}

function limparErro(campo, elementoMensagem) {
  // Apaga a mensagem anterior desse campo.
  elementoMensagem.textContent = "";

  // Remove apenas a classe de erro; preserva as demais classes.
  campo.classList.remove("campo-invalido");

  // Remove o estado de erro anteriormente informado.
  campo.removeAttribute("aria-invalid");
}

function limparSucesso() {
  // Evita manter uma confirmação que pertence a dados anteriores.
  mensagemSucesso.textContent = "";
  mensagemSucesso.classList.remove("sucesso");
}

function limparMensagens() {
  // Reutiliza a mesma função para os três campos.
  limparErro(campoNome, erroNome);
  limparErro(campoEmail, erroEmail);
  limparErro(campoSenha, erroSenha);
  limparSucesso();
}

// ============================================================
// 3. ENVIO: executa a validação em cada tentativa de submit.
// ============================================================
formulario.addEventListener("submit", function (evento) {
  // Cancela o envio padrão. O protótipo não envia dados.
  evento.preventDefault();

  // Começa esta tentativa sem mensagens da tentativa anterior.
  limparMensagens();

  // Lê os valores atuais, não os valores do carregamento da página.
  const nome = campoNome.value.trim();
  const email = campoEmail.value.trim();
  const senha = campoSenha.value;

  // Atualiza o e-mail com o valor tratado antes de consultar validity.
  campoEmail.value = email;

  // Assume validade inicialmente. Qualquer erro mudará para false.
  // É let porque a variável pode receber outro valor nesta execução.
  let valido = true;

  // REGRA 1: nome vazio, inclusive quando composto apenas por espaços.
  if (nome === "") {
    mostrarErro(campoNome, erroNome, "Informe seu nome.");
    valido = false;
  }

  // REGRA 2: primeiro trata a ausência do e-mail.
  if (email === "") {
    mostrarErro(campoEmail, erroEmail, "Informe seu e-mail.");
    valido = false;
  } else if (campoEmail.validity.typeMismatch) {
    // true indica incompatibilidade com o formato de type="email".
    mostrarErro(campoEmail, erroEmail, "Digite um e-mail em formato válido.");
    valido = false;
  }

  // REGRA 3: verifica o comprimento sem modificar a senha.
  if (senha.length < 6) {
    mostrarErro(campoSenha, erroSenha, "Use pelo menos 6 caracteres na senha.");
    valido = false;
  }

  // As regras são independentes: todos os erros já foram apresentados.
  if (!valido) {
    // !valido significa "não válido".
    // Localiza o primeiro campo marcado, na ordem do formulário.
    const primeiroInvalido = formulario.querySelector(".campo-invalido");

    // Direciona a navegação para a primeira correção necessária.
    primeiroInvalido.focus();

    // Encerra esta tentativa para não mostrar a confirmação.
    return;
  }

  // Só executa quando nenhuma regra marcou valido como false.
  mensagemSucesso.textContent = "Dados válidos! Inscrição simulada; nada foi enviado ou salvo.";
  mensagemSucesso.classList.add("sucesso");
});

// ============================================================
// 4. EDIÇÃO: retira confirmações antigas quando os dados mudam.
// ============================================================
formulario.addEventListener("input", function () {
  // O evento dos campos se propaga até o formulário.
  // Isso permite observar a edição dos três campos com um ouvinte.
  limparSucesso();

  // Neste projeto, os erros são reavaliados no próximo submit.
  // Não afirmamos que um campo ficou válido só porque foi editado.
});

// ============================================================
// 5. LIMPEZA: restaura valores e remove estados visuais.
// ============================================================
botaoLimpar.addEventListener("click", function () {
  // reset restaura os valores iniciais definidos no HTML.
  // Como os campos deste HTML começam vazios, voltam a ficar vazios.
  formulario.reset();

  // reset não apaga mensagens ou classes adicionadas pelo JS.
  limparMensagens();

  // Deixa a interface pronta para um novo preenchimento.
  campoNome.focus();
});
```

---

## 12. Entendendo as decisões do gabarito

### 12.1. Por que criar funções auxiliares?

Nome, e-mail e senha precisam da mesma sequência: escrever mensagem, adicionar classe e informar estado inválido. A função `mostrarErro` descreve essa tarefa uma vez.

Na chamada `mostrarErro(campoNome, erroNome, "Informe seu nome.")`, os três argumentos ocupam os parâmetros `campo`, `elementoMensagem` e `texto`. É como uma ordem de manutenção que informa **onde agir**, **onde registrar o aviso** e **qual aviso escrever**.

Não estamos passando strings com os nomes das variáveis. Estamos passando referências reais aos elementos, além do texto da mensagem.

### 12.2. Por que usar vários `if`?

Nome, e-mail e senha podem estar errados ao mesmo tempo. Por isso, as verificações dos campos são independentes. Um único encadeamento `if / else if / else if` entre os três campos mostraria apenas um dos erros por tentativa.

Dentro da regra do e-mail, usamos `else if`: quando ele está vazio, preferimos a mensagem de obrigatoriedade. A avaliação de formato fica para um valor preenchido.

### 12.3. Por que `valido` começa como `true` a cada envio?

Cada tentativa precisa ser avaliada novamente. Começamos supondo que passará; qualquer regra descumprida muda o resultado para `false`. Um campo correto não pode restaurar `true` depois que outro campo já encontrou um erro.

| Momento | Dados | Valor de `valido` |
| --- | --- | --- |
| Início da tentativa | Ainda não avaliados | `true` |
| Nome | Vazio | `false` |
| E-mail | Correto | Continua `false` |
| Senha | Correta | Continua `false` |
| Resultado | Há erro no nome | Não mostra sucesso |

### 12.4. Por que limpar a interface também?

Apagar o valor de um campo não apaga automaticamente o parágrafo de erro. São elementos diferentes. Da mesma forma, `reset()` não remove classes nem mensagens personalizadas.

O estado da interface inclui **valores**, **mensagens**, **classes** e **atributos**. Limpar a atividade significa cuidar de todos esses elementos.

### 12.5. Por que a confirmação some durante a edição?

Se o usuário validou os dados e depois apagou o nome, a confirmação anterior não descreve mais os valores atuais. O evento `input` remove essa confirmação. Os erros serão recalculados no próximo envio.

Neste exemplo, escutamos `input` no formulário porque esse evento se propaga a partir dos campos. Essa propagação é chamada de **bubbling**. Se precisássemos saber qual campo foi editado, poderíamos consultar `evento.target`.

**Verificação:** o que aconteceria se `limparMensagens()` não fosse chamado no início de uma nova tentativa?

---

## 13. Roteiro de testes em dupla

Um colega opera a página; o outro confere o resultado. Depois, troquem os papéis. Abra o Console e confirme também se aparecem erros de JavaScript.

| Caso | Ação ou dados | Resultado esperado |
| --- | --- | --- |
| 1 | Enviar tudo vazio | Três mensagens; foco no nome; sem confirmação |
| 2 | Nome apenas com espaços; demais campos corretos | Erro no nome |
| 3 | Nome `Ana`, e-mail `ana`, senha `abc123` | Erro de formato do e-mail |
| 4 | Nome `Ana`, e-mail `@`, senha `abc123` | Erro de formato do e-mail |
| 5 | Nome `Ana`, e-mail `ana@example.com`, senha `12345` | Erro na senha |
| 6 | Mesmos dados, senha `123456` | Confirmação de validação |
| 7 | Corrigir todos os campos após uma tentativa inválida e reenviar | Erros e bordas removidos; confirmação visível |
| 8 | Após validar, editar qualquer campo | Confirmação anterior removida |
| 9 | Após validar, apagar o nome e reenviar | Erro de nome; sem confirmação |
| 10 | Clicar em Limpar após erros | Campos vazios; sem mensagens ou bordas de erro; foco no nome |
| 11 | Clicar em Limpar após sucesso | Campos vazios; sem confirmação |
| 12 | Preencher corretamente e enviar com Enter em um campo | Mesmo fluxo de validação do botão |
| 13 | Navegar usando Tab | Foco visível e controles acessíveis pelo teclado |

Use senha fictícia. Não registre senhas no Console. A evidência da atividade deve mostrar os resultados da interface, sem dados pessoais reais.

### Critérios sugeridos de revisão

| Critério | Pontos |
| --- | --- |
| Arquivos conectados e seletores corretos | 2 |
| Uso de eventos e cancelamento do envio | 2 |
| Regras e mensagens específicas funcionando | 2 |
| Correção de erros anteriores e limpeza completa | 2 |
| Clareza dos comentários e execução dos testes | 2 |
| **Total** | **10** |

---

## 14. Erros frequentes e como investigar

| Sintoma | Causa provável | O que verificar |
| --- | --- | --- |
| Erro ao chamar `addEventListener` de `null` | Elemento não encontrado | ID, seletor, arquivo HTML aberto e `defer` |
| Clique não produz efeito | JS não carregou ou parou por erro anterior | Caminho do script e Console |
| Só o primeiro card muda | Uso de `querySelector` | Para vários, usar `querySelectorAll` e percorrer |
| Nome digitado não aparece | Leitura de propriedade errada ou leitura antecipada | Usar `.value` dentro do evento |
| Página navega no envio | Comportamento padrão não foi cancelado | `preventDefault()` no ouvinte de `submit` |
| Mensagem nativa aparece antes da personalizada | Validação nativa bloqueou o envio | Nesta proposta, conferir `novalidate` |
| Borda vermelha continua após correção | Classe antiga não foi removida | Limpeza antes da nova validação |
| Sucesso continua com dados alterados | Confirmação antiga não foi apagada | Ouvinte de `input` e `limparSucesso()` |
| Botão Limpar também envia | Botão sem tipo adequado | Usar `type="button"` |
| `Identifier ... has already been declared` | Declarações duplicadas no mesmo escopo | Não copiar alternativas juntas nem carregar script duas vezes |

**Estratégia:** leia a primeira mensagem de erro do Console, encontre a linha indicada e confira os elementos envolvidos. Não troque várias partes ao mesmo tempo sem verificar a causa.

---

## 15. Perguntas de fixação

Responda antes de consultar a seção seguinte:

1. O que é o DOM e como ele se relaciona com o HTML?
2. Qual a diferença entre `querySelector` e `querySelectorAll`?
3. Por que `getElementById("nome")` não usa `#`, mas `querySelector("#nome")` usa?
4. Qual propriedade lê o conteúdo digitado em um campo?
5. Quando usar `textContent` em vez de `innerHTML`?
6. O que `classList.toggle()` faz em dois cliques seguidos?
7. Qual a diferença entre `input` e `change` em um campo de texto?
8. O que `preventDefault()` cancela no formulário? Ele encerra a função?
9. Por que escutar `submit` no formulário, em vez de apenas `click` no botão?
10. Para que serve `novalidate` neste projeto?
11. Por que `reset()` sozinho não limpa toda a interface?
12. Mostrar uma mensagem de sucesso significa que os dados foram salvos?

## 16. Gabarito comentado das perguntas

1. O DOM é a representação do documento em objetos e relações de árvore. O HTML participa da construção dessa estrutura, e o JavaScript pode modificá-la durante a execução.
2. `querySelector` retorna a primeira correspondência ou `null`. `querySelectorAll` retorna uma coleção estática com todas as correspondências, possivelmente vazia.
3. O primeiro método já espera um ID. O segundo espera um seletor CSS; nesse formato, `#` indica ID.
4. `.value`. Usamos `.textContent` para textos de elementos como parágrafos e títulos.
5. Para texto simples, especialmente conteúdo vindo do usuário. `innerHTML` interpreta marcação e precisa de cuidados com conteúdo não confiável.
6. Se a classe começar ausente, o primeiro clique adiciona e o segundo remove. Se começar presente, ocorre o inverso.
7. `input` acompanha alterações do valor pelo usuário, incluindo colar e apagar. Em um campo de texto, `change` ocorre ao confirmar a mudança ao sair do campo.
8. Cancela a ação padrão de envio daquele evento. A função continua; `return` pode encerrá-la em um ponto escolhido.
9. O envio pode ocorrer pelo teclado. A validação pertence à ação do formulário, não exclusivamente ao mouse.
10. Desativa o bloqueio automático da validação nativa no envio, permitindo que nosso código de `submit` conduza as mensagens. Não apaga as restrições dos campos.
11. Ele restaura os valores iniciais dos controles; não remove mensagens, classes e atributos personalizados adicionados pelo script.
12. Não. Nosso código só altera a interface. Persistência exigiria uma implementação adicional de armazenamento; neste exercício ela não existe.

---

## 17. [AVANÇADO — OPCIONAL] Desafios de extensão

### Desafio A — Confirmação de senha

Adicione um campo para repetir a senha. No envio, compare os valores com `===` e mostre uma mensagem quando forem diferentes. Inclua o novo campo nas funções de limpeza e nos testes.

**Pergunta:** se a confirmação estiver correta e a primeira senha for alterada depois, o que deverá acontecer no próximo envio?

### Desafio B — Contador com aviso visual

Acrescente uma descrição da inscrição e um contador. Ao ultrapassar 100 caracteres, adicione uma classe de aviso. Ao voltar ao limite permitido, remova-a.

**Pergunta:** por que uma classe adicionada precisa também ter uma condição de remoção?

### Desafio C — Mostrar e ocultar senha

Crie um botão `type="button"` que alterne o `type` do campo entre `password` e `text`. Atualize também o texto do botão e seu estado `aria-pressed`.

**Pergunta:** mudar a forma de exibição da senha altera seu valor?

### Desafio D — Revalidar um campo durante a edição

Após uma tentativa inválida, revalide o campo editado no evento `input`. Remova o aviso somente se a regra passar. Uma forma de organizar isso é criar uma função de validação para cada campo e reutilizá-la no envio e na edição.

**Pergunta:** por que apenas apagar a mensagem ao digitar não garante que o problema foi resolvido?

## 18. Fechamento da aula

Volte à casa interativa: o DOM fornece os objetos acessíveis, os seletores localizam cada objeto, os eventos informam uma ação e a função decide a resposta.

**Bilhete de saída:** explique, em até cinco linhas, o que acontece desde o clique em “Validar inscrição” até o aparecimento de um erro. Use os termos **evento**, **value**, **condição**, **textContent** e **classList**.

## 19. Referências técnicas

Exemplos, analogias, atividades e roteiro elaborados para esta aula. As referências abaixo documentam os comportamentos das APIs; não são estudos de eficácia pedagógica. Documentação de atualização contínua, consultada em 8 de outubro de 2026; “s.d.” indica ausência de um ano único de publicação.

- WHATWG. (2026). **DOM Standard**. Estrutura do DOM, seletores e eventos. https://dom.spec.whatwg.org/
- WHATWG. (s.d.). **HTML Standard — Forms**. Formulários e regras da plataforma. https://html.spec.whatwg.org/multipage/forms.html
- MDN contributors. (s.d.). **Document: querySelector() method**. https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector
- MDN contributors. (s.d.). **Node: textContent property**. https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent
- MDN contributors. (s.d.). **Element: input event**. https://developer.mozilla.org/en-US/docs/Web/API/Element/input_event
- MDN contributors. (s.d.). **HTMLElement: change event**. https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/change_event
- MDN contributors. (s.d.). **HTMLFormElement: submit event**. https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/submit_event
- MDN contributors. (s.d.). **HTMLFormElement**. https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement
- MDN contributors. (s.d.). **The Script element**. https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script
