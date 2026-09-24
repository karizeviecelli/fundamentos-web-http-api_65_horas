# Aula 05 — Box Model e Espaçamento

**Módulo:** Fundamentos de Arquitetura Web, Protocolo HTTP, Conceitos de API REST, HTML, CSS e JavaScript
**Carga horária da aula:** 4 horas
**Professor(a):** @karizeviecelli

---

## 🎯 Objetivos da Aula

Ao final desta aula, você será capaz de:

- Explicar o conceito de box model e suas quatro camadas.
- Aplicar `padding`, `border` e `margin` com controle de espaçamento.
- Usar `box-sizing: border-box` para simplificar o cálculo de tamanhos.
- Diferenciar e aplicar corretamente `display: block`, `inline`, `inline-block` e `none`.

---

## 🖼️ Analogia Inicial: toda caixa é um quadro na parede

Todo elemento HTML, no fundo, é uma **caixa retangular** — mesmo que pareça um texto simples ou um botão redondo. E toda caixa se parece com um **quadro pendurado na parede**, com quatro camadas, de dentro para fora:

- **Content (conteúdo):** a própria pintura, dentro da moldura.
- **Padding (preenchimento):** o passe-partout — aquele espaço branco entre a pintura e a moldura.
- **Border (borda):** a moldura em si, que envolve tudo.
- **Margin (margem):** o espaço vazio na parede entre este quadro e o quadro vizinho.

Entender essas quatro camadas é a base para qualquer layout em CSS — sem isso, "alinhar as coisas" vira adivinhação.

---

## 📚 Conteúdo Teórico

### 1. As quatro camadas do box model

```css
.quadro {
  /* CONTENT: a pintura em si — definida por width e height */
  width: 200px;
  height: 150px;

  /* PADDING: passe-partout — espaço interno, entre o conteúdo e a borda */
  padding: 16px;

  /* BORDER: a moldura — contorno visível ao redor do padding */
  border: 2px solid #1e293b;

  /* MARGIN: espaço na parede — distância até o quadro vizinho */
  margin: 20px;
}
```

Uma pegadinha importante: por padrão, `width` e `height` definem apenas o tamanho do **content**. O `padding` e a `border` são somados por fora, deixando a caixa maior do que você esperava — é aí que entra o próximo tópico.

### 2. `box-sizing: border-box`

```css
/* Sem box-sizing (padrão do navegador) */
.caixa-padrao {
  width: 200px;
  padding: 20px;
  border: 5px solid black;
  /* Largura REAL na tela: 200 + 20+20 (padding) + 5+5 (border) = 250px */
}

/* Com box-sizing: border-box */
.caixa-border-box {
  box-sizing: border-box;
  width: 200px;
  padding: 20px;
  border: 5px solid black;
  /* Largura REAL na tela: exatamente 200px */
  /* padding e border passam a ser DESCONTADOS de dentro do width */
}
```

Com `box-sizing: border-box`, o `width` volta a significar "o tamanho total da moldura na parede" — muito mais intuitivo. Por isso é comum ver, no início de um CSS profissional:

```css
* {
  box-sizing: border-box;
}
```

### 3. `display`: como a caixa se comporta na fila

```css
/* BLOCK: ocupa a linha inteira, sempre começa em uma nova linha */
/* Exemplos padrão: <div>, <p>, <h1>, <section> */
.bloco {
  display: block;
}

/* INLINE: ocupa só o espaço do conteúdo, fica lado a lado com outros */
/* Exemplos padrão: <span>, <a>, <strong>, <em> */
/* IMPORTANTE: width e height NÃO funcionam em elementos inline */
.linha {
  display: inline;
}

/* INLINE-BLOCK: o melhor dos dois mundos */
/* Fica lado a lado (como inline), mas aceita width, height, padding e margin */
.linha-com-tamanho {
  display: inline-block;
  width: 120px;
  padding: 8px;
}

/* NONE: remove o elemento completamente — nem o espaço dele fica reservado */
.escondido {
  display: none;
}
```

Pense em `block` como um quadro grande que ocupa a parede inteira, obrigando o próximo quadro a ir para outra parede. `inline` é um adesivo pequeno colado ao lado de outro, sem moldura própria. `inline-block` é um quadro pequeno, mas que ainda é um quadro — com moldura e tamanho definidos. `none` é o quadro que foi guardado no depósito: nem a marca dele fica na parede.

