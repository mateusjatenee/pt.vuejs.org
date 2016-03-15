---
title: Começando
type: guide
order: 1
---

Vamos começar com um pequeno tour das funcionalidades de data binding do Vue. Se você está interessado num resumo de "alto nível" antes, dê uma olhada nesse [post](http://blog.evanyou.me/2015/10/25/vuejs-re-introduction/).

A forma mais fácil de testar o Vue.js é usando o [Exemplo de Hello World no JSFIddle](https://jsfiddle.net/yyx990803/okv0rgrk/). Sinta-se livre para abri-lo em outra aba e segui-lo enquanto vemos alguns exemplos básicos. Se você prefere baixar ou instalar a partir de um gerenciador de pacotes, cheque a [página de instalação](/guide/installation.html).

### Hello World

``` html
<div id="app">
  {{ message }}
</div>
```
``` js
new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js!'
  }
})
```
{% raw %}
<div id="app" class="demo">
  {{ message }}
</div>
<script>
new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js!'
  }
})
</script>
{% endraw %}

### Two-way Binding

``` html
<div id="app">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```
``` js
new Vue({
  el: '#app',
  data: {
    message: 'Olá Vue.js!'
  }
})
```
{% raw %}
<div id="app2" class="demo">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
<script>
new Vue({
  el: '#app2',
  data: {
    message: 'Olá Vue.js!'
  }
})
</script>
{% endraw %}

### Renderizando uma lista

``` html
<div id="app">
  <ul>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ul>
</div>
```
``` js
new Vue({
  el: '#app',
  data: {
    todos: [
      { text: 'Aprenda JavaScript' },
      { text: 'Aprenda Vue.js' },
      { text: 'Crie algo incrível' }
    ]
  }
})
```
{% raw %}
<div id="app3" class="demo">
  <ul>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ul>
</div>
<script>
new Vue({
  el: '#app3',
  data: {
    todos: [
      { text: 'Aprenda JavaScript' },
      { text: 'Aprenda Vue.js' },
      { text: 'Crie algo incrível' }
    ]
  }
})
</script>
{% endraw %}

### Handle User Input

``` html
<div id="app">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
```
``` js
new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
{% raw %}
<div id="app4" class="demo">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
<script>
new Vue({
  el: '#app4',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

### All Together Now

``` html
<div id="app">
  <input v-model="newTodo" v-on:keyup.enter="addTodo">
  <ul>
    <li v-for="todo in todos">
      <span>{{ todo.text }}</span>
      <button v-on:click="removeTodo($index)">X</button>
    </li>
  </ul>
</div>
```
``` js
new Vue({
  el: '#app',
  data: {
    newTodo: '',
    todos: [
      { text: 'Add some todos' }
    ]
  },
  methods: {
    addTodo: function () {
      var text = this.newTodo.trim()
      if (text) {
        this.todos.push({ text: text })
        this.newTodo = ''
      }
    },
    removeTodo: function (index) {
      this.todos.splice(index, 1)
    }
  }
})
```
{% raw %}
<div id="app5" class="demo">
  <input v-model="newTodo" v-on:keyup.enter="addTodo">
  <ul>
    <li v-for="todo in todos">
      <span>{{ todo.text }}</span>
      <button v-on:click="removeTodo($index)">X</button>
    </li>
  </ul>
</div>
<script>
new Vue({
  el: '#app5',
  data: {
    newTodo: '',
    todos: [
      { text: 'Add some todos' }
    ]
  },
  methods: {
    addTodo: function () {
      var text = this.newTodo.trim()
      if (text) {
        this.todos.push({ text: text })
        this.newTodo = ''
      }
    },
    removeTodo: function (index) {
      this.todos.splice(index, 1)
    }
  }
})
</script>
{% endraw %}

I hope this gives you a basic idea of how Vue.js works. I'm sure you also have many questions now - read along, and we will cover them in the rest of the guide.
