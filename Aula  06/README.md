# Aula 06 — Layout com Flexbox

**Módulo:** Fundamentos de Arquitetura Web, Protocolo HTTP, Conceitos de API REST, HTML, CSS e JavaScript
**Carga horária da aula:** 4 horas
**Professor(a):** @karizeviecelli

---

## 🎯 Objetivos da Aula

Ao final desta aula, você será capaz de:

- Ativar o Flexbox com `display: flex` e identificar o eixo principal e o eixo cruzado.
- Distribuir espaço entre itens com `justify-content` e alinhá-los com `align-items`.
- Controlar quebra de linha com `flex-wrap` e espaçamento com `gap`.
- Ajustar o comportamento individual de um item com `flex`, `flex-grow`, `flex-shrink`, `flex-basis`, `align-self` e `order`.
- Trocar a direção do fluxo com `flex-direction: column` e entender o que muda nos eixos.
- Construir, com Flexbox, uma barra de navegação, uma grade de cards e uma página de equipe.

---

## 🖼️ Analogia Inicial: os móveis da sala se ajustam ao espaço

Até agora, cada caixa (Aula 5) era um quadro fixo na parede, com tamanho e posição definidos manualmente. O Flexbox muda essa lógica: é como **organizar os móveis de uma sala com um decorador inteligente**.

Você diz ao decorador (o `display: flex` no elemento pai) qual é a "direção da sala" — os móveis ficam em fila (`row`) ou empilhados (`column`). Depois, você dá instruções gerais, e ele se vira para encaixar tudo:

- **`justify-content`:** "Encoste os móveis à esquerda", "distribua com espaço igual entre eles", "centralize tudo".
- **`align-items`:** "Alinhe todos pela base", "centralize na altura da parede".
- **`flex-wrap`:** "Se não couber tudo na fila, pode continuar numa segunda fileira".
- **`gap`:** "Deixe sempre esse tanto de espaço entre um móvel e outro".

E cada móvel também pode ter uma personalidade própria: um sofá pode "crescer" para ocupar o espaço sobrante (`flex-grow`), uma poltrona pode pedir para "encolher" se faltar espaço (`flex-shrink`), e você pode pedir para um quadro específico ficar centralizado mesmo que os outros estejam alinhados embaixo (`align-self`).

O Flexbox tira de você o trabalho manual de calcular posição — você descreve a intenção, e o navegador organiza.

---

## 📚 Conteúdo Teórico

> Todos os exemplos abaixo são completos: copie o HTML e o CSS para um arquivo, abra no navegador e experimente alterar os valores.

### 1. Ativando o Flexbox e os dois eixos

```html
<div class="container">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>
```

```css
/* O display:flex é aplicado no elemento PAI (o "container") */
.container {
  display: flex;
  flex-direction: row; /* padrão: itens em fila, da esquerda para a direita */
  /* outras opções: column, row-reverse, column-reverse */
  border: 2px dashed #94a3b8;
  padding: 8px;
}

.item {
  background-color: #3b82f6;
  color: white;
  padding: 16px 24px;
}
```

Assim que um elemento vira `display: flex`, todos os seus **filhos diretos** (não os netos) passam a ser "itens flexíveis" e se organizam automaticamente em fila (ou coluna), sem precisar de `float` ou de truques antigos de posicionamento.

Dois eixos importantes:

- **Eixo principal (main axis):** a direção da fila — horizontal se `flex-direction: row`.
- **Eixo cruzado (cross axis):** perpendicular ao principal — vertical se `flex-direction: row`.

`justify-content` trabalha no eixo principal. `align-items` trabalha no eixo cruzado. Essa é a maior fonte de confusão de quem está começando — decore essa regra.

**O que muda com `flex-direction: column`?** Os eixos giram 90°: o principal passa a ser vertical (então `justify-content` distribui os itens de cima para baixo) e o cruzado passa a ser horizontal (então `align-items` alinha à esquerda, ao centro ou à direita).

