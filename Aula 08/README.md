# Aula 08 — Responsividade e Media Queries

**Módulo:** Fundamentos de Arquitetura Web, Protocolo HTTP, Conceitos de API REST, HTML, CSS e JavaScript
**Carga horária da aula:** 4 horas
**Professor(a):** @karizeviecelli

---

## 🎯 Objetivos da Aula

Ao final desta aula, você será capaz de:

- Explicar o conceito de design responsivo e por que ele é indispensável hoje.
- Escrever media queries com a sintaxe `@media (max-width: ...) { ... }`.
- Aplicar breakpoints comuns (480px, 768px, 1024px) para adaptar o layout.
- Usar imagens fluídas e unidades relativas para um design que se adapta.
- Aplicar a filosofia mobile first ao planejar um layout.
- Tornar o dashboard construído na Aula 7 responsivo para celular, tablet e desktop.

---

## 🖼️ Analogia Inicial: a casa inteligente que se reconfigura sozinha

Nas últimas aulas, desenhamos a planta baixa da casa (Grid) e organizamos os móveis em fila (Flexbox). Mas até agora, essa planta era **fixa** — pensada para um terreno de um tamanho só. E se o mesmo projeto precisasse funcionar tanto num apartamento pequeno quanto numa casa grande?

É exatamente isso que a responsividade resolve: uma **casa inteligente**, que percebe o tamanho do terreno disponível (a tela do usuário) e se reorganiza sozinha:

- Em um **terreno pequeno** (celular), os móveis se empilham, só o essencial fica visível, e os corredores ficam mais estreitos.
- Em um **terreno médio** (tablet), alguns móveis já cabem lado a lado.
- Em um **terreno grande** (desktop), a casa se espalha, com cômodos generosos e tudo visível ao mesmo tempo.

As **media queries** são as "regras de reforma automática" da casa: "se o terreno for menor que tanto, mude o layout desse jeito". E as unidades relativas e imagens fluídas garantem que os móveis e quadros também se ajustem proporcionalmente, sem quebrar ou vazar para fora das paredes.

---

## 📚 Conteúdo Teórico

### 1. Por que design responsivo?

```css
/* Sem responsividade: layout fixo, quebra em telas pequenas */
.dashboard {
  width: 1200px; /* em um celular de 375px, isso gera scroll horizontal */
}

/* Com responsividade: layout se adapta */
.dashboard {
  width: 100%;
  max-width: 1200px; /* nunca passa de 1200px, mas encolhe em telas menores */
}
```

Hoje, a maioria do tráfego de muitos sites vem de celulares. Um layout que só funciona bem em telas grandes exclui parte real dos usuários — por isso a responsividade deixou de ser "extra" e virou requisito básico de qualquer projeto profissional.

### 2. Sintaxe de media queries

```css
/* Estilo "padrão", aplicado em qualquer tamanho de tela */
.menu {
  display: flex;
  gap: 16px;
}

/* @media aplica um bloco de CSS SÓ quando a condição é verdadeira */
@media (max-width: 768px) {
  /* Este bloco só vale quando a tela tem 768px de largura OU MENOS */
  .menu {
    flex-direction: column; /* menu vira uma coluna em telas menores */
    gap: 8px;
  }
}

@media (min-width: 1024px) {
  /* Este bloco só vale quando a tela tem 1024px OU MAIS */
  .dashboard {
    grid-template-columns: 260px 1fr; /* menu lateral mais largo em telas grandes */
  }
}
```

`max-width` significa "até este tamanho" (útil para regras de telas pequenas). `min-width` significa "a partir deste tamanho" (útil para regras de telas grandes). As media queries são avaliadas em ordem: se duas regras conflitam, a que vem depois no arquivo CSS "vence" (respeitada a especificidade).

### 3. Breakpoints comuns

```css
/* Breakpoints são "pontos de quebra" — larguras em que o layout muda de comportamento */

/* Celulares (até ~480px) */
@media (max-width: 480px) { /* ... */ }

/* Tablets (até ~768px) */
@media (max-width: 768px) { /* ... */ }

/* Notebooks/desktops menores (até ~1024px) */
@media (max-width: 1024px) { /* ... */ }
```

Esses números não são "mágicos" nem obrigatórios — são convenções baseadas nos tamanhos de tela mais comuns do mercado. O importante é testar e ajustar os breakpoints onde o SEU layout realmente "quebra" (fica feio ou ilegível), e não decorar os valores de cabeça.

### 4. Imagens fluídas e unidades relativas

```css
/* Imagem fluída: nunca ultrapassa a largura do container pai */
img {
  max-width: 100%;
  height: auto; /* mantém a proporção original */
}

/* Unidades relativas se ajustam ao contexto, ao invés de um valor fixo */
.titulo {
  font-size: 2rem; /* relativo ao tamanho de fonte raiz do documento */
}

.secao {
  padding: 5vw; /* 5% da largura da viewport (tela) — cresce e encolhe com a tela */
}
```

Enquanto `px` é um valor fixo (sempre 16px, não importa a tela), unidades como `rem`, `%` e `vw`/`vh` são proporcionais — elas "respiram" junto com o tamanho da tela, sem precisar de uma media query para cada ajuste fino.

### 5. Mobile first: comece pequeno, cresça depois

