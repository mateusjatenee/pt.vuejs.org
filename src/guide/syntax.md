---
title: Sintaxe de Data Binding
type: guide
order: 4
---

O Vue usa uma implementação de DOM baseada em templates. Isso significa que todos os templates do Vue.js são essencialmente HTML válido e melhorado com alguns atributos especiais. Mantenha isso em mente, já que é isso que faz os templates do Vue diferentes de templates baseados em strings.

## Interpolações

### Texto

The most basic form of data binding is text interpolation using the "Mustache" syntax (double curly braces):
A forma mais básica de data binding é interpolação de texto usando a sintaxe "Mustache" (Bigode) => {{ }}

``` html
<span>Mensagem: {{ msg }}</span>
```

A tag mustache (bigode) será substituída pelo valor da propriedade `msg` nos objeto de dados correspondente. Será, também, atualizado toda vez que o valor da propriedade `msg` mudar.
Você também pode criar interpolações que ocorrem apenas uma vez:

``` html
<span>Isso nunca mudará: {{* msg }}</span>
```

### HTML Puro

As duas chaves interpretam os dados como texto simples, não HTML. Para exibir o verdadeiro HTML, você precisa usar três chaves:

``` html
<div>{{{ raw_html }}}</div>
```

O conteúdo será exibido como HTML simples — data bindings serão ignorados. Se você precisa reutilizar pedaços de templates, você deve usar [templates parciais](/api/#partial).

<p class="tip">Renderizar HTML arbitrariamente no seu site pode ser muito perigoso porque pode levar facilmente a um [ataque XSS](https://en.wikipedia.org/wiki/Cross-site_scripting). Apenas use interpolação de HTML quando puder confiar no conteúdo e **nunca** use em conteúdo provido pelo usuário.</p>

### Atributos

Chaves duplas também podem ser usadas dentro de atributos HTML:

``` html
<div id="item-{{ id }}"></div>
```

Note que interpolações de atributo são desabilitadas em diretivas e atributos especiais do Vue. Não se preocupe, o Vue irá automaticamente emitir alertas quando as chaves duplas forem usadas nos lugares errados.

## Binding Expressions

O texto que colocamos dentro das chaves duplas são chamados de **binding expressions**. No Vue, uma binding expression consiste de uma única expressão JavaScript seguida opcionalmente de um ou mais filtros.

### Expressões JavaScript

Até agora nós somente ligamos coisas à propriedades simples em nossos templates. Mas, na verdade, o Vue suporta todo o poder de expressões Javascript dentro de data bindings:

``` html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}
```

Essas expressões serão processadas no escopo dos dados da instância do Vue à qual pertence. Uma restrição é que cada ligação pode conter apenas **uma única expressão**, então o seguinte **NÃO** irá funcionar: 

``` html
<!-- isso é uma declaração, não uma expressão: -->
{{ var a = 1 }}

<!-- controle de fluxo também não irá funcionar. Use expressões ternárias -->
{{ if (ok) { return message } }}
```

### Filters

Vue.js allows you to append optional "filters" to the end of an expression, denoted by the "pipe" symbol:

``` html
{{ message | capitalize }}
```

Here we are "piping" the value of the `message` expression through the built-in `capitalize` filter, which is in fact just a JavaScript function that returns the capitalized value. Vue.js provides a number of built-in filters, and we will talk about how to write your own filters later.

Note that the pipe syntax is not part of JavaScript syntax, therefore you cannot mix filters inside expressions; you can only append them at the end of an expression.

Filters can be chained:

``` html
{{ message | filterA | filterB }}
```

Filters can also take arguments:

``` html
{{ message | filterA 'arg1' arg2 }}
```

The filter function always receives the expression's value as the first argument. Quoted arguments are interpreted as plain string, while un-quoted ones will be evaluated as expressions. Here, the plain string `'arg1'` will be passed into the filter as the second argument, and the value of expression `arg2` will be evaluated and passed in as the third argument.

## Directives

Directives are special attributes with the `v-` prefix. Directive attribute values are expected to be **binding expressions**, so the rules about JavaScript expressions and filters mentioned above apply here as well. A directive's job is to reactively apply special behavior to the DOM when the value of its expression changes. Let's review the example we saw in the introduction:

``` html
<p v-if="greeting">Hello!</p>
```

Here, the `v-if` directive would remove/insert the `<p>` element based on the truthiness of the value of the expression `greeting`.

### Arguments

Some directives can take an "argument", denoted by a colon after the directive name. For example, the `v-bind` directive is used to reactively update an HTML attribute:

``` html
<a v-bind:href="url"></a>
```

Here `href` is the argument, which tells the `v-bind` directive to bind the element's `href` attribute to the value of the expression `url`. You may have noticed this achieves the same result as an attribute interpolation using `{% raw %}href="{{url}}"{% endraw %}`: that is correct, and in fact, attribute interpolations are translated into `v-bind` bindings internally.

Another example is the `v-on` directive, which listens to DOM events:

``` html
<a v-on:click="doSomething">
```

Here the argument is the event name to listen to. We will talk about event handling in more detail too.

### Modifiers

Modifiers are special postfixes denoted by a dot, which indicates that a directive should be bound in some special way. For example, the `.literal` modifier tells the directive to interpret its attribute value as a literal string rather than an expression:

``` html
<a v-bind:href.literal="/a/b/c"></a>
```

Of course, this seems pointless because we can just do `href="/a/b/c"` instead of using a directive. The example here is just for demonstrating the syntax. We will see more practical uses of modifiers later.

## Shorthands

The `v-` prefix serves as a visual cue for identifying Vue-specific attributes in your templates. This is useful when you are using Vue.js to apply dynamic behavior to some existing markup, but can feel verbose for some frequently used directives. At the same time, the need for the `v-` prefix becomes less important when you are building an SPA where Vue.js manages every template. Therefore, Vue.js provides special shorthands for two of the most often used directives, `v-bind` and `v-on`:

### `v-bind` Shorthand

``` html
<!-- full syntax -->
<a v-bind:href="url"></a>

<!-- shorthand -->
<a :href="url"></a>

or

<!-- full syntax -->
<button v-bind:disabled="someDynamicCondition">Button</button>

<!-- shorthand -->
<button :disabled="someDynamicCondition">Button</button>
```


### `v-on` Shorthand

``` html
<!-- full syntax -->
<a v-on:click="doSomething"></a>

<!-- shorthand -->
<a @click="doSomething"></a>
```

They may look a bit different from "valid" HTML, but all Vue.js supported browsers can parse it correctly, and they do not appear in the final rendered markup. The shorthand syntax is totally optional, but you will likely appreciate it when you learn more about its usage later.