### 2. Distribuindo espaço: `justify-content` e `align-items`

```html
<nav class="navbar">
  <div class="logo">MeuSite</div>
  <ul class="nav-links">
    <li><a href="#">Início</a></li>
    <li><a href="#">Sobre</a></li>
    <li><a href="#">Contato</a></li>
  </ul>
</nav>
```

```css
.navbar {
  display: flex;

  /* JUSTIFY-CONTENT: distribui os itens no eixo PRINCIPAL (horizontal aqui) */
  justify-content: space-between; /* logo na ponta esquerda, links na ponta direita */
  /* outras opções: flex-start, flex-end, center, space-around, space-evenly */

  /* ALIGN-ITEMS: alinha os itens no eixo CRUZADO (vertical aqui) */
  align-items: center; /* centraliza verticalmente, mesmo com alturas diferentes */
  /* outras opções: flex-start, flex-end, stretch (padrão), baseline */

  padding: 16px 24px;
  background-color: #1e293b;
  color: white;
}

.logo {
  font-size: 22px;
  font-weight: bold;
}

.nav-links {
  display: flex;   /* a lista também vira um container flex: os <li> ficam lado a lado */
  gap: 20px;       /* espaço entre os links, sem margin em cada um */
  list-style: none;
  margin: 0;
  padding: 0;
}

.nav-links a {
  color: white;
  text-decoration: none;
}
```

Diferença entre os três `space-*`:

| Valor | Espaço entre itens | Espaço nas pontas |
|---|---|---|
| `space-between` | igual | nenhum |
| `space-around` | igual | metade do espaço entre itens |
| `space-evenly` | igual | igual ao espaço entre itens |

### 3. `flex-wrap` e `gap`: quando os móveis não cabem na fila

```html
<section class="grade-de-cards">
  <article class="card">Card 1</article>
  <article class="card">Card 2</article>
  <article class="card">Card 3</article>
  <article class="card">Card 4</article>
  <article class="card">Card 5</article>
  <article class="card">Card 6</article>
</section>
```

```css
.grade-de-cards {
  display: flex;
  flex-wrap: wrap; /* se não couber tudo numa linha, continua na próxima */
  gap: 16px;       /* espaço entre os itens, na horizontal E na vertical */
}

.card {
  flex: 1 1 200px; /* explicado na seção 4 */
  padding: 16px;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
}
```

Sem `flex-wrap: wrap` (o padrão é `nowrap`), os itens tentam encolher para caber todos na mesma linha — o que pode espremer o conteúdo. Com `wrap`, o excesso simplesmente "desce" para uma nova linha, como texto que quebra ao chegar na margem da página.

### 4. Propriedades dos itens: `flex`, `flex-grow`, `flex-shrink`, `flex-basis`

```css
.card {
  /* FLEX-GROW: quantas "partes" do espaço sobrando este item recebe */
  flex-grow: 1;

  /* FLEX-SHRINK: o quanto este item aceita encolher quando falta espaço */
  flex-shrink: 1; /* padrão — 0 impede o item de encolher */

  /* FLEX-BASIS: o tamanho de partida, antes de crescer ou encolher */
  flex-basis: 200px;

  /* Atalho equivalente às três linhas acima: flex: grow shrink basis */
  flex: 1 1 200px;
}
```

Pense em `flex-grow` como "quantas partes do espaço sobrando eu quero". Exemplo com dois itens:

```css
.texto  { flex: 2; } /* recebe 2 partes */
.imagem { flex: 1; } /* recebe 1 parte  */
/* resultado: o texto ocupa 2/3 da largura e a imagem 1/3 */
```

Atalhos que valem decorar:

- `flex: 1` equivale a `flex: 1 1 0` — todos com `flex: 1` ficam com a mesma largura.
- `flex: 0 0 250px` — largura fixa de 250px: não cresce nem encolhe.

