---
title: Conditional Rendering
type: guide
order: 7
---

## v-if

Em templates como Handlebars, nós poderíamos escrever um bloco condicional assim:

``` html
<!-- Handlebars template -->
{{#if ok}}
  <h1>Yes</h1>
{{/if}}
```

No Vue.js, nós usamos a diretiva `v-if` para atingir o mesmo resultado:

``` html
<h1 v-if="ok">Yes</h1>
```

Também é possível adicionar um bloco "else" com `v-else`:

``` html
<h1 v-if="ok">Yes</h1>
<h1 v-else>No</h1>
```

## Template v-if

Por conta de `v-if` ser uma diretiva, ela tem de ser anexada a um único elemento. Mas e se quisermos adicionar mais de 
um elemento? Nesse caso, podemos usar `v-if` num elemento `<template>`, que serve como um "wrapper" invisível. O template renderizado não irá incluir o elemento `<template`.

``` html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

## v-show

Outra opção para exibir elementos condicionalmente é a diretiva `v-show`. O uso é geralmente o mesmo:

``` html
<h1 v-show="ok">Hello!</h1>
```

A diferença é que um elemento com `v-show` sempre será renderizando e existirá na DOM; `v-show` simplesmente altera a propriedade CSS `display` do elemento.

Note que `v-show` não suporta a síntaxe de `<template>`.

## v-else

Você pode usar a diretiva `v-else` para indicar um "bloco else" para `v-if` ou `v-show`:

``` html
<div v-if="Math.random() > 0.5">
  Sorry
</div>
<div v-else>
  Not sorry
</div>
```

O elemento `v-else` deve estar logo após o elemento com `v-if` ou `v-show` - se não, ele não será reconhecido.

## v-if vs. v-show

Quando um bloco `v-if` é alternado, o Vue.js precisará fazer um processo de compilação parcial, porque o conteúdo dentro de um bloco `v-if` pode conter data bindings e componentes-filhos. A diretiva `v-if` realmente baseia-se em renderização condicional porque ela garante que event listeners e componentes-filhos dentro do bloco condicional são destruídos e recriados durante alternâncias.

A diretiva `v-if` é, também, **lazy**: se uma condição é falsa na renderização inicial, ela não irá fazer nada — a compilação não irá começar até que a condição torne-se verdadeira pela primeira vez.

Em comparação, a diretiva `v-show` é muito mais simples - o elemento sempre é compilado e preservado, com apenas uma propriedade CSS básica.

Geralmente, a diretiva `v-if` tem um maior custo de processamento enquanto `v-show` tem um custo de renderização inicial. Portanto, prefira usar `v-show` se você precisa alternar a visiblidade de algo todo o tempo, e prefira `v-if` se a condição raramente vai mudar.
