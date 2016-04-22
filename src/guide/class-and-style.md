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

### Sintaxe de Array

Nós podemos passar um Array para a diretiva `v-bind:class` para aplicar uma lista de classes:

``` html
<div v-bind:class="[classA, classB]">
```
``` js
data: {
  classA: 'class-a',
  classB: 'class-b'
}
```

Que irá renderizar:

``` html
<div class="class-a class-b"></div>
```

Se você também gostaria de alternar uma classe na lista condicionalmente, você pode fazê-lo com uma expressão ternária:

``` html
<div v-bind:class="[classA, isB ? classB : '']">
```

Isso irá sempre aplicar `classA`, mas irá aplicar `classB` apenas quando `isB` for `true`.

## Ligando Estilos

### Sintaxe de Objeto

A sintaxe de Objeto para a diretiva `v-bind:style` é bem simples - é quase como CSS, exceto por ser um objeto do JavaScript. Você pode usar camelCase ou kebab-case para o nome das propriedades CSS:

``` html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
``` js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

Geralmente é uma boa ideia estilizar o objeto diretamente, deixando o template mais limpo: 

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

Novamente, a sintaxe de Objeto é geralmente utilizada em conjunto à computed properties que retornam objetos.

### Array Syntax

The Array syntax for `v-bind:style` allows you to apply multiple style objects to the same element:

``` html
<div v-bind:style="[styleObjectA, styleObjectB]">
```

### Auto-prefixing

When you use a CSS property that requires vendor prefixes in `v-bind:style`, for example `transform`, Vue.js will automatically detect and add appropriate prefixes to the applied styles.