### 5. `align-self` e `order`: exceções para um item específico

```html
<footer class="rodape">
  <div class="coluna">Sobre</div>
  <div class="coluna">Links</div>
  <div class="coluna coluna-contato">Contato</div>
</footer>
```

```css
.rodape {
  display: flex;
  align-items: flex-start; /* todas as colunas começam no topo */
  gap: 24px;
  min-height: 160px;
  background-color: #f1f5f9;
  padding: 24px;
}

.coluna {
  flex: 1;
}

.coluna-contato {
  /* ALIGN-SELF: sobrescreve o align-items só para ESTE item */
  align-self: flex-end; /* esta coluna vai para a base do rodapé */

  /* ORDER: muda a ordem visual sem mudar a ordem no HTML */
  order: -1; /* aparece primeiro (o padrão de todos é 0) */
}
```

Cuidado com `order`: ele muda apenas a posição visual. Leitores de tela e a navegação por `Tab` continuam seguindo a ordem do HTML.

### 6. `flex-direction: column` na prática

```html
<article class="membro">
  <div class="avatar">AB</div>
  <h3>Ana Beatriz</h3>
  <p>Desenvolvedora front-end</p>
  <a href="#" class="botao">Ver perfil</a>
</article>
```

```css
.membro {
  display: flex;
  flex-direction: column;       /* eixo principal agora é VERTICAL */
  align-items: center;          /* eixo cruzado é horizontal: centraliza na largura */
  justify-content: space-between; /* distribui na altura: botão vai para a base */
  gap: 8px;
  min-height: 220px;
  padding: 16px;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
}

.avatar {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background-color: #3b82f6;
  color: white;
  display: flex;               /* centralizar texto dentro do círculo: */
  justify-content: center;     /* horizontal */
  align-items: center;         /* vertical */
}
```

Repare que `align-items: center` aqui centraliza **horizontalmente** — porque, em `column`, o eixo cruzado é o horizontal. É exatamente a inversão descrita na seção 1.

---

<a id="atividade"></a>
## 💻 Atividade Prática

**Duração sugerida:** 120 minutos (60 min guiados + 60 min autônomos)
**Formato:** individual, com apoio em dupla
**⭐ Checkpoint 2 do projeto final**

### Parte 1 — Prática guiada: barra de navegação e grade de cards (`navbar.html`)

Reproduz o que foi construído na demonstração ao vivo.

1. Crie um `<nav class="navbar">` com uma `<div class="logo">` e uma `<ul class="nav-links">` com 3 links. Aplique `display: flex`, `justify-content: space-between` e `align-items: center` na navbar.
2. Faça a `.nav-links` também virar um container flex, com `gap: 20px` e sem marcadores de lista. Não use `margin` nos links.
3. Abaixo da navbar, crie uma `<section class="grade-de-cards">` com **pelo menos 6** `<article class="card">`. No container: `display: flex`, `flex-wrap: wrap`, `gap: 16px`.
4. Em cada card: `flex: 1 1 200px`, `padding` e `border`.
5. Escolha um card do meio do HTML, dê a ele a classe `card-destaque` e aplique `order: -1`.
6. Redimensione a janela e observe os cards crescerem, encolherem e quebrarem linha.

### Parte 2 — Desafio autônomo: página "Nossa Equipe" (`equipe.html`)

Agora sem passo a passo. Construa uma página com quatro seções, usando **somente Flexbox** (nada de `float`, `position` ou media queries):

