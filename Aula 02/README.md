# Aula 02 — HTML: Estrutura Básica

**Módulo:** Fundamentos de Arquitetura Web, Protocolo HTTP, Conceitos de API REST, HTML, CSS e JavaScript
**Carga horária da aula:** 4 horas
**Professor(a):** @karizeviecelli

---

## 🎯 Objetivos da Aula

Ao final desta aula, você será capaz de:

- Explicar o que é HTML e qual seu papel na construção de uma página web.
- Montar a estrutura básica de um documento HTML.
- Usar as tags essenciais de título, parágrafo e quebra de linha.
- Aplicar ênfase e destaque em textos.
- Criar hiperligações (links) e entender caminhos absolutos e relativos.
- Inserir imagens com os atributos corretos.
- Criar listas ordenadas e não ordenadas.

---

## 🏗️ Analogia Inicial: HTML é a obra bruta de uma casa

Antes de uma casa ficar bonita — com pintura, móveis e decoração — ela precisa da **obra bruta**: fundação, paredes, cômodos, portas e janelas no lugar certo. Sem isso, não existe onde pintar nem o que decorar.

Na Web, o **HTML** é essa obra bruta:

- Define **onde** fica cada cômodo (seções, títulos, parágrafos).
- Define **o que existe** na casa (uma porta é um link, uma janela é uma imagem).
- Ainda não tem cor nem estilo — isso vem depois, com o **CSS** (a pintura e a decoração).
- Ainda não tem luz automática nem interruptores inteligentes — isso vem com o **JavaScript** (a parte elétrica e a automação).

Hoje construímos só a obra bruta. E está tudo certo ela parecer "feia" por enquanto — esse é o papel do HTML.

---

## 📚 Conteúdo Teórico

### 1. Estrutura básica de um documento HTML

Todo documento HTML segue um "esqueleto" padrão:

```html
<!DOCTYPE html>
<!-- Avisa ao navegador: "isso aqui é um documento HTML5" -->

<html lang="pt-BR">
<!-- A "casa inteira". lang="pt-BR" diz que o conteúdo está em português do Brasil -->

  <head>
    <!-- Informações que o USUÁRIO NÃO VÊ na página, mas o navegador usa -->
    <meta charset="UTF-8">
    <!-- Garante que acentos e caracteres especiais (ã, ç, é) apareçam corretamente -->
    <title>Minha Primeira Página</title>
    <!-- Título exibido na aba do navegador -->
  </head>

  <body>
    <!-- Tudo que aparece VISUALMENTE na página fica aqui dentro -->
    <h1>Olá, mundo!</h1>
  </body>

</html>
```

Pense em `<html>` como a casa inteira, `<head>` como a "planta e os documentos da obra" (ninguém vê morando, mas são essenciais) e `<body>` como os cômodos habitáveis, onde a vida realmente acontece.

### 2. Títulos e parágrafos

```html
<h1>Título Principal</h1>       <!-- O maior e mais importante. Só um por página, geralmente -->
<h2>Subtítulo</h2>              <!-- Divide seções dentro do conteúdo -->
<h3>Um nível abaixo do h2</h3>  <!-- E assim vai, até h6 -->

<p>
  Este é um parágrafo de texto comum.
  <!-- Tudo dentro de <p> é tratado como um bloco único de texto -->
</p>

<p>
  Primeira linha do endereço<br>
  <!-- <br> força uma quebra de linha SEM criar um novo parágrafo -->
  Segunda linha do endereço
</p>
```

`<h1>` a `<h6>` funcionam como os títulos de um sumário: quanto menor o número, mais importante o título.

### 3. Ênfase e destaque

```html
<p>Isso é <strong>muito importante</strong> de lembrar.</p>
<!-- <strong> = importância forte (o navegador costuma deixar em negrito) -->

<p>Isso é uma <em>observação com ênfase</em> no texto.</p>
<!-- <em> = ênfase (o navegador costuma deixar em itálico) -->
```

`<strong>` e `<em>` não são só sobre aparência (negrito/itálico) — eles comunicam **significado**: "isso é importante" e "isso tem ênfase", inclusive para leitores de tela usados por pessoas com deficiência visual.

### 4. Links e caminhos

```html
<!-- Link absoluto: endereço completo, aponta para OUTRO site -->
<a href="https://www.google.com">Ir para o Google</a>

<!-- Link relativo: aponta para um arquivo DENTRO do mesmo projeto -->
<a href="sobre.html">Sobre nós</a>

<!-- Link relativo, subindo uma pasta -->
<a href="../index.html">Voltar para a Home</a>
```

- **Caminho absoluto:** o "endereço completo do país, cidade e rua" — funciona de qualquer lugar.
- **Caminho relativo:** dizer "é a casa ao lado" — só faz sentido a partir de onde você já está.

### 5. Imagens

```html
<img src="foto-perfil.jpg" alt="Foto de perfil do usuário" width="200" height="200">
<!--
  src    = de onde vem a imagem (caminho do arquivo ou URL)
  alt    = texto alternativo, exibido se a imagem não carregar
           e lido por leitores de tela (acessibilidade)
  width  = largura em pixels
  height = altura em pixels
-->
```

