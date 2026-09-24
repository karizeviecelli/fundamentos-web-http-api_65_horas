# Aula 04 — Introdução ao CSS

**Módulo:** Fundamentos de Arquitetura Web, Protocolo HTTP, Conceitos de API REST, HTML, CSS e JavaScript
**Carga horária da aula:** 4 horas
**Professor(a):** @karizeviecelli

---

## 🎯 Objetivos da Aula

Ao final desta aula, você será capaz de:

- Explicar o que é CSS e qual seu papel na construção de uma página web.
- Aplicar CSS de três formas: inline, interno e externo.
- Usar os principais seletores CSS (tag, classe, id, universal, descendente, filho).
- Aplicar propriedades básicas de cor, fundo e tipografia.
- Entender e escolher a unidade de medida correta para cada situação.

---

## 🎨 Analogia Inicial: chegou a hora da pintura e da decoração

Nas últimas três aulas, construímos a obra bruta da casa: paredes, cômodos, placas nas portas, fichas de atendimento. A casa já funciona — mas está tudo cinza, sem cor, sem estilo.

Hoje entra o **pintor e decorador**: o **CSS**.

- O HTML já disse **o que existe** (uma parede, uma porta, uma janela).
- O CSS decide **como cada coisa aparece**: a cor da parede, o tipo de piso, o tamanho da porta.
- E existem diferentes formas de dar instrução ao pintor:
  - **Inline:** um bilhete colado direto no objeto ("pinte esta parede de azul").
  - **Interno:** uma lista de instruções afixada na entrada do cômodo, valendo só para aquele cômodo.
  - **Externo:** um manual de decoração único, guardado numa pasta à parte, que pode ser reaproveitado em várias casas (páginas) diferentes.

---

## 📚 Conteúdo Teórico

### 1. Formas de incluir CSS

```html
<!-- 1) INLINE: estilo escrito direto na tag, via atributo style -->
<!-- Só vale para AQUELE elemento específico. Use com moderação. -->
<p style="color: blue; font-size: 18px;">Texto azul e maior.</p>

<!-- 2) INTERNO: bloco <style> dentro do <head> -->
<!-- Vale para toda a página onde está escrito. -->
<head>
  <style>
    p {
      color: blue;
    }
  </style>
</head>

<!-- 3) EXTERNO: arquivo .css separado, ligado via <link> -->
<!-- A forma recomendada: reutilizável em várias páginas. -->
<head>
  <link rel="stylesheet" href="estilos.css">
</head>
```

Assim como o bilhete colado é rápido mas difícil de manter, o **inline** deve ser evitado em projetos reais. O **externo** é o "manual de decoração" que qualquer página do site pode consultar — é o padrão profissional.

### 2. Seletores CSS

```css
/* Seletor de TAG: atinge todos os elementos daquele tipo */
p {
  color: #333;
}

/* Seletor de CLASSE (.): atinge todo elemento com aquela classe */
/* Pode ser reaproveitado em vários elementos diferentes */
.destaque {
  color: red;
  font-weight: bold;
}

/* Seletor de ID (#): atinge um único elemento, o de id exato */
/* Cada id deve existir apenas UMA vez na página */
#titulo-principal {
  font-size: 32px;
}

/* Seletor UNIVERSAL (*): atinge TODOS os elementos da página */
* {
  margin: 0;
  padding: 0;
}

/* Seletor DESCENDENTE: atinge um elemento que está DENTRO de outro */
/* Aqui: todo <a> que estiver dentro de um <nav> */
nav a {
  text-decoration: none;
}

/* Seletor FILHO (>): atinge apenas o filho DIRETO */
/* Aqui: apenas os <li> que são filhos diretos de <ul> */
ul > li {
  list-style: none;
}
```

Pense na classe como um "crachá" que várias pessoas (elementos) podem usar ao mesmo tempo. O id é como um "CPF" — único, exclusivo daquela pessoa.

### 3. Propriedades básicas

```css
p {
  color: #1e293b;              /* cor do texto */
  background-color: #f8fafc;   /* cor de fundo */
  font-family: Arial, sans-serif; /* fonte usada */
  font-size: 16px;             /* tamanho do texto */
  text-align: center;          /* alinhamento: left, center, right, justify */
}
```

Cada propriedade responde a uma pergunta: `color` responde "de que cor é o texto?", `background-color` responde "de que cor é o fundo?", `font-family` responde "com que 'letra de forma' o texto é escrito?".

### 4. Unidades de medida

| Unidade | O que é | Quando usar |
|---|---|---|
| `px` | Pixels — medida fixa, absoluta | Quando você quer um tamanho exato, que não muda |
| `%` | Porcentagem do elemento pai | Layouts flexíveis, que se adaptam ao espaço disponível |
| `em` | Relativo ao tamanho de fonte do elemento pai | Espaçamentos que devem crescer junto com o texto |
| `rem` | Relativo ao tamanho de fonte da página (`<html>`) | Tamanhos de fonte consistentes em toda a página |
| `vh` | Porcentagem da altura da janela do navegador | Elementos que ocupam parte da tela (ex.: altura total) |
| `vw` | Porcentagem da largura da janela do navegador | Elementos que acompanham a largura da tela |

