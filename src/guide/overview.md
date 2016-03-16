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

O segundo exemplo mostra que não apenas podemos ligar textos da DOM a dados, mas também podemos ligar **estruturas** da DOM a dados. O Vue ainda tem um sistema de efeitos de transições que automaticamente aplica efeitos de transição quando elementos são inseridos ou removidos pelo Vue.

Existem outras diretivas, cada uma com sua funcionalidade especial. Por exemplo, a diretiva `v-for` permite mostrar itens em um Array, e a `v-bind` permite ligar atributos HTML. Vamos falar sobre isso no futuro.

## Sistema de Componentes

O Sistema de Componentes é outro conceito importante no Vue.js porque é uma abstração que nos permite criar aplicações de larga-escala compostas por pequenos, reutilizáveis componentes. Se pensarmos nisso, praticamente qualquer aplicação pode ser abstraída em em uma árvore de componentes:

![Árvore de Componentes](/images/components.png)

Na verdade, uma aplicação de larga escala feita com Vue.js iria geralmente ser da forma da direita — uma árvore de componentes. Vamos falar sobre componentes depois nesse guia, mas aqui está um exemplo (imaginário) de como seria a estrutura de uma aplicação usando componentes.

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

Você pode ter percebido que os componentes do Vue.js são muito parecidos com **Elementos Customizados**, que é parte dos [Web Components](http://www.w3.org/wiki/WebComponents/). Na verdade, a sintaxe de componentes do Vue.js é levemente baseada neles. Por exemplo, os componentes do Vue implementam a [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) e o atributo especial `is`. No entanto, existem algumas diferenças:

1. A especificação de Web Components ainda é um projeto em progresso, e não é suportado nativamente por todos os browsers. Em comparação, os componentes do Vue não precisam de nenhum polyfill e funcionam consistentemente nos browsers suportados (a partir do Internet Explorer 9). Quando necessário, os componentes do Vue podem estar dentro de um elemento customizado nativo.

2. Os componentes do Vue.js fornecem funcionalidades importantes que não são disponíveis usando elementos customizados, a mais notável sendo o fluxo de dados entre componentes, comunicação de eventos customizada e troca dinâmica de copmonentes com efeitos de transição.

O sistema de componentes é a fundação para criar aplicativos de larga escala com o Vue. Além disso, o ecossistema do Vue fornece ferramentas e bibliotecas que podem ser usadas juntas para criar um sistema mais parecido com um Framework.