```css
/* MOBILE FIRST: o estilo "padrão" (sem media query) já é pensado para celular */
.cards-stats {
  display: grid;
  grid-template-columns: 1fr; /* 1 coluna, ideal para telas pequenas */
  gap: 10px;
}

/* Conforme a tela cresce, adicionamos mais colunas */
@media (min-width: 768px) {
  .cards-stats {
    grid-template-columns: repeat(2, 1fr); /* tablets: 2 colunas */
  }
}

@media (min-width: 1024px) {
  .cards-stats {
    grid-template-columns: repeat(3, 1fr); /* desktop: 3 colunas */
  }
}
```

Na abordagem mobile first, você escreve o CSS "base" pensando no celular (a tela mais restrita), e usa `min-width` para ir ACRESCENTANDO complexidade conforme a tela cresce. É considerada uma boa prática porque força a pensar primeiro no essencial, e porque parte do princípio "menos CSS por padrão, mais CSS sob demanda" — geralmente resulta em código mais enxuto.

---

<a id="atividade"></a>
## 💻 Atividade Prática

**Duração sugerida:** 90 minutos
**Formato:** individual, com apoio em dupla

### Parte 1 — Tornando o dashboard responsivo

Retome o `dashboard.html` construído na Aula 7 e aplique:

1. Adicione a meta tag de viewport no `<head>` (se ainda não tiver): `<meta name="viewport" content="width=device-width, initial-scale=1.0">`.
2. Escreva uma media query com `max-width: 768px` que transforme o layout do dashboard: o menu lateral deve sair da lateral e ir para cima (ou virar uma barra horizontal), e o `grid-template-columns` deve virar uma coluna só.
3. Escreva uma segunda media query com `max-width: 480px` ajustando o tamanho de fontes e paddings para caber melhor em telas de celular.
4. Na sub-grade de cards de estatísticas, use a abordagem mobile first: 1 coluna por padrão, 2 colunas a partir de 768px, 3 colunas a partir de 1024px.
5. Se houver imagens na página, garanta que usem `max-width: 100%; height: auto;`.

### Parte 2 — Perguntas de fixação

Responda com suas palavras:

1. Qual a diferença entre `max-width` e `min-width` em uma media query? Em que situação cada uma é mais indicada?
2. Por que `px` é considerado um valor "fixo" e `rem`/`vw` são considerados "relativos"? Dê um exemplo prático da diferença.
3. O que significa a abordagem "mobile first", e por que ela costuma resultar em CSS mais enxuto?
4. Um breakpoint de 768px "quebrou" mal em um layout específico. É correto usar um valor diferente, como 820px? Justifique.

Quando terminar, confira sua resposta no gabarito.

[Ver Gabarito »](#gabarito)

---

<a id="gabarito"></a>
## ✅ Gabarito

### Parte 1 — Tornando o dashboard responsivo

```css
/* Mobile first: 1 coluna por padrão */
.cards-stats {
  display: grid;
  grid-template-columns: 1fr;  /* 4) */
  gap: 10px;
}

@media (min-width: 768px) {
  .cards-stats {
    grid-template-columns: repeat(2, 1fr); /* 4) tablets */
  }
}

@media (min-width: 1024px) {
  .cards-stats {
    grid-template-columns: repeat(3, 1fr); /* 4) desktop */
  }
}

/* Layout geral do dashboard */
@media (max-width: 768px) {           /* 2) */
  .dashboard {
    grid-template-columns: 1fr;        /* 2) uma coluna só */
    grid-template-areas:
      "cabecalho"
      "menu"
      "conteudo"
      "rodape";
  }
  .menu {
    display: flex;                      /* 2) menu vira barra horizontal */
    flex-direction: row;
    overflow-x: auto;
  }
}

@media (max-width: 480px) {            /* 3) */
  .cabecalho {
    font-size: 14px;
    padding: 0 10px;
  }
  .stat-card {
    padding: 10px;
  }
}

img {
  max-width: 100%;   /* 5) */
  height: auto;        /* 5) */
}
```

```html
<!-- 1) -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### Parte 2 — Perguntas de fixação

1. `max-width` aplica o bloco de CSS quando a tela tem AQUELE tamanho ou MENOS — útil para escrever regras pensando em telas pequenas a partir de um layout desktop. `min-width` aplica o bloco quando a tela tem AQUELE tamanho ou MAIS — útil na abordagem mobile first, para ir adicionando complexidade conforme a tela cresce.
2. `px` representa um valor absoluto: 16px sempre corresponde à mesma medida física de tela, não importa o tamanho do dispositivo. `rem` é relativo ao tamanho de fonte raiz do documento (normalmente ajustável pelo usuário), e `vw`/`vh` são relativos à largura/altura da viewport (tela) — ambos crescem e encolhem proporcionalmente sem precisar de uma media query específica. Exemplo: `padding: 5vw` sempre representa 5% da largura da tela, seja ela um celular ou um monitor grande.
3. Mobile first significa escrever o CSS "padrão" (sem media query) já pensando no celular — a tela mais restrita — e usar `min-width` para acrescentar regras conforme a tela cresce. Resulta em CSS mais enxuto porque o navegador de celular só precisa processar o CSS base (sem sobrescrever um monte de regras pensadas para desktop), e porque força o desenvolvedor a definir primeiro o que é essencial.
4. Sim, é correto. Os valores 480px/768px/1024px são convenções de mercado, não regras fixas do CSS. O critério certo é testar o layout redimensionando a janela (ou usando o modo responsivo do DevTools) e definir o breakpoint no ponto exato em que o design realmente fica ruim — mesmo que isso resulte em um valor "não convencional" como 820px.

[« Voltar para a Atividade](#atividade)