`px` é como medir com uma régua fixa. `%`, `vh` e `vw` são como medir "em relação ao tamanho da sala" — mudam se a sala mudar de tamanho.

---

<a id="atividade"></a>
## 💻 Atividade Prática

**Duração sugerida:** 90 minutos
**Formato:** individual, com apoio em dupla

### Parte 1 — Estilizando sua página

Retome o arquivo `perfil.html` (ou `landing.html`) das aulas anteriores e crie um arquivo `estilos.css` na mesma pasta. Depois:

1. Conecte o `estilos.css` ao HTML usando `<link rel="stylesheet" href="estilos.css">` dentro do `<head>`.
2. No CSS, use um seletor de **tag** para definir `font-family` e `color` para todo o `body`.
3. Use um seletor de **id** para estilizar o `<h1>` com um `font-size` maior (em `rem`).
4. Crie uma **classe** chamada `.destaque` (com `color` e `font-weight`) e aplique-a a pelo menos um trecho de texto no HTML.
5. Use um seletor **descendente** para estilizar os links (`<a>`) apenas dentro de uma seção específica (ex.: `nav a` ou `footer a`).
6. Defina um `background-color` para o `body` usando um código hexadecimal.
7. Use pelo menos 3 unidades diferentes (`px`, `%`, `rem`, `em`, `vh` ou `vw`) em propriedades diferentes.

### Parte 2 — Perguntas de fixação

Responda com suas palavras:

1. Qual a diferença entre CSS inline, interno e externo? Qual é o mais recomendado em projetos reais e por quê?
2. Qual a diferença entre um seletor de classe e um seletor de id? Quando usar cada um?
3. O que significa `nav a { }`? Isso é diferente de `nav > a { }`? Explique.
4. Por que `rem` costuma ser preferido a `px` para definir tamanhos de fonte em projetos maiores?

Quando terminar, confira sua resposta no gabarito.

[Ver Gabarito »](#gabarito)

---

<a id="gabarito"></a>
## ✅ Gabarito

### Parte 1 — Estilizando sua página

Exemplo de resolução (o conteúdo pode variar, mas a estrutura deve seguir este padrão):

```html
<!-- No <head> do perfil.html -->
<link rel="stylesheet" href="estilos.css">
```

```css
/* estilos.css */

/* 2) Seletor de tag: aplica a TODO o body */
body {
  font-family: Arial, sans-serif;
  color: #1e293b;
  background-color: #f8fafc; /* 6) cor de fundo em hexadecimal */
}

/* 3) Seletor de id: aplica só ao elemento com id="titulo" */
#titulo {
  font-size: 2rem; /* 7) unidade rem */
}

/* 4) Classe reutilizável */
.destaque {
  color: #dc2626;
  font-weight: bold;
}

/* 5) Seletor descendente: só os <a> dentro de <nav> */
nav a {
  text-decoration: none;
  color: #1d4ed8;
  padding: 4px 8px; /* 7) unidade px */
}

/* 7) Outra unidade usada: largura em porcentagem */
main {
  width: 90%;
  margin: 0 auto;
}
```

```html
<!-- No corpo do HTML -->
<h1 id="titulo">Karize</h1>
<p>Estudante <span class="destaque">apaixonada por tecnologia</span>.</p>
```

### Parte 2 — Perguntas de fixação

1. **Inline** é escrito direto na tag (`style="..."`), vale só para aquele elemento e é difícil de manter. **Interno** fica em um bloco `<style>` no `<head>`, vale para a página inteira, mas não é reaproveitável em outras páginas. **Externo** fica em um arquivo `.css` separado, ligado por `<link>`, e é o mais recomendado por ser reutilizável, organizado e fácil de manter em projetos com várias páginas.
2. A **classe** (`.nome`) pode ser aplicada a vários elementos ao mesmo tempo — é como um "crachá" compartilhado. O **id** (`#nome`) deve ser único na página — é como um "CPF", exclusivo de um único elemento. Use classe para estilos reutilizáveis e id para um elemento específico e único.
3. `nav a` é um seletor **descendente**: atinge qualquer `<a>` que esteja dentro de um `<nav>`, não importa quantos níveis de profundidade. `nav > a` é um seletor **filho direto**: atinge apenas os `<a>` que são filhos imediatos do `<nav>`, ignorando `<a>` que estejam, por exemplo, dentro de uma `<ul>` dentro do `<nav>`.
4. `rem` é relativo ao tamanho de fonte definido no `<html>` (a raiz da página), então todos os tamanhos em `rem` crescem ou diminuem juntos e de forma proporcional se esse valor raiz mudar — o que facilita ajustes de acessibilidade e responsividade. `px` é um valor fixo, que não se adapta a essas mudanças.

[« Voltar para a Atividade](#atividade)
