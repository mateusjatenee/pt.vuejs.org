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

### Filtros

O Vue lhe permite adicionar "filtros" opcionais ao final de uma expressão, detonado pelo símbolo "pipe" ( | ):

``` html
{{ message | capitalize }}
```

Aqui estamos passando o valor da expressão `message` pelo filtro embutido `capitalize`, que na verdade apenas uma função que retorna o valor capitalizado. O Vue fornece um grande número de filtros embutidos, e falaremos de como criar seu próprio filtro depois.


Note que a sintaxe "pipe" não é parte da sintaxe do Javascript, portanto você não pode misturar filtros dentro de expressões; você pode apenas adicioná-las no final de uma expressão.

Filtros podem ser encadeados:

``` html
{{ message | filterA | filterB }}
```

Filtros também podem receber parâmetros:

``` html
{{ message | filterA 'arg1' arg2 }}
```

A função filter sempre recebe o valor da expressão como primeiro parâmetro. Parâmetros dentro de aspas serão tratados como strings, enquanto parâmetros sem aspas serão tratrados como expressões. Aqui, a string `arg1` será passada a um filtro como o segundo parâmetro, e o valor da expressão `arg2` será tratada e passada como terceiro parâmetro.

## Diretivas

Diretivas são atributos especiais com o prefixo `v-`. Espera-se que os valores do atributo de uma diretiva sejam **binding expressions*, portanto as regras sobre expressões e filtros JavaScript que falamos acima se aplicam aqui também. O trabalho de uma diretiva é aplicar comportamentos especiais a DOM de forma reativa quando o valor da expressão mudar. Vamos revisar o exemplo que vimos na introdução:

``` html
<p v-if="greeting">Olá!</p>
```

Aqui, a diretiva `v-if` iria remover/inserir o elemento `<p>` baseado na veracidade do valor da expressão `greeting`. 

### Argumentos

Algumas diretivas podem receber "parâmetros", detonados por dois pontos após o nome da diretiva. Por exemplo, a diretiva `v-bind` é usada para atualizar um atributo HTML reativamente:

``` html
<a v-bind:href="url"></a>
```

Aqui, `href` é o parâmetro, que diz à diretiva `v-bind` para ligar o atributo `href` do elemento ao valor da expressão `url`. Você talvez tenha notado que isso tem o mesmo resultado de uma interpolação de atributo usando `{%raw %}href="{{url}}"{% endraw %}`: isso é correto, e de fato, interpolações de atributo são convertidas para ligações `v-bind` internamente.

Outro exemplo é a diretiva `v-on`, que escuta os eventos da DOM:

``` html
<a v-on:click="doSomething">
```

Aqui, o argumento é o nome do evento ao qual a diretiva deve escutar. Vamos falar mais sobre event handling no futuro.

### Modificadores

Modificares são prefixos especiais detonados por um ponto, que indica que a diretiva deve ser "ligada" de forma diferente. Por exemplo, o modificador `.literal` diz à diretiva para interpretar o valor do atributo como uma string ao invés de uma expressão:

``` html
<a v-bind:href.literal="/a/b/c"></a>
```

Esse exemplo parece sem sentido já que podemos usar apenas `href="/a/b/c"` ao invés de usar a diretiva. O exemplo foi apenas para mostrar a sintaxe — veremos uso prático de modificadores no futuro.

## Abreviações

O prefixo `v-` serve como uma forma de identificar atributos especificos do Vue nos seus templates. Isso é útil quando você está usando o Vue.js para aplicar comportamentos dinâmicos a uma tag existente, mas pode parecer verboso para algumas diretivas usadas frequentemente. Ao mesmo tmepo, a necessidade do prefixo `v-` torna-se menos importante quando você está montando um SPA onde o Vue administra todos os templates. Portanto, o Vue fornece abreviações especiais para as diretivas mais usadas, `v-bind` e `v-on`:

### Abreviacões de `v-bind`

``` html
<!-- sintaxe completa -->
<a v-bind:href="url"></a>

<!-- abreviação -->
<a :href="url"></a>

ou

<!-- sintaxe completa -->
<button v-bind:disabled="someDynamicCondition">Botão</button>

<!-- abreviação -->
<button :disabled="someDynamicCondition">Botão</button>
```


### Abreviação `v-on`

``` html
<!-- full syntax -->
<a v-on:click="doSomething"></a>

<!-- shorthand -->
<a @click="doSomething"></a>
```

Elas podem parecer um pouco diferentes de HTML "válido", mas todos os browsers que suportam o Vue.js podem analisar ele corretamente, e eles não aparecem na marcação final renderizada. A sintaxe abreviada é totalmente opcional, mas você provavelmente irá gostar de usá-la quando você aprender mais sobre seu uso.
