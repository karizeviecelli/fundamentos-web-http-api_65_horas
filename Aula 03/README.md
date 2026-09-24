# Aula 03 — HTML Semântico, Formulários e Multimídia

**Módulo:** Fundamentos de Arquitetura Web, Protocolo HTTP, Conceitos de API REST, HTML, CSS e JavaScript
**Carga horária da aula:** 4 horas
**Professor(a):** @karizeviecelli

---

## 🎯 Objetivos da Aula

Ao final desta aula, você será capaz de:

- Usar elementos semânticos para organizar as áreas de uma página.
- Criar tabelas para exibir dados estruturados.
- Construir formulários funcionais com os principais campos de entrada.
- Incorporar vídeo, áudio e conteúdo externo em uma página.

---

## 🚪 Analogia Inicial: colocando placas nas portas da casa

Na Aula 02 construímos a obra bruta da casa: paredes, cômodos, portas e janelas. Só que, até agora, nenhum cômodo tem placa dizendo o que ele é — para o navegador, tudo é só uma "sala genérica" (`<div>`).

Hoje colocamos as placas:

- `<header>` → a **fachada e a entrada** da casa
- `<nav>` → o **corredor de acesso**, com placas indicando "cozinha", "quarto", "sala"
- `<main>` → a **sala principal**, onde a vida da casa realmente acontece
- `<section>` → **cômodos** dentro da sala principal (sala de jantar, sala de estar)
- `<article>` → um **quadro autônomo na parede**: faz sentido mesmo se pendurado em outra casa
- `<aside>` → um **anexo lateral**, relacionado à casa mas não essencial (um jardim de inverno)
- `<footer>` → o **quintal dos fundos**, com informações de serviço (endereço, contatos)

Com as placas certas, qualquer visitante — inclusive leitores de tela e mecanismos de busca — entende a casa sem precisar adivinhar.

---

## 📚 Conteúdo Teórico

### 1. Elementos semânticos

```html
<body>

  <header>
    <!-- Cabeçalho do site: geralmente logo, nome do site e menu -->
    <h1>Meu Portfólio</h1>
  </header>

  <nav>
    <!-- Área de navegação: links principais do site -->
    <a href="#projetos">Projetos</a>
    <a href="#contato">Contato</a>
  </nav>

  <main>
    <!-- Conteúdo PRINCIPAL da página. Só existe um <main> por página -->

    <section id="projetos">
      <!-- Uma seção temática dentro do conteúdo principal -->
      <h2>Projetos</h2>

      <article>
        <!-- Um conteúdo independente, que faria sentido sozinho -->
        <h3>Projeto 1</h3>
        <p>Descrição do projeto...</p>
      </article>
    </section>

    <aside>
      <!-- Conteúdo relacionado, mas secundário (barra lateral) -->
      <p>Curiosidade: este site foi feito só com HTML e CSS.</p>
    </aside>

  </main>

  <footer>
    <!-- Rodapé: informações de encerramento, contato, direitos autorais -->
    <p>&copy; 2026 Karize Viecelli</p>
  </footer>

</body>
```

Antes dessas tags existirem, tudo era `<div>`. O problema é que `<div>` não diz **nada** sobre o que está dentro — é como uma caixa sem etiqueta. Tags semânticas comunicam significado, e isso ajuda acessibilidade (leitores de tela) e SEO (buscadores como o Google).

### 2. Tabelas

```html
<table>
  <thead>
    <!-- Cabeçalho da tabela -->
    <tr>
      <th>Aluno</th>
      <th>Nota</th>
    </tr>
  </thead>
  <tbody>
    <!-- Corpo da tabela, com os dados de fato -->
    <tr>
      <td>Ana</td>
      <td>9.0</td>
    </tr>
    <tr>
      <td>Bruno</td>
      <td>7.5</td>
    </tr>
  </tbody>
</table>
```

Pense na tabela como um **quadro de horários pregado na parede**: `<table>` é o quadro inteiro, `<tr>` (table row) é cada linha, `<th>` (table header) é o cabeçalho de cada coluna, e `<td>` (table data) é cada célula de dado.

### 3. Formulários

```html
<form action="/enviar-contato" method="POST">
  <!--
    action = para onde os dados serão enviados (a URL do servidor)
    method = qual método HTTP será usado (GET ou POST)
  -->

  <label for="nome">Nome:</label>
  <input type="text" id="nome" name="nome" required>
  <!--
    id    = identificador único, conecta o <input> ao <label>
    name  = nome do campo, usado quando os dados chegam no servidor
    required = campo obrigatório
  -->

  <label for="email">E-mail:</label>
  <input type="email" id="email" name="email" required>

  <label for="mensagem">Mensagem:</label>
  <textarea id="mensagem" name="mensagem" rows="4"></textarea>
  <!-- textarea: campo de texto com várias linhas -->

  <label for="assunto">Assunto:</label>
  <select id="assunto" name="assunto">
    <!-- select: lista de opções (menu suspenso) -->
    <option value="duvida">Dúvida</option>
    <option value="sugestao">Sugestão</option>
    <option value="elogio">Elogio</option>
  </select>

  <button type="submit">Enviar</button>
  <!-- type="submit": envia o formulário quando clicado -->

</form>
```

Um formulário é como a **ficha de atendimento de uma recepção**: `action` é para qual balcão a ficha vai, `method` é como ela é entregue (à mão, pelo correio), e cada `<input>`/`<textarea>`/`<select>` é um campo em branco que a pessoa preenche. O `name` de cada campo é o rótulo que o atendente usa para separar as informações depois.

