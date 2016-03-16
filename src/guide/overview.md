---
title: Resumo
type: guide
order: 2
---

Vue.js é uma biblioteca para a criação de interfaces web interativas. O objetivo do Vue.js é fornecer os benefícios de **data binding reativo** e **componentes** em uma API que é o mais simples possível.

O Vue.js em si não é um framework — ele é focado apenas na camada de apresentação. Ele é, portanto, muito fácil de aprender e integrar com outras bibliotecas ou projetos existentes. Por outro lado, quando usado em combinação com as ferramentas certas e bibliotecas de suporte, é completamente possível usar o Vue.js em SPAs (Single-Page Applications).

Se você é um desenvolvedor experiente e quer saber como o Vue.js se compara em relação à outras bibliotecas/frameworks, dê uma olhada na página [Comparação com outros frameworks](comparison.html); se você é mais interessado em como o Vue.js lida com aplicações de larga escala, dê uma olhada na seção [Criando aplicações de larga escala](application.html).

## Data Binding Reativo

Em seu núcleo, o Vue.js é um sistema de data-binding reativo que faz manter seus dados e a DOM sincronizados super simples. Ao usar jQuery para manipular a DOM manualmente, o código que escrevemos é — geralmente — imperativo, repetitivo e tende a ter erros. O Vue.js abraça o conceito de **data-driven view**. Simplificando: siginifica que usamos uma sintaxe especial nos nossos templates HTML e "ligamos" a DOM com os onssos dados. Assim que as ligações forem criadas, a DOM irá se manter em sincronia com os dados. Quando você modificar os dados, a DOM irá atualizar-se. Como resultado, a maioria da lógica da nossa aplicação é agora manipular dados, ao invés de ficar trabalhando com atualizações da DOM. Como resultado, fica mais fácil escrever nosso código, simples de ler e mais fácil de manter.


![MVVM](/images/mvvm.png)

O exemplo mais simples possível:

``` html
<!-- essa é a nossa view -->
<div id="exemplo-1">
  Olá {{ name }}!
</div>
```

``` js
// esse é o nosso modelo
var exampleData = {
  name: 'Vue.js'
}

// cria uma instância do Vue
// que faz a conexão entre a View e o Modelo
var exampleVM = new Vue({
  el: '#exemplo-1',
  data: exampleData
})
```

Resultado:
{% raw %}
<div id="exemplo-1" class="demo">Hello {{ name }}!</div>
<script>
var exampleData = {
  name: 'Vue.js'
}
var exampleVM = new Vue({
  el: '#exemplo-1',
  data: exampleData
})
</script>
{% endraw %}


Parece que estamos apenas renderizando um template, mas o Vue.js fez bastante coisa debaixo dos panos. Os dados e a DOM agora estão conectados, e agora tudo é **reativo**. Como sabemos disso? Abra o Console do seu browser e modifique `exampleData.name`. Você deve ver que o exemplo renderizado também muda.

Note que não tivemos que escrever nenhum código de manipulação da DOM: o template HTML, aprimorado com os bindings (conexões), é uma forma de mapear os dados, que não são nada mais do que objetos em Javascript. A nossa view é completamente data-driven (orientada a dados). <!-- essa parte ficou bizarra -->

Vamos dar uma olhada no segundo exemplo:

``` html
<div id="example-2">
  <p v-if="greeting">Olá!</p>
</div>
```

``` js
var exampleVM2 = new Vue({
  el: '#example-2',
  data: {
    greeting: true
  }
})
```

{% raw %}
<div id="example-2" class="demo">
  <span v-if="greeting">Olá!</span>
</div>
<script>
var exampleVM2 = new Vue({
  el: '#example-2',
  data: {
    greeting: true
  }
})
</script>
{% endraw %}

Aqui encontramos uma coisa nova. O atributo `v-if` que você está vendo é uma **diretiva**. Diretivas têm o prefixo `v-` para indicar que são atributos especiais providos pelo Vue, e, assim como você provavlemente imaginou, eles aplicam um comportamente especial à DOM renderizada. Vá em frente e defina `exampleVM2.greeting` para `false` no seu Console. Você deve ver a mensagem "Hello" sumir.

This second example demonstrates that not only can we bind DOM text to the data, we can also bind the **structure** of the DOM to the data. Moreover, Vue.js also provides a powerful transition effect system that can automatically apply transition effects when elements are inserted/removed by Vue.

There are quite a few other directives, each with its own special functionality. For example the `v-for` directive for displaying items in an Array, or the `v-bind` directive for binding HTML attributes. We will discuss the full data-binding syntax with more details later.

## Component System

The Component System is another important concept in Vue.js, because it's an abstraction that allows us to build large-scale applications composed of small, self-contained, and often reusable components. If we think about it, almost any type of application interface can be abstracted into a tree of components:

![Component Tree](/images/components.png)

In fact, a typical large application built with Vue.js would form exactly what is on the right - a tree of components. We will talk a lot more about components later in the guide, but here's an (imaginary) example of what an app's template would look like with components:

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

You may have noticed that Vue.js components are very similar to **Custom Elements**, which is part of the [Web Components Spec](http://www.w3.org/wiki/WebComponents/). In fact, Vue.js' component syntax is loosely modeled after the spec. For example, Vue components implement the [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) and the `is` special attribute. However, there are a few key differences:

1. The Web Components Spec is still very much a work in progress, and is not natively implemented in every browser. In comparison, Vue.js components don't require any polyfills and works consistently in all supported browsers (IE9 and above). When needed, Vue.js components can also be wrapped inside a native custom element.

2. Vue.js components provide important features that are not available in plain custom elements, most notably cross-component data flow, custom event communication and dynamic component switching with transition effects.

The component system is the foundation for building large apps with Vue.js. In addition, the Vue.js ecosystem also provides advanced tooling and various supporting libraries that can be put together to create a more "framework" like system.