1. **Hero (destaque)** — um bloco de texto (título + parágrafo) à esquerda e um bloco de imagem (pode ser uma `<div>` colorida) à direita. O texto deve ocupar **o dobro** da largura da imagem (`flex: 2` e `flex: 1`) e os dois blocos devem ficar centralizados verticalmente.
2. **Faixa de números** — 4 blocos (ex.: "12 projetos", "5 anos", "3 prêmios", "40 clientes") distribuídos com espaço igual entre eles **e nas pontas** (`space-evenly`).
3. **Membros da equipe** — pelo menos 4 cards com avatar, nome, cargo e um botão. Cada card deve usar `flex-direction: column`, com o conteúdo centralizado horizontalmente e o botão sempre na base do card. Os cards devem quebrar linha quando a janela diminuir.
4. **Rodapé** — 3 colunas de mesma largura. A coluna "Contato" deve estar **por último no HTML**, mas aparecer **primeiro** na tela (`order`), e ficar alinhada à **base** do rodapé enquanto as outras ficam no topo (`align-self`).

### Parte 3 — Perguntas de fixação

1. Qual a diferença entre o eixo principal (main axis) e o eixo cruzado (cross axis)? O que muda quando `flex-direction` é `column`?
2. Qual propriedade e valor você usaria para distribuir itens com espaço igual entre eles, mas SEM espaço nas pontas? E para ter espaço igual também nas pontas?
3. Para que serve o `flex-wrap: wrap`, e o que acontece por padrão sem ele?
4. Se um item tem `flex-grow: 3` e os outros três itens têm `flex-grow: 1` cada, como o espaço sobrando é dividido entre eles?
5. Num card com `flex-direction: column`, qual propriedade centraliza o conteúdo **horizontalmente**? Por quê?

Quando terminar, confira sua resposta no gabarito.

