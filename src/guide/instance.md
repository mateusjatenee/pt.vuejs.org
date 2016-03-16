---
title: A instância do Vue
type: guide
order: 3
---

## Construtor


Toda aplicação que usa o Vue.js inicia quando é criada uma **instância do Vue** usando o construtor `Vue`:

``` js
var vm = new Vue({
  // options
})
```

Uma instância do Vue é essencialmente um **ViewModel** — como definido no [padrão MVVM](https://en.wikipedia.org/wiki/Model_View_ViewModel) — por isso o nome da variável `vm` que você verá ao longo da documentação.

Quando você cria uma instância do Vue, você precisa passar um **objeto de opções** que pode conter opções para dados, template, o elemento ao qual o Vue será vinculado, métodos, ciclo de vida dos callbacks e mais. A lista completa de opções está na referência da API.

O construtor `Vue` pode ser estendido para criar `construtores de componentes` reutilizáveis com opções pré-definidas:

``` js
var MyComponent = Vue.extend({
  // opções de extensão
})

// todas as instâncias de `MyComponent` são
// criados com as opções pré-definidas
var myComponentInstance = new MyComponent()
```

Apesar de você poder criar instâncias estendidas imperativamente, na maioria dos casos você estará registrando um construtor de um componente como um elemento customizado e compondo em templates declarativamente. Vamos falar mais sobre o sistema de componentes depois. Por enquanto, tudo o que você precisa saber é que todos os componentes do Vue.js são essencialmente instâncias estendidas do Vue.

## Propriedades e Métodos

Cada instância do Vue **intercepta** todas as propriedades encontradas no objeto `data`:

``` js
var data = { a: 1 }
var vm = new Vue({
  data: data
})

vm.a === data.a // -> verdadeiro (true)

// mudar a propriedade também afeta os dados originais
vm.a = 2
data.a // -> 2

// ... e vice versa
data.a = 3
vm.a // -> 3
```

Deve-se notar que apenas essas propriedades interceptadas são **reativas**. Se você adicionar uma nova propriedade à instância depois que ela foi criada, nenhum update na view vai acontecer. Vamos falar sobre o sistema de reatividade mais tarde.

Além das propriedades de dados, instâncias do Vue expoem vários métodos e propriedades úteis. Essas propriedades e métodos tem o prefixo `$` para diferenciá-las de propriedades de dados interceptadas. Por exemplo:

``` js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // -> true
vm.$el === document.getElementById('example') // -> true

// $watch is an instance method
vm.$watch('a', function (newVal, oldVal) {
  // this callback will be called when `vm.a` changes
})
```

Consult the [API reference](/api) for the full list of instance properties and methods.

## Instance Lifecycle

Each Vue instance goes through a series of initialization steps when it is created - for example, it needs to set up data observation, compile the template, and create the necessary data bindings. Along the way, it will also invoke some **lifecycle hooks**, which give us the opportunity to execute custom logic. For example, the `created` hook is called after the instance is created:

``` js
var vm = new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` points to the vm instance
    console.log('a is: ' + this.a)
  }
})
// -> "a is: 1"
```

There are also other hooks which will be called at different stages of the instance's lifecycle, for example `compiled`, `ready` and `destroyed`. All lifecycle hooks are called with their `this` context pointing to the Vue instance invoking it. Some users may have been wondering where the concept of "controllers" lives in the Vue.js world, and the answer is: there are no controllers in Vue.js. Your custom logic for a component would be split among these lifecycle hooks.

## Lifecycle Diagram

Below is a diagram for the instance lifecycle. You don't need to fully understand everything going on right now, but this diagram will be helpful in the future.

![Lifecycle](/images/lifecycle.png)