### 4. Multimídia

```html
<video src="apresentacao.mp4" controls width="480">
  <!-- controls: mostra botões de play, pausa e volume -->
  Seu navegador não suporta vídeo.
</video>

<audio src="podcast.mp3" controls>
  Seu navegador não suporta áudio.
</audio>

<iframe src="https://www.google.com/maps" width="600" height="450">
  <!-- iframe: incorpora OUTRA página dentro da sua página -->
</iframe>
```

O `<iframe>` é como uma **janela para outra casa**: você está dentro do seu site, mas vendo, através dela, o conteúdo de outro endereço (um mapa, um vídeo do YouTube etc.).

---

<a id="atividade"></a>
## 💻 Atividade Prática

**Duração sugerida:** 90 minutos
**Formato:** individual, com apoio em dupla

### Parte 1 — Landing page semântica

Crie um arquivo `landing.html` estruturando uma página fictícia de um evento (uma feira de tecnologia, por exemplo), contendo obrigatoriamente:

1. `<header>` com o nome do evento.
2. `<nav>` com pelo menos 3 links de navegação interna (usando `#`).
3. `<main>` contendo:
   - Uma `<section>` com uma `<table>` mostrando a programação (horário e atividade), com no mínimo 3 linhas.
   - Um `<article>` descrevendo um dos palestrantes.
4. `<aside>` com uma informação extra (ex.: "vagas limitadas").
5. Um `<form>` de inscrição no evento, com campos de nome, e-mail, um `<select>` de turno (manhã/tarde/noite) e um botão de envio.
6. `<footer>` com informações de contato.

### Parte 2 — Perguntas de fixação

Responda com suas palavras:

1. Qual a diferença entre `<section>` e `<article>`? Dê um exemplo de cada.
2. Por que usar `<label for="...">` junto com o `<input>` é importante, além de deixar o formulário mais organizado visualmente?
3. O que acontece, na prática, quando o usuário clica em um botão `type="submit"` dentro de um `<form>`?
4. Em que situação você usaria um `<iframe>` em vez de simplesmente colocar um link para outro site?

Quando terminar, confira sua resposta no gabarito.

[Ver Gabarito »](#gabarito)

---

<a id="gabarito"></a>
## ✅ Gabarito

### Parte 1 — Landing page semântica

Exemplo de resolução (o conteúdo pode variar, mas a estrutura deve seguir este padrão):

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Feira de Tecnologia 2026</title>
</head>
<body>

  <header>
    <h1>Feira de Tecnologia 2026</h1>
  </header>

  <nav>
    <a href="#programacao">Programação</a>
    <a href="#palestrantes">Palestrantes</a>
    <a href="#inscricao">Inscrição</a>
  </nav>

  <main>

    <section id="programacao">
      <h2>Programação</h2>
      <table>
        <thead>
          <tr>
            <th>Horário</th>
            <th>Atividade</th>
          </tr>
        </thead>
        <tbody>
          <tr><td>09h00</td><td>Abertura</td></tr>
          <tr><td>10h00</td><td>Palestra: Introdução à Web</td></tr>
          <tr><td>14h00</td><td>Oficina de HTML e CSS</td></tr>
        </tbody>
      </table>
    </section>

    <section id="palestrantes">
      <h2>Palestrantes</h2>
      <article>
        <h3>Karize Viecelli</h3>
        <p>Instrutora de Desenvolvimento Web, especialista em HTML, CSS e JavaScript.</p>
      </article>
    </section>

    <aside>
      <p>⚠️ Vagas limitadas! Garanta a sua inscrição.</p>
    </aside>

    <section id="inscricao">
      <h2>Inscrição</h2>
      <form action="/inscrever" method="POST">
        <label for="nome">Nome:</label>
        <input type="text" id="nome" name="nome" required>

        <label for="email">E-mail:</label>
        <input type="email" id="email" name="email" required>

        <label for="turno">Turno:</label>
        <select id="turno" name="turno">
          <option value="manha">Manhã</option>
          <option value="tarde">Tarde</option>
          <option value="noite">Noite</option>
        </select>

        <button type="submit">Inscrever-se</button>
      </form>
    </section>

  </main>

  <footer>
    <p>Contato: contato@feiratech.com &copy; 2026</p>
  </footer>

</body>
</html>
```

### Parte 2 — Perguntas de fixação

1. `<section>` agrupa uma parte temática do conteúdo, geralmente dependente do contexto da página (ex.: a seção "Programação" só faz sentido dentro da página do evento). `<article>` representa um conteúdo independente, que faria sentido mesmo publicado sozinho em outro lugar (ex.: a descrição de um palestrante, um post de blog).
2. O `<label for="id">` conecta o rótulo ao campo pelo atributo `id`. Isso permite que, ao clicar no texto do rótulo, o campo correspondente seja focado automaticamente — e é essencial para leitores de tela anunciarem corretamente qual campo o usuário está preenchendo (acessibilidade).
3. O botão `type="submit"` dispara o envio do formulário: o navegador reúne os valores de todos os campos (usando o `name` de cada um) e os envia para a URL definida em `action`, usando o método definido em `method`.
4. O `<iframe>` é usado quando você quer **exibir o conteúdo de outra página dentro da sua**, sem que o usuário saia do seu site — por exemplo, embutir um mapa do Google Maps, um vídeo do YouTube ou um formulário externo. Um link simples, em vez disso, leva o usuário para fora do seu site.

[« Voltar para a Atividade](#atividade)
