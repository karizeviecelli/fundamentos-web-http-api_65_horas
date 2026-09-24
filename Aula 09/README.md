# Aula 09 — JavaScript: Lógica e Fundamentos

**Módulo:** Fundamentos de Arquitetura Web, Protocolo HTTP, Conceitos de API REST, HTML, CSS e JavaScript
**Carga horária da aula:** 4 horas
**Professor(a):** @karizeviecelli

---

## 🎯 Objetivos da Aula

Ao final desta aula, você será capaz de:

- Declarar variáveis com `let` e `const`, entendendo a diferença entre elas.
- Reconhecer os principais tipos de dados do JavaScript.
- Usar operadores aritméticos, de comparação e lógicos.
- Escrever estruturas condicionais (`if`, `else if`, `else`).
- Escrever estruturas de repetição (`for`, `while`).
- Declarar e usar funções, com parâmetros e retorno.

---

## 🖼️ Analogia Inicial: dando um cérebro à casa inteligente

Nas últimas aulas, construímos a estrutura (HTML), decoramos e organizamos os cômodos (CSS) — mas a casa ainda era **estática**. Ela tinha uma forma bonita, mas não pensava, não reagia, não decidia nada sozinha.

O JavaScript é o **cérebro e o sistema nervoso** dessa casa. É ele que permite:

- **Guardar informações** (`let`, `const`): como gavetas etiquetadas onde a casa guarda dados — "temperatura atual", "número de visitantes hoje".
- **Tomar decisões** (`if`/`else`): como um termostato que decide "se a temperatura passar de 25°C, ligue o ar-condicionado; senão, deixe desligado".
- **Repetir tarefas** (`for`/`while`): como uma rotina automática — "regue cada uma das 10 plantas do jardim, uma de cada vez".
- **Empacotar comportamentos reutilizáveis** (funções): como um "modo" programado no controle remoto — "modo cinema" liga várias coisas de uma vez, sempre que acionado.

A partir de hoje, a casa deixa de ser só uma bela fachada e passa a **reagir** ao que acontece — o primeiro passo antes de fazer ela reagir a cliques e eventos reais (isso vem na próxima aula).

---

## 📚 Conteúdo Teórico

### 1. Variáveis: `let` e `const`

```javascript
// let: usado quando o valor PODE mudar depois
let temperatura = 22;
temperatura = 25; // permitido — o valor foi reatribuído

// const: usado quando o valor NÃO deve mudar depois de definido
const nomeDaCasa = "Casa Inteligente";
// nomeDaCasa = "Outro nome"; // ERRO! const não pode ser reatribuída

// Boa prática: use const por padrão, e só troque para let quando
// tiver certeza de que o valor vai precisar mudar
```

`var` também existe no JavaScript (é a forma mais antiga), mas hoje é evitada em código novo por causa de comportamentos confusos com escopo. Nas nossas aulas, usamos sempre `let` ou `const`.

### 2. Tipos de dados

```javascript
// Number: números, inteiros ou decimais
let idade = 25;
let preco = 19.90;

// String: texto, entre aspas simples, duplas ou crase
let nome = "Maria";
let saudacao = `Olá, ${nome}!`; // template literal — permite interpolar variáveis com ${}

// Boolean: verdadeiro ou falso
let ligado = true;
let desligado = false;

// Array: uma lista de valores
let comodos = ["sala", "cozinha", "quarto"];

// Object: uma coleção de dados relacionados, em pares chave-valor
let casa = {
  nome: "Casa Inteligente",
  quartos: 3,
  temInternet: true
};
```

### 3. Operadores aritméticos, de comparação e lógicos

```javascript
// ARITMÉTICOS: fazem contas
let soma = 10 + 5;        // 15
let resto = 10 % 3;       // 1 (resto da divisão — muito usado em lógica)

// DE COMPARAÇÃO: retornam true ou false
let igual = (5 === 5);     // true — === compara valor E tipo (recomendado)
let diferente = (5 !== 3); // true
let maior = (10 > 5);      // true

// LÓGICOS: combinam condições
let podeEntrar = (idade >= 18) && (temIngresso === true); // AND: as duas precisam ser true
let temDesconto = (ehEstudante === true) || (ehIdoso === true); // OR: basta uma ser true
let naoEstaChovendo = !estaChovendo; // NOT: inverte o valor
```

Use sempre `===` (comparação estrita) em vez de `==`. O `==` tenta "converter" tipos diferentes antes de comparar, o que gera resultados inesperados (`"5" == 5` é `true`, mas `"5" === 5` é `false`).

### 4. Estruturas condicionais

```javascript
let temperatura = 28;

if (temperatura > 25) {
  console.log("Ligar o ar-condicionado");
} else if (temperatura < 18) {
  console.log("Ligar o aquecedor");
} else {
  console.log("Temperatura agradável, nada a fazer");
}
```

O `if` testa uma condição; se for `true`, executa o bloco. `else if` testa uma condição alternativa, e `else` é o "caso contrário", executado quando nenhuma condição anterior foi verdadeira.

### 5. Estruturas de repetição

