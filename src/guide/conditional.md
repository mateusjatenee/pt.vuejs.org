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

Another option for conditionally displaying an element is the `v-show` directive. The usage is largely the same:

``` html
<h1 v-show="ok">Hello!</h1>
```

The difference is that an element with `v-show` will always be rendered and remain in the DOM; `v-show` simply toggles the `display` CSS property of the element.

Note that `v-show` doesn't support the `<template>` syntax.

## v-else

You can use the `v-else` directive to indicate an "else block" for `v-if` or `v-show`:

``` html
<div v-if="Math.random() > 0.5">
  Sorry
</div>
<div v-else>
  Not sorry
</div>
```

The `v-else` element must immediately follow the `v-if` or `v-show` element - otherwise it will not be recognized.

## v-if vs. v-show

When a `v-if` block is toggled, Vue.js will have to perform a partial compilation/teardown process, because the template content inside `v-if` can also contain data bindings or child components. `v-if` is "real" conditional rendering because it ensures that event listeners and child components inside the conditional block are properly destroyed and re-created during toggles.

`v-if` is also **lazy**: if the condition is false on initial render, it will not do anything - partial compilation won't start until the condition becomes true for the first time (and the compilation is subsequently cached).

In comparison, `v-show` is much simpler - the element is always compiled and preserved, with just simple CSS-based toggling.

Generally speaking, `v-if` has higher toggle costs while `v-show` has higher initial render costs. So prefer `v-show` if you need to toggle something very often, and prefer `v-if` if the condition is unlikely to change at runtime.
