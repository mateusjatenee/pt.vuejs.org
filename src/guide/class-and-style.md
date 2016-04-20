---
title: Class and Style Bindings
type: guide
order: 6
---

Um uso comum de data binding é para manipular as classes e estilos de um elemento. Já que eles são ambos atributos, nós podemos usar a diretiva `v-bind` para isso: nós apenas precisamos calcular uma string final com nossas expressões. No entanto, lidar com concatenação de strings é chato e propenso a errors. Por conta disso, o Vue.js fornece melhorias quando a diretiva `v-bind` for usada para `class` e `style`. Em adição às Strings, as expressões também podem ser processadas como objetos ou arrays.

## Vinculando Classes HTML

<p class="tip">Apesar de você poder usar interpolações com chaves duplas, como `{% raw %}class="{{ className}}"{% endraw %}` para ligar à classe, não é recomendado misturar esse estilo com `v-bind:class`. Use apenas um ou outro!</p>

### Object Syntax

Nós podemos passar um objeto para a diretiva `v-bind:class` para trocar as classes dinamicamente. Note que a diretiva `v-bind:class` pode co-existir com o atributo `class`:

``` html
<div class="static" v-bind:class="{ 'class-a': isA, 'class-b': isB }"></div>
```
``` js
data: {
  isA: true,
  isB: false
}
```

Que vai renderizar:

``` html
<div class="static class-a"></div>
```

Quando `isA` e `isB` mudarem, a lista de classes será atualizada de acordo. Por exemplo, se `isB` tornar-se `true`, a lista de classes irá transformar-se em `"static class-a class-b"`.

E você pode ligar diretamente a um objeto também:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  classObject: {
    'class-a': true,
    'class-b': false
  }
}
```

Isso irá renderizar o mesmo resultado. Como você talvez tenha percebido, nós também podemos à uma [computed property](computed.html) que retorna um objeto. Isso é um padrão comum e poderoso.

### Array Syntax

We can pass an Array to `v-bind:class` to apply a list of classes:

``` html
<div v-bind:class="[classA, classB]">
```
``` js
data: {
  classA: 'class-a',
  classB: 'class-b'
}
```

Which will render:

``` html
<div class="class-a class-b"></div>
```

If you would like to also toggle a class in the list conditionally, you can do it with a ternary expression:

``` html
<div v-bind:class="[classA, isB ? classB : '']">
```

This will always apply `classA`, but will only apply `classB` when `isB` is `true`.

## Binding Inline Styles

### Object Syntax

The Object syntax for `v-bind:style` is pretty straightforward - it looks almost like CSS, except it's a JavaScript object. You can use either camelCase or kebab-case for the CSS property names:

``` html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
``` js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

It is often a good idea to bind to a style object directly so that the template is cleaner:

``` html
<div v-bind:style="styleObject"></div>
```
``` js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

Again, the Object syntax is often used in conjunction with computed properties that return Objects.

### Array Syntax

The Array syntax for `v-bind:style` allows you to apply multiple style objects to the same element:

``` html
<div v-bind:style="[styleObjectA, styleObjectB]">
```

### Auto-prefixing

When you use a CSS property that requires vendor prefixes in `v-bind:style`, for example `transform`, Vue.js will automatically detect and add appropriate prefixes to the applied styles.