```javascript
// FOR: repete um número definido de vezes — ótimo quando você sabe quantas vezes repetir
for (let i = 0; i < 5; i++) {
  console.log("Regando a planta número " + i);
}
// i = 0 (início) | i < 5 (condição de parada) | i++ (incremento a cada volta)

// WHILE: repete enquanto uma condição for verdadeira — ótimo quando não se sabe o número exato de repetições
let bateria = 100;
while (bateria > 0) {
  bateria -= 20;
  console.log("Bateria em: " + bateria + "%");
}
```

Cuidado com **loops infinitos**: se a condição do `while` nunca se tornar falsa, o código trava o navegador. Sempre garanta que algo dentro do loop está caminhando em direção ao fim da condição.

### 6. Funções

```javascript
// Declaração de uma função — um "modo programado" reutilizável
function ligarModoCinema(nomeDoComodo) {
  console.log("Apagando luzes de: " + nomeDoComodo);
  console.log("Ligando a TV");
  console.log("Fechando as cortinas");
}

// Chamando (executando) a função, passando um parâmetro
ligarModoCinema("sala");

// Função com retorno de valor
function calcularMediaFinal(nota1, nota2) {
  const media = (nota1 + nota2) / 2;
  return media; // devolve o valor para quem chamou a função
}

const resultado = calcularMediaFinal(8, 6);
console.log(resultado); // 7
```

Uma função "empacota" um comportamento para ser reutilizado quantas vezes for necessário, sem repetir código. Os valores entre parênteses na declaração (`nomeDoComodo`, `nota1`, `nota2`) são os **parâmetros** — as "variáveis de entrada" da função. `return` define o que a função "devolve" para quem a chamou.

---

<a id="atividade"></a>
## 💻 Atividade Prática

**Duração sugerida:** 90 minutos
**Formato:** individual, com apoio em dupla

### Parte 1 — Exercícios de lógica de programação

Crie um arquivo `logica.js` (conectado a um `logica.html` simples, ou testado direto no Console do navegador) e resolva:

1. Declare uma variável `idade` e um `if`/`else` que imprima `"Maior de idade"` se for 18 ou mais, e `"Menor de idade"` caso contrário.
2. Usando um `for`, imprima os números de 1 a 10 no console.
3. Usando um `for`, imprima apenas os números PARES de 1 a 20 (dica: use o operador `%`).
4. Crie uma função `calcularMedia(a, b, c)` que receba 3 notas e retorne a média das três.
5. Crie uma função `classificarNota(media)` que receba uma média e retorne `"Aprovado"` se for maior ou igual a 6, e `"Reprovado"` caso contrário.
6. Usando um `while`, simule uma contagem regressiva de 10 até 0, imprimindo cada número.

### Parte 2 — Perguntas de fixação

Responda com suas palavras:

1. Qual a diferença prática entre `let` e `const`? Quando usar cada uma?
2. Por que `===` é preferível a `==` no JavaScript?
3. Quando usar `for` em vez de `while`, e vice-versa?
4. O que é um "parâmetro" de uma função, e o que faz o `return`?

Quando terminar, confira sua resposta no gabarito.

[Ver Gabarito »](#gabarito)

---

<a id="gabarito"></a>
## ✅ Gabarito

### Parte 1 — Exercícios de lógica de programação

```javascript
// 1)
let idade = 20;
if (idade >= 18) {
  console.log("Maior de idade");
} else {
  console.log("Menor de idade");
}

// 2)
for (let i = 1; i <= 10; i++) {
  console.log(i);
}

// 3)
for (let i = 1; i <= 20; i++) {
  if (i % 2 === 0) {
    console.log(i);
  }
}

// 4)
function calcularMedia(a, b, c) {
  return (a + b + c) / 3;
}

// 5)
function classificarNota(media) {
  if (media >= 6) {
    return "Aprovado";
  } else {
    return "Reprovado";
  }
}

// 6)
let contador = 10;
while (contador >= 0) {
  console.log(contador);
  contador--;
}
```

### Parte 2 — Perguntas de fixação

1. `let` permite reatribuir o valor da variável depois de declarada; `const` não permite (o valor fica constante). Use `const` por padrão, sempre que o valor não precisar mudar depois — isso deixa o código mais previsível e evita bugs de reatribuição acidental. Troque para `let` só quando souber que o valor vai mudar (contadores de loop, acumuladores, etc.).
2. `===` (comparação estrita) verifica valor E tipo, sem converter nada. `==` tenta converter os tipos antes de comparar, o que gera resultados inesperados — por exemplo, `"5" == 5` retorna `true` mesmo sendo uma string comparada a um número, o que geralmente não é a intenção do programador. `===` deixa o comportamento do código mais previsível.
3. Use `for` quando você já sabe (ou pode calcular) quantas vezes precisa repetir — por exemplo, percorrer os itens de uma lista com tamanho conhecido. Use `while` quando o número de repetições depende de uma condição que só é conhecida durante a execução — por exemplo, repetir "enquanto a bateria for maior que 0", sem saber de antemão quantas voltas isso vai levar.
4. Um parâmetro é uma "variável de entrada" declarada entre os parênteses da função — é o espaço reservado para o valor que será passado quando a função for chamada (ex.: `nota1`, `nota2` em `calcularMedia(nota1, nota2)`). O `return` define o valor que a função "devolve" para quem a chamou, encerrando a execução da função naquele ponto — sem `return`, a função executa suas instruções mas não entrega nenhum valor de volta (retorna `undefined`).

[« Voltar para a Atividade](#atividade)