[Ver Gabarito »](#gabarito)

---

<a id="gabarito"></a>
## ✅ Gabarito

### Parte 1 — Barra de navegação e grade de cards

```html
<nav class="navbar">
  <div class="logo">MeuSite</div>
  <ul class="nav-links">
    <li><a href="#">Início</a></li>
    <li><a href="#">Sobre</a></li>
    <li><a href="#">Contato</a></li>
  </ul>
</nav>

<section class="grade-de-cards">
  <article class="card">Card 1</article>
  <article class="card">Card 2</article>
  <article class="card">Card 3</article>
  <article class="card card-destaque">Card 4 (destaque)</article>
  <article class="card">Card 5</article>
  <article class="card">Card 6</article>
</section>
```

```css
.navbar {
  display: flex;
  justify-content: space-between; /* 1) logo à esquerda, links à direita */
  align-items: center;             /* 1) tudo alinhado verticalmente ao centro */
  padding: 16px 24px;
  background-color: #1e293b;
  color: white;
}

.nav-links {
  display: flex;       /* 2) a lista também vira um container flex */
  gap: 20px;           /* 2) espaçamento entre os links, sem margin individual */
  list-style: none;
  margin: 0;
  padding: 0;
}

.nav-links a {
  color: white;
  text-decoration: none;
}

.grade-de-cards {
  display: flex;       /* 3) */
  flex-wrap: wrap;     /* 3) permite quebrar linha */
  gap: 16px;           /* 3) */
  padding: 16px;
}

.card {
  flex: 1 1 200px;     /* 4) cresce, encolhe, começa com 200px */
  padding: 16px;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
}

.card-destaque {
  order: -1;           /* 5) aparece primeiro, mesmo estando no meio do HTML */
  border-color: #3b82f6;
}
```

### Parte 2 — Página "Nossa Equipe"

```html
<section class="hero">
  <div class="hero-texto">
    <h1>Nossa Equipe</h1>
    <p>Pessoas que constroem produtos digitais com cuidado.</p>
  </div>
  <div class="hero-imagem"></div>
</section>

<section class="numeros">
  <div class="numero"><strong>12</strong> projetos</div>
  <div class="numero"><strong>5</strong> anos</div>
  <div class="numero"><strong>3</strong> prêmios</div>
  <div class="numero"><strong>40</strong> clientes</div>
</section>

<section class="equipe">
  <article class="membro">
    <div class="avatar">AB</div>
    <h3>Ana Beatriz</h3>
    <p>Front-end</p>
    <a href="#" class="botao">Ver perfil</a>
  </article>
  <!-- repita o <article> para os outros membros -->
</section>

<footer class="rodape">
  <div class="coluna"><h4>Sobre</h4><p>Quem somos.</p></div>
  <div class="coluna"><h4>Links</h4><p>Início · Projetos</p></div>
  <div class="coluna coluna-contato"><h4>Contato</h4><p>equipe@site.com</p></div>
</footer>
```

```css
/* 1) HERO: texto com o dobro da largura da imagem, centralizados verticalmente */
.hero {
  display: flex;
  align-items: center;
  gap: 32px;
  padding: 40px 24px;
}
.hero-texto  { flex: 2; }
.hero-imagem { flex: 1; min-height: 220px; background-color: #93c5fd; border-radius: 12px; }

/* 2) NÚMEROS: espaço igual entre os blocos E nas pontas */
.numeros {
  display: flex;
  justify-content: space-evenly;
  padding: 24px;
  background-color: #1e293b;
  color: white;
  text-align: center;
}
.numero strong { display: block; font-size: 28px; }

/* 3) EQUIPE: grade que quebra linha; cada card em coluna com botão na base */
.equipe {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
  padding: 24px;
}
.membro {
  flex: 1 1 200px;
  display: flex;
  flex-direction: column;         /* eixo principal vertical */
  align-items: center;            /* eixo cruzado horizontal: centraliza na largura */
  justify-content: space-between; /* distribui na altura: botão vai para a base */
  gap: 8px;
  min-height: 220px;
  padding: 16px;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
}
.avatar {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background-color: #3b82f6;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
}
.botao {
  padding: 8px 16px;
  background-color: #2563eb;
  color: white;
  border-radius: 6px;
  text-decoration: none;
}

/* 4) RODAPÉ: 3 colunas iguais; contato primeiro e alinhado à base */
.rodape {
  display: flex;
  align-items: flex-start;
  gap: 24px;
  min-height: 160px;
  padding: 24px;
  background-color: #f1f5f9;
}
.coluna { flex: 1; }
.coluna-contato {
  order: -1;            /* aparece primeiro, embora esteja por último no HTML */
  align-self: flex-end; /* só esta coluna vai para a base */
}
```

### Parte 3 — Perguntas de fixação

1. O eixo principal é a direção em que os itens fluem (definida por `flex-direction`); o eixo cruzado é perpendicular a ele. Com `flex-direction: row` (padrão), o principal é horizontal e o cruzado é vertical. Com `flex-direction: column`, isso se inverte: o principal passa a ser vertical (`justify-content` passa a controlar o alinhamento vertical) e o cruzado horizontal (`align-items` passa a controlar o alinhamento horizontal).
2. `justify-content: space-between` distribui espaço igual apenas entre os itens, sem espaço nas pontas (o primeiro encosta no início, o último no fim). `justify-content: space-evenly` distribui espaço igual entre todos os itens E também nas pontas.
3. `flex-wrap: wrap` permite que os itens continuem em uma nova linha quando não cabem todos na mesma fileira. Por padrão (`flex-wrap: nowrap`), os itens tentam encolher (respeitando `flex-shrink`) para caber todos na mesma linha, o que pode espremer o conteúdo.
4. O espaço sobrando é dividido em partes proporcionais aos valores de `flex-grow`: neste caso há 3+1+1+1 = 6 "partes" no total. O item com `flex-grow: 3` recebe 3/6 (metade) do espaço extra, e cada um dos outros três recebe 1/6.
5. `align-items: center`. Em `flex-direction: column`, o eixo principal é vertical e o cruzado é horizontal — e `align-items` sempre atua no eixo cruzado. Por isso, o que era "centralizar verticalmente" em `row` vira "centralizar horizontalmente" em `column`.

[« Voltar para a Atividade](#atividade)