---

<a id="atividade"></a>
## 💻 Atividade Prática

**Duração sugerida:** 90 minutos
**Formato:** individual, com apoio em dupla

### Parte 1 — Criando cards com espaçamento correto

No `estilos.css` da sua página (ou em um novo arquivo `cards.html` + `cards.css`), crie **3 cards** (podem representar produtos, hobbies ou qualquer conteúdo à sua escolha), aplicando obrigatoriamente:

1. Cada card deve ter `width`, `padding`, `border` e `margin` definidos.
2. Aplique `box-sizing: border-box` (idealmente com o seletor universal `*`).
3. Os 3 cards devem ficar lado a lado, usando `display: inline-block` (ou outra técnica equivalente).
4. Adicione um elemento com `display: none` na página (ex.: uma mensagem de aviso escondida) e comente no CSS por que ele não aparece.
5. Use pelo menos um elemento com `display: inline` (ex.: um `<span>` destacando uma palavra) e compare visualmente com os cards em `inline-block`.

### Parte 2 — Perguntas de fixação

Responda com suas palavras:

1. Descreva, na ordem correta (de dentro para fora), as quatro camadas do box model.
2. O que muda no cálculo do tamanho de um elemento quando você aplica `box-sizing: border-box`?
3. Qual a diferença prática entre `display: inline` e `display: inline-block`? Por que `width` não funciona no primeiro?
4. Qual a diferença entre `display: none` e simplesmente deixar um elemento sem conteúdo (vazio)?

Quando terminar, confira sua resposta no gabarito.

[Ver Gabarito »](#gabarito)

---

<a id="gabarito"></a>
## ✅ Gabarito

### Parte 1 — Criando cards com espaçamento correto

Exemplo de resolução (o conteúdo pode variar, mas a estrutura deve seguir este padrão):

```css
* {
  box-sizing: border-box; /* 2) padding e border não somam ao width */
}

.card {
  width: 180px;
  padding: 16px;          /* 1) espaço interno */
  border: 1px solid #cbd5e1; /* 1) moldura */
  margin: 10px;            /* 1) espaço entre os cards */
  display: inline-block;   /* 3) cards ficam lado a lado */
  background-color: #fff;
}

.aviso-oculto {
  display: none; /* 4) o elemento nem reserva espaço na página */
}

.destaque-inline {
  display: inline; /* 5) fica na mesma linha do texto ao redor */
  color: #dc2626;
}
```

```html
<div class="card">Card 1</div>
<div class="card">Card 2</div>
<div class="card">Card 3</div>

<p class="aviso-oculto">Este aviso está temporariamente desativado.</p>

<p>Confira nossa <span class="destaque-inline">promoção especial</span> desta semana.</p>
```

### Parte 2 — Perguntas de fixação

1. De dentro para fora: **content** (o conteúdo em si) → **padding** (espaço interno, entre o conteúdo e a borda) → **border** (a moldura, contorno visível) → **margin** (espaço externo, até o próximo elemento).
2. Sem `border-box`, o `width`/`height` definem só o content, e `padding`+`border` são somados por fora — a caixa fica maior que o valor definido. Com `box-sizing: border-box`, o `width`/`height` passam a representar o tamanho TOTAL da caixa (incluindo padding e border), que são descontados de dentro.
3. `inline` ocupa apenas o espaço do próprio conteúdo, fica na mesma linha de outros elementos inline, e ignora `width`/`height` — o navegador ajusta o tamanho automaticamente ao conteúdo. `inline-block` também fica lado a lado com outros elementos, mas se comporta como uma caixa "de verdade": aceita `width`, `height`, `padding` e `margin` definidos manualmente.
4. `display: none` remove o elemento completamente da renderização — ele não aparece e nem reserva espaço, como se não existisse na página. Um elemento vazio (sem conteúdo), por outro lado, ainda existe no layout: dependendo do CSS aplicado (padding, border, altura mínima), ele pode continuar ocupando espaço visível, mesmo sem nenhum texto dentro.

[« Voltar para a Atividade](#atividade)
