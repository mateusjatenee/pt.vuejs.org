---
title: Computed Properties
type: guide
order: 5
---

Expressões dentro de templates são convenientes, mas elas são destinadas apenas à operações simples. Templates devem descrever a estrutura da sua view. Colocar muita lógica nos seus templates pode torná-los "sujos" e difíceis de manter. É por isso que o Vue.js limita as expressões de ligação à apenas uma expressão. Para qualquer lógica que precise de mais de uma expressão, você deve usar uma **computed property**.

### Exemplo Básico

``` html
<div id="example">
  a={{ a }}, b={{ b }}
</div>
```

``` js
var vm = new Vue({
  el: '#example',
  data: {
    a: 1
  },
  computed: {
    // um getter computado
    b: function () {
      // `this` referencia a instância da vm
      return this.a + 1
    }
  }
})
```

Resultado:

{% raw %}
<div id="example" class="demo">
  a={{ a }}, b={{ b }}
</div>
<script>
var vm = new Vue({
  el: '#example',
  data: {
    a: 1
  },
  computed: {
    b: function () {
      return this.a + 1
    }
  }
})
</script>
{% endraw %}

Aqui nós declaramos uma propriedade computada `b`. A função que fornecemos será usada como um getter para a propriedade `vm.b`:

``` js
console.log(vm.b) // -> 2
vm.a = 2
console.log(vm.b) // -> 3
```

Você pode abrir o console e brincar com o exemplo. O valor de `vm.b` será sempre dependente do valor de `vm.a`.

Você pode ligar computed properties em templates assim como uma propriedade normal. O Vue está ciente de que `vm.b` depende de `vm.a`, então ele irá atualizar quaisquer valores que dependem de `vm.b` quando `vm.a` mudar. E a melhor parte é que criamos essa relação de dependência declarativamente: o getter da computed property é pura e não tem nenhum colateral, o que o torna fácil de testar e compreender.

### Computed Property vs. $watch

A API do Vue.js oferece um método chamado `$watch` que lhe permite observar mudanças de dados numa instância do Vue. Quando você tem alguns dados que precisam mudar baseados em outros dados, é tentador usar o `$watch` - especialmente se você vem do AngularJS. Entretanto, geralmente é melhor voc%e usar uma computed property, ao invés de usar um callback imperativo do `$watch`. Considere esse exemplo:  

``` html
<div id="demo">{{fullName}}</div>
```

``` js
var vm = new Vue({
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  }
})

vm.$watch('firstName', function (val) {
  this.fullName = val + ' ' + this.lastName
})

vm.$watch('lastName', function (val) {
  this.fullName = this.firstName + ' ' + val
})
```

O código acima é imperativo e repetitivo. Compare-o com uma versão usando computed properties:

``` js
var vm = new Vue({
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

Bem melhor, não?

### Computed Setter

Computed properties are by default getter-only, but you can also provide a setter when you need it:

``` js
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

Now when you call `vm.fullName = 'John Doe'`, the setter will be invoked and `vm.firstName` and `vm.lastName` will be updated accordingly.

The technical details behind how computed properties are updated are [discussed in another section](reactivity.html#Inside_Computed_Properties) dedicated to the reactivity system.
