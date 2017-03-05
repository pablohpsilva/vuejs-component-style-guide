# Style Guide para Componentes Vue.js

## Propósito

Este guia provê uma estrutura uniforme para o seu código [Vue.js](http://vuejs.org/), tornando-o mais:

* Fácil para desenvolvedores/times entender e encontrar coisas;
* Fácil para IDE's interpretarem e dar suporte ao código;
* Fácil de (re)usar ferramentas que você já usa;
* Fácil de fazer cache e gerar bundles separadamente;

Este guia foi inspirado pelo [RiotJS Style Guide](https://github.com/voorhoede/riotjs-style-guide) por [De Voorhoede](https://github.com/voorhoede).

## Índice

* [Desenvolvimento baseado em módulo](#desenvolvimento-baseado-em-modulo)
* [Nomes de componentes Vue](#nomes-de-componentes-vue)
<!-- * [Use `*.vue` extension](#use-vue-extension) -->
* [Mantenha simples as expressões dos componentes](#mantenha-simples-as-expressoes-dos-componentes)
* [Mantenha `props` primitivas](#mantenha-props-primitivas)
* [Pense bem nas `props` do seu componente](#pense-bem-nas-props-do-seu-componente)
* [`this` já é o seu componente](#this-ja-e-o-seu-componente)
* [Estrutura do componente](#estrutura-do-componente)
* [Nome de eventos do componente](#nome-de-eventos-do-componente)
* [Evite `this.$parent`](#evite-thisparent)
* [Use `this.$refs` com cuidado](#use-thisrefs-com-cuidado)
* [Use o nome do componente como escopo para o `<style>`](#use-o-nome-do-componente-como-escopo-para-o-style)
* [Documente a API do seu componente](#documente-a-api-do-seu-componente)
* [Crie uma demonstração](#crie-uma-demonstracao)
* [Lint os arquivos do seu componente](#lint-os-arquivos-do-seu-componente)
<!-- * [Add badge to your project](#add-badge-to-your-project) -->


## Desenvolvimento baseado em módulo

Sempre construa sua aplicação (app) em pequenos módulos que façam somente uma única coisa (e a faça bem feita).

Um módulo é uma parte da aplicação que é auto-contida. A biblioteca Vue.js foi especificamente desenvolvida para ajudar-lhe na criação de módulos *view-logic*.

### Porque?

Módulos pequenos são fáceis de aprender, compreender, manter, reusar e debugar, tanto para você quanto para outros desenvolvedores;

### Como?

Cada componente Vue (como qualquer outro módulo) deve ser [FIRST](https://addyosmani.com/first/): *Focused* ([single responsibility](http://en.wikipedia.org/wiki/Single_responsibility_principle)), *Independent*, *Reusable*, *Small* e *Testable*.

Se o seu componente faz muitas coisas ou está grande demais, quebre-o em componentes menores, cada um fazendo uma única coisa. Como uma regra
If your component does too much or gets too big, split it up into smaller components which each do just one thing. Como um princípio básico, tente manter o componente com menos de 100 linhas de código.
Assegure que o componente Vue funcione isoladamente, como por exemplo, se somente ele é necessário para uma demonstração dele mesmo.

[↑ voltar para o Índice](#indice)


## Nomes de componentes Vue

Cada nome dos componentes devem ser:

* **Significativo**: não super especificado, não muito abstrato;
* **Pequeno**: 2 ou 3 palavras.
* **Pronunciável**: queremos poder falar sobre eles;

Os nomes também devem ser:

* **Compatível com especificação de elemento personalizado**: [inclua um hífen](https://www.w3.org/TR/custom-elements/#concepts), não use nomes reservados.
* **Usar `app-` como namespace**: se for muito genérico, caso contrário, use uma única palavra, para que ele seja facilmente reusável em outros projetos.

### Porque?

* O nome deve ser usado para falar sobre o componente, logo, precisa ser pequeno, significativo e pronunciável.

### Como?

```html
<!-- recomendado -->
<app-header></app-header>
<user-list></user-list>
<range-slider></range-slider>

<!-- evite -->
<btn-group></btn-group> <!-- pequeno, mas não pronunciável. Ao invés, use `button-group` -->
<ui-slider></ui-slider> <!-- todos os componentes são elementos ui, logo é insignificativo -->
<slider></slider> <!-- Não é compatível com especificação de elemento personalizado -->
```

[↑ voltar para o Índice](#indice)


## Mantenha simples as expressões dos componentes

As expressões em linha do Vue.js's são 100% Javascript. Isso torna-as extremamente poderosas, porém, potencialmente complexas. Sendo assim, você deve **manter simples as expressões dos componentes**.

### Porque?

* Expressões complexas em linha são difíceis de ler;
* Expressões em linhas não podem ser reusadas em outros lugares. Isso pode levar a duplicação de código;
* IDEs tipicamente não dão suporte a sintaxe de expressões, logo, sua IDE não pode fazer autocomple ou fazer validart.

### Como?

Se elas tornam-se muito complexas ou difíceis de ler, **mova-as para um método ou uma `computed property`**!

```html
<!-- recomendado -->
<template>
	<h1>
		{{ `${year}-${month}` }}
	</h1>
</template>
<script type="text/javascript">
  export default {
    computed: {
      month() {
        return this.twoDigits((new Date()).getUTCMonth() + 1);
      },
      year() {
        return (new Date()).getUTCFullYear();
      }
    },
    methods: {
      twoDigits(num) {
        return ('0' + num).slice(-2);
      }
    },
  };
</script>

<!-- evite -->
<template>
	<h1>
		{{ `${(new Date()).getUTCFullYear()}-${('0' + ((new Date()).getUTCMonth()+1)).slice(-2)}` }}
	</h1>
</template>
```

[↑ voltar para o Índice](#indice)


## Mantenha `props` primitivas

While Vue.js supports passing complex JavaScript objects via these attributes, you should try to **keep the component props as primitive as possible**. Try to only use [JavaScript primitives](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) (strings, numbers, booleans) and functions. Avoid complex objects.

### Porque?

* By using an attribute for each option separately the component has a clear and expressive API.
* By using only primitives and functions as option values our component APIs are similar to the APIs of native HTML(5) elements. Which makes our custom elements directly familiar.
* By using an attribute for each option, other developers can easily understand what is passed to the component instance.
* When passing complex objects it's not apparent which properties and methods of the objects are actually being used by the custom components. This makes it hard to refactor code and can lead to code rot.

### Como?

Use a component attribute per option, with a primitive or function as value:

```html
<!-- recomendado -->
<range-slider
  :values="[10, 20]"
  min="0"
  max="100"
  step="5"
  :on-slide="updateInputs"
  :on-end="updateResults">
</range-slider>

<!-- evite -->
<range-slider :config="complexConfigObject"></range-slider>
```

[↑ voltar para o Índice](#indice)


## Pense bem nas `props` do seu componente

In Vue.js your component props are your API. A robust and predictable API makes your components easy to use by other developers.

Component props are passed via custom HTML attributes. The values of these attributes can be Vue.js plain strings (`:attr="value"` or `v-bind:attr="value"`) or missing entirely. You should **Pense bem nas `props` do seu componente** to allow for these different cases.

### Porque?

Harnessing your component props ensures your component will always function (defensive programming). Even when other developers later use your components in ways you haven't thought of yet.

### Como?

* Use defaults for option values.
* Use `type` option to [validate](http://vuejs.org/v2/guide/components.html#Prop-Validation) values to an expected type.**[1\*]**
* Check if option exists before using it.

```html
<template>
  <input type="range" v-model="value" :max="max" :min="min">
</template>
<script type="text/javascript">
  export default {
    props: {
      max: {
        type: Number, // [1*] This will validate the 'max' prop to be a Number.
        default() { return 10; },
      },
      min: {
        type: Number,
        default() { return 0; },
      },
      value: {
        type: Number,
        default() { return 4; },
      },
    },
  };
</script>
```

[↑ voltar para o Índice](#indice)


## `this` já é o seu componente

Within the context of a Vue.js component element, `this` is bound to the component instance.
Therefore when you need to reference it in a different context, ensure `this` is available as `component`.

In other words: Do **NOT** code things like `const self = this;` anymore. You're safe using Vue components.

### Porque?

* By assigning `this` to a variable named `component` the variable tells developers it's bound to the component instance wherever it's used.

### Como?

```html
<script type="text/javascript">
export default {
  methods: {
    hello() {
      return 'hello';
    },
    printHello() {
      console.log(this.hello());
    },
  },
};
</script>

<!-- evite -->
<script type="text/javascript">
export default {
  methods: {
    hello() {
      return 'hello';
    },
    printHello() {
      const self = this; // unnecessary
      console.log(self.hello());
    },
  },
};
</script>
```

[↑ voltar para o Índice](#indice)


## Estrutura do componente

Make it easy to reason and follow a sequence of thoughts. See the How.

### Porque?

* Having the component export a clear and grouped object, makes the code easy to read and easier for developers to have a code standard.
* Alphabetizing the properties, data, computed, watches, and methods makes them easy to find.
* Again, grouping makes the component easier to read (props, data and computed; watch and methods; lifecycle methods, etc.);
* Use the `name` attribute. Using [vue devtools](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=en) and that attribute will make your development/testing easier;
* Use a CSS naming Methodology, like [BEM](https://medium.com/tldr-tech/bem-blocks-elements-and-modifiers-6b3b0af9e3ea#.bhnomd7gw) - [details?](#use-o-nome-do-componente-como-escopo-para-o-style);
* Use the template-script-style .vue file organization. The odds are you'll spend more time developing/fixing/testing on HTML than JavaScript in most cases.

### Como?

Estrutura do componente:

```html
<template lang="html">
	<div class="Ranger__Wrapper">
		<!-- ... -->
	</div>
</template>

<script type="text/javascript">
  export default {
		// Do not forget this little guy
    name: 'RangeSlider',
    // compose new components
    extends: {},
    // component properties/variables
    props: {
			bar: {}, // Alphabetized
			foo: {},
			fooBar: {},
		},
    // variables
    data() {},
    computed: {},
    // when component uses other components
    components: {},
    // methods
    watch: {},
    methods: {},
    // component Lifecycle hooks
    beforeCreate() {},
    mounted() {},
};
</script>

<style scoped>
  .Ranger__Wrapper { /* ... */ }
</style>
```

[↑ voltar para o Índice](#indice)

## Nome de eventos do componente

Vue.js provides all Vue handler functions and expressions are strictly bound to the ViewModel. Each component events should follow a good naming style that will avoid issues during the development. See the **Why** below.

### Porque?

* Developers are free to use native likes event names and it can cause confusion down the line;
* The freedom of naming events can lead to a [DOM templates incompatibility](https://vuejs.org/v2/guide/components.html#DOM-Template-Parsing-Caveats);

### Como?

* Event names should be kebab-cased;
* A unique event name should be fired for unique actions in your component that will be of interest to the outside world, like: upload-success, upload-error or even dropzone-upload-success, dropzone-upload-error (if you see the need for having a scoped prefix);
* Events should either end in verbs in the infinitive form (e.g. client-api-load) or nouns (e.g drive-upload-success) ([source](https://github.com/GoogleWebComponents/style-guide#events));

[↑ voltar para o Índice](#indice)

## Evite `this.$parent`

Vue.js supports nested components which have access to their parent context. Accessing context outside your vue component violates the [FIRST](https://addyosmani.com/first/) rule of [component based development](#desenvolvimento-baseado-em-modulo). Therefore you should **avoid using `this.$parent`**.

### Porque?

* A vue component, like any component, must work in isolation. If a component needs to access its parent, this rule is broken.
* If a component needs access to its parent, it can no longer be reused in a different context.

### Como?

* Pass values from the parent to the child component using attribute/properties.
* Pass methods defined on the parent component to the child component using callbacks in attribute expressions.
* Emit events from child components and catch it on parent component.

[↑ voltar para o Índice](#indice)

## Use `this.$refs` com cuidado

Vue.js supports components to have access to other components and basic HTML elements context via `ref` attribute. That attribute will provide an accessible way through `this.$refs` to a component or DOM element context. In most cases, the need to access **other components** context via `this.$refs` could be avoided. This is why you should be careful when using it to avoid wrong component APIs.

### Porque?

* A vue component, like any component, **must work in isolation**. If a component does not support all the access needed, it was badly designed/implemented.
* Properties and events should be sufficient to most of your components.

### Como?

* Create a good component API.
* Always focus on the component purpose out of the box.
* Never write specific code. If you need to write specific code inside a generic component, it means its API isn't generic enough or maybe you need a new component to manage other cases.
* Check all the props to see if something is missing. If it is, create an issue or enhance the component yourself.
* Check all the events. In most cases developers forget that Child-Parent communication (events) is needed, that's why they only remember the Parent-Child communication (using props).
* **Props down, events up!** Upgrade your component when requested with a good API and isolation as goals.
* Using `this.$refs` on components should be used when props and events are exhausted and having it makes sense (see the example below).
* Using `this.$refs` to access DOM elements (instead of doing `jQuery`, `document.getElement*`, `document.queryElement`) is just fine, when the element can't be manipulated with data bindings or for a directive.

```html
<!-- good, no need for ref -->
<range :max="max"
  :min="min"
  @current-value="currentValue"
  :step="1"></range>
```

```html
<!-- good example of when to use this.$refs -->
<modal ref="basicModal">
  <h4>Basic Modal</h4>
  <button class="primary" @click="$refs.basicModal.close()">Close</button>
</modal>
<button @click="$refs.basicModal.open()">Open modal</button>

<!-- Modal component -->
<template>
  <div v-show="active">
    <!-- ... -->
  </div>
</template>

<script>
  export default {
    // ...
    data() {
        return {
            active: false,
        };
    },
    methods: {
      open() {
      	this.active = true;
      },
      hide() {
      	this.active = false;
      },
    },
    // ...
  };
</script>

```

```html
<!-- avoid accessing something that could be emitted -->
<template>
  <range :max="max"
    :min="min"
    ref="range"
    :step="1"></range>
</template>

<script>
  export default {
    // ...
    methods: {
      getRangeCurrentValue() {
      	return this.$refs.range.currentValue;
      },
    },
    // ...
  };
</script>
```

[↑ voltar para o Índice](#indice)


## Use o nome do componente como escopo para o `<style>`

Vue.js component elements are custom elements which can very well be used as style scope root.
Alternatively the component name can be used as CSS class namespace.

### Porque?

* Scoping styles to a component element improves predictability as its prevents styles leaking outside the component element.
* Using the same name for the module directory, the Vue.js component and the style root makes it easy for developers to understand they belong together.

### Como?

Use the component name as a namespace prefix based on BEM and OOCSS **and** use the `scoped` attribute on your style class.
The use of `scoped` will tell your Vue compiler to add a signature on every class that your `<style>` have. That signature will force your browser (if it supports) to apply your components
CSS on all tags that compose your component, leading to a no leaking css styling.

```html
<style scoped>
	/* recommended */
	.MyExample { }
	.MyExample li { }
	.MyExample__item { }

	/* avoid */
	.My-Example { } /* not scoped to component or module name, not BEM compliant */
</style>
```

[↑ voltar para o Índice](#indice)


## Documente a API do seu componente

A Vue.js component instance is created by using the component element inside your application. The instance is configured through its custom attributes. For the component to be used by other developers, these custom attributes - the component's API - should be documented in a `README.md` file.

### Porque?

* Documentation provides developers with a high level overview to a component, without the need to go through all its code. This makes a component more accessible and easier to use.
* A component's API is the set of custom attributes through which its configured. Therefore these are especially of interest to other developers which only want to consume (and not develop) the component.
* Documentation formalises the API and tells developers which functionality to keep backwards compatible when modifying the component's code.
* `README.md` is the de facto standard filename for documentation to be read first. Code repository hosting services (Github, Bitbucket, Gitlab etc) display the contents of the the README's, directly when browsing through source directories. This applies to our module directories as well.

### Como?

Add a `README.md` file to the component's module directory:

```
range-slider/
├── range-slider.vue
├── range-slider.less
└── README.md
```

Within the README file, describe the functionality and the usage of the module. For a vue component its most useful to describe the custom attributes it supports as those are its API:


# Range slider

## Functionality

The range slider lets the user to set a numeric range by dragging a handle on a slider rail for both the start and end value.

This module uses the [noUiSlider](http://refreshless.com/nouislider/) for cross browser and touch support.

## Usage

`<range-slider>` supports the following custom component attributes:

| attribute | type | description
| --- | --- | ---
| `min` | Number | number where range starts (lower limit).
| `max` | Number | Number where range ends (upper limit).
| `values` | Number[] *optional* | Array containing start and end value.  E.g. `values="[10, 20]"`. Defaults to `[opts.min, opts.max]`.
| `step` | Number *optional* | Number to increment / decrement values by. Defaults to 1.
| `on-slide` | Function *optional* | Function called with `(values, HANDLE)` while a user drags the start (`HANDLE == 0`) or end (`HANDLE == 1`) handle. E.g. `on-slide={ updateInputs }`, with `component.updateInputs = (values, HANDLE) => { const value = values[HANDLE]; }`.
| `on-end` | Function *optional* | Function called with `(values, HANDLE)` when user stops dragging a handle.

For customising the slider appearance see the [Styling section in the noUiSlider docs](http://refreshless.com/nouislider/more/#section-styling).


[↑ voltar para o Índice](#indice)


## Crie uma demonstração

Add a `index.html` file with demos of the component with different configurations, showing how the component can be used.

### Porque?

* A component demo proves the module works in isolation.
* A component demo gives developers a preview before having to dig into the documentation or code.
* Demos can illustrate all the possible configurations and variations a component can be used in.

[↑ voltar para o Índice](#indice)


## Lint os arquivos do seu componente

Linters improve code consistency and help trace syntax errors. .vue files can be linted adding the `eslint-plugin-html` in your project. If you choose, you can start a project with ESLint enabled by default using `vue-cli`;

### Porque?

* Linting component files ensures all developers use the same code style.
* Linting component files helps you trace syntax errors before it's too late.

### Como?

To allow linters to extract the scripts from your `*.vue` files, [put script inside a `<script>` component](#use-script-inside-component) and [Mantenha simples as expressões dos componentes](#keep-component-expressions-simple) (as linters don't understand those). Configure your linter to allow global variables `vue` and component `opts`.

#### ESLint

[ESLint](http://eslint.org/) requires an extra [ESLint HTML plugin](https://github.com/BenoitZugmeyer/eslint-plugin-html#eslint-plugin-html) to extract the script from the component files.

Configure ESLint in `módulos/.eslintrc` (so IDEs can interpret it as well):
```json
{
  "extends": "eslint:recommended",
  "plugins": ["html"],
  "env": {
    "browser": true
  },
  "globals": {
    "opts": true,
    "vue": true
  }
}
```

Run ESLint
```bash
eslint módulos/**/*.vue
```

#### JSHint

[JSHint](http://jshint.com/) can parse HTML (using `--extra-ext`) and extract script (using `--extract=auto`).

Configure JSHint in `módulos/.jshintrc` (so IDEs can interpret it as well):
```json
{
  "browser": true,
  "predef": ["opts", "vue"]
}
```

Run JSHint
```bash
jshint --config módulos/.jshintrc --extra-ext=html --extract=auto módulos/
```
Note: JSHint does not accept `vue` as extension, but only `html`.

[↑ voltar para o Índice](#indice)

---

# Wanna help?

Fork it and Pull Request what you think it should be good to have or just create an [Issue](https://github.com/pablohpsilva/vuejs-component-style-guide/issues).

# Thank you for your help!
@miljan-aleksic on issue [#1](https://github.com/pablohpsilva/vuejs-component-style-guide/issues/1)


<!-- ## Add badge to your project

You can use the *Vue.js Style Guide badge* to link to this guide:

[![Vue.js Style Guide badge](https://cdn.rawgit.com/voorhoede/Vue.js-style-guide/master/Vue.js-style-guide.svg)](https://github.com/voorhoede/Vue.js-style-guide)

### Porque?

Inform other developers your project is following the Vue.js Style Guide. And let them know where they can find this guide.

### Como?

Include the badge in your project. In markdown:

```markdown
[![Vue.js Style Guide badge](https://cdn.rawgit.com/voorhoede/Vue.js-style-guide/master/Vue.js-style-guide.svg)](https://github.com/voorhoede/Vue.js-style-guide)
```

Or html:

```html
<a href="https://github.com/voorhoede/Vue.js-style-guide">
    <img alt="Vue.js Style Guide badge"
    	 src="https://cdn.rawgit.com/voorhoede/Vue.js-style-guide/master/Vue.js-style-guide.svg">
</a>
```

[↑ voltar para o Índice](#indice)

---

## License

[![CC0](http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

[De Voorhoede](https://twitter.com/devoorhoede) waives all rights to this work worldwide under copyright law, including all related and neighboring rights, to the extent allowed by law.

You can copy, modify, distribute and perform the work, even for commercial purposes, all without asking permission.
 -->