O atributo `alt` não é opcional na prática: imagine descrever uma foto por telefone para alguém que não pode vê-la — é exatamente essa a função dele.

### 6. Listas

```html
<!-- Lista não ordenada: quando a ORDEM não importa -->
<ul>
  <li>Leite</li>
  <li>Ovos</li>
  <li>Pão</li>
</ul>

<!-- Lista ordenada: quando a ORDEM importa (passo a passo) -->
<ol>
  <li>Ligue o forno</li>
  <li>Misture os ingredientes</li>
  <li>Asse por 30 minutos</li>
</ol>
```

Use `<ul>` para uma lista de compras (a ordem não muda o resultado) e `<ol>` para uma receita (trocar a ordem dos passos muda tudo).

---

<a id="atividade"></a>
## 💻 Atividade Prática

**Duração sugerida:** 90 minutos
**Formato:** individual, com apoio em dupla

### Parte 1 — Construindo sua primeira página

Crie um arquivo chamado `perfil.html` e monte uma página sobre você (ou um personagem fictício), contendo obrigatoriamente:

1. A estrutura básica (`<!DOCTYPE>`, `<html>`, `<head>` com `<title>`, `<body>`).
2. Um `<h1>` com seu nome.
3. Um `<p>` de apresentação, com pelo menos uma palavra destacada em `<strong>` e outra em `<em>`.
4. Uma imagem (`<img>`) com `src`, `alt`, `width` e `height` preenchidos corretamente.
5. Uma lista não ordenada (`<ul>`) com 3 hobbies ou interesses.
6. Uma lista ordenada (`<ol>`) com 3 passos de uma rotina sua (ex.: rotina de estudos).
7. Um link absoluto para um site externo de sua preferência.
8. Um link relativo para um arquivo chamado `contato.html` (não precisa existir de verdade ainda).

### Parte 2 — Perguntas de fixação

Responda com suas palavras:

1. Qual a diferença entre o que fica dentro de `<head>` e o que fica dentro de `<body>`?
2. Por que o atributo `alt` da tag `<img>` é importante mesmo quando a imagem carrega normalmente?
3. Quando você usaria `<ol>` em vez de `<ul>`? Dê um exemplo próprio.
4. Qual a diferença entre um link absoluto e um link relativo?

Quando terminar, confira sua resposta no gabarito.

[Ver Gabarito »](#gabarito)

---

<a id="gabarito"></a>
## ✅ Gabarito

### Parte 1 — Construindo sua primeira página

Exemplo de resolução (o conteúdo pode variar, mas a estrutura deve seguir este padrão):

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Perfil - Karize</title>
</head>
<body>

  <h1>Karize</h1>

  <p>
    Estudante do curso Técnico em Desenvolvimento de Sistemas.
    <strong>Apaixonada por tecnologia</strong> e sempre em busca de
    <em>novos desafios</em> para aprender.
  </p>

  <img src="foto.jpg" alt="Foto de perfil de Karize" width="200" height="200">

  <h2>Hobbies</h2>
  <ul>
    <li>Programação</li>
    <li>Leitura</li>
    <li>Fotografia</li>
  </ul>

  <h2>Rotina de estudos</h2>
  <ol>
    <li>Revisar o conteúdo da aula anterior</li>
    <li>Praticar exercícios de código</li>
    <li>Ler documentação sobre o assunto do dia</li>
  </ol>

  <p>
    <a href="https://developer.mozilla.org/pt-BR/">Consultar a documentação MDN</a>
    <!-- link absoluto: aponta para outro site -->
  </p>

  <p>
    <a href="contato.html">Ir para a página de contato</a>
    <!-- link relativo: aponta para um arquivo do mesmo projeto -->
  </p>

</body>
</html>
```

### Parte 2 — Perguntas de fixação

1. Dentro de `<head>` fica o que o navegador precisa saber, mas o usuário não vê diretamente na página (título da aba, codificação de caracteres, links para arquivos CSS, etc.). Dentro de `<body>` fica tudo o que aparece visualmente na tela.
2. O `alt` é lido por leitores de tela (acessibilidade), é exibido se a imagem falhar ao carregar, e ajuda motores de busca a entender o conteúdo da imagem — mesmo quando tudo funciona normalmente, ele continua tendo esse papel de apoio.
3. Use `<ol>` quando a ordem dos itens importa para o resultado — por exemplo, o passo a passo de uma receita de bolo, onde inverter a ordem dos passos muda o resultado final. Use `<ul>` quando a ordem é apenas organizacional, como uma lista de ingredientes.
4. Um **link absoluto** contém o endereço completo (`https://...`) e aponta para qualquer lugar da internet, inclusive outros sites. Um **link relativo** aponta para um arquivo dentro do mesmo projeto, usando o caminho a partir do arquivo atual (ex.: `contato.html` ou `../index.html`).

[« Voltar para a Atividade](#atividade)
