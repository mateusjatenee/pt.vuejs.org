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

vm.$data === data // -> verdadeiro
vm.$el === document.getElementById('example') // -> verdadeiro

// $watch é um método da instância
vm.$watch('a', function (newVal, oldVal) {
  // esse callback será chamado quando `vm.a` mudar
})
```

Consulte a [referência da API](/api) para a lista completa de propriedades e métodos da instância.

## Ciclo de Vida da Instância

Cada instância do Vue passa por uma série de etapas de inicialização quando é criada - por exemplo, ela precisa definir dados de observação, compilar o template, e criar os data bindings necessários. Durante esse procedimento, ela também invoca alguns **métodos do ciclo de vida**, que nos dam a oportunidade de executar lógica customizada. Por exemplo, o método `created` é chamado depois da instância ser criada.

``` js
var vm = new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` aponta à uma instância do Vue
    console.log('a is: ' + this.a)
  }
})
// -> "a is: 1"
```

Existem também outros métodos que serão chamados em diferentes estágios do ciclo de vida da instância — `compiled`, `ready` e `destroyed`, por exemplo. Todos os métodos do ciclo de vida são chamados com o contexto `this` apontando para a instância do Vue que o invoca.  Alguns usuários talvez tenham se questionado onde está o conceito de "controllers" no mundo do Vue, e a resposta é: não existem controllers no Vue. A sua lógica customizada para um componente seria dividida nesses métodos do ciclo de vida.

## Lifecycle Diagram

Below is a diagram for the instance lifecycle. You don't need to fully understand everything going on right now, but this diagram will be helpful in the future.

![Lifecycle](/images/lifecycle.png)
