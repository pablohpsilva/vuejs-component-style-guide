# Vue.js Component Style Guide

<p align="center">
  <img src="https://raw.githubusercontent.com/pablohpsilva/vuejs-component-style-guide/master/img/logo.png"/>
</p>

### Translations
* [Brazilian Portuguese](https://pablohpsilva.github.io/vuejs-component-style-guide/#/portuguese)
* [Chinese](https://pablohpsilva.github.io/vuejs-component-style-guide/#/chinese)
* [Japanese](https://pablohpsilva.github.io/vuejs-component-style-guide/#/japanese)
* [Korean](https://pablohpsilva.github.io/vuejs-component-style-guide/#/korean)
* [Russian](https://pablohpsilva.github.io/vuejs-component-style-guide/#/russian)

## Purpose

This guide provides a uniform way to structure your [Vue.js](http://vuejs.org/) code. Making it:

* easier for developers/team members to understand and find things.
* easier for IDEs to interpret the code and provide assistance.
* easier to (re)use build tools you already use.
* easier to cache and serve bundles of code separately.

This guide is inspired by the [RiotJS Style Guide](https://github.com/voorhoede/riotjs-style-guide) by [De Voorhoede](https://github.com/voorhoede).

## Table of Contents

* [Module based development](#module-based-development)
* [vue component names](#vue-component-names)
* [Keep component expressions simple](#keep-component-expressions-simple)
* [Keep component props primitive](#keep-component-props-primitive)
* [Harness your component props](#harness-your-component-props)
* [Assign `this` to `component`](#assign-this-to-component)
* [Component structure](#component-structure)
* [Component event names](#component-event-names)
* [Avoid `this.$parent`](#avoid-thisparent)
* [Use `this.$refs` with caution](#use-thisrefs-with-caution)
* [Use component name as style scope](#use-component-name-as-style-scope)
* [Document your component API](#document-your-component-api)
* [Add a component demo](#add-a-component-demo)
* [Lint your component files](#lint-your-component-files)
* [Create components when needed](#create-components-when-needed)
<!-- * [Use `*.vue` extension](#use-vue-extension) -->
<!-- * [Add badge to your project](#add-badge-to-your-project) -->


## Module based development

Always construct your app out of small modules which do one thing and do it well.

A module is a small self-contained part of an application. The Vue.js library is specifically designed to help you create *view-logic modules*.

### Why?

Small modules are easier to learn, understand, maintain, reuse and debug. Both by you and other developers.

### How?

Each Vue component (like any module) must be [FIRST](https://addyosmani.com/first/): *Focused* ([single responsibility](http://en.wikipedia.org/wiki/Single_responsibility_principle)), *Independent*, *Reusable*, *Small* and *Testable*.

If your component does too much or gets too big, split it up into smaller components which each do just one thing. As a rule of thumb, try to keep each component file less than 100 lines of code.
Also ensure your Vue component works in isolation. For instance by adding a stand-alone demo.

[↑ back to Table of Contents](#table-of-contents)


## Vue Component Names

Each component name must be:

* **Meaningful**: not over specific, not overly abstract.
* **Short**: 2 or 3 words.
* **Pronounceable**: we want to be able to talk about them.

Vue component names must also be:

* **Custom element spec compliant**: [include a hyphen](https://www.w3.org/TR/custom-elements/#concepts), don't use reserved names.
* **`app-` namespaced**: if very generic and otherwise 1 word, so that it can easily be reused in other projects.

### Why?

* The name is used to communicate about the component. So it must be short, meaningful and pronounceable.

### How?

```html
<!-- recommended -->
<app-header></app-header>
<user-list></user-list>
<range-slider></range-slider>

<!-- avoid -->
<btn-group></btn-group> <!-- short, but unpronounceable. use `button-group` instead -->
<ui-slider></ui-slider> <!-- all components are ui elements, so is meaningless -->
<slider></slider> <!-- not custom element spec compliant -->
```

[↑ back to Table of Contents](#table-of-contents)


## Keep component expressions simple

Vue.js's inline expressions are 100% Javascript. This makes them extremely powerful, but potentially also very complex. Therefore you should **keep expressions simple**.

### Why?

* Complex inline expressions are hard to read.
* Inline expressions can't be reused elsewhere. This can lead to code duplication and code rot.
* IDEs typically don't have support for expression syntax, so your IDE can't autocomplete or validate.

### How?

If it gets too complex or hard to read **move it to methods or computed properties**!

```html
<!-- recommended -->
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

<!-- avoid -->
<template>
  <h1>
    {{ `${(new Date()).getUTCFullYear()}-${('0' + ((new Date()).getUTCMonth()+1)).slice(-2)}` }}
  </h1>
</template>
```

[↑ back to Table of Contents](#table-of-contents)


## Keep component props primitive

While Vue.js supports passing complex JavaScript objects via these attributes, you should try to **keep the component props as primitive as possible**. Try to only use [JavaScript primitives](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) (strings, numbers, booleans) and functions. Avoid complex objects.

### Why?

* By using an attribute for each prop separately the component has a clear and expressive API;
* By using only primitives and functions as props values our component APIs are similar to the APIs of native HTML(5) elements;
* By using an attribute for each prop, other developers can easily understand what is passed to the component instance;
* When passing complex objects it's not apparent which properties and methods of the objects are actually being used by the custom components. This makes it hard to refactor code and can lead to code rot.

### How?

Use a component attribute per props, with a primitive or function as value:

```html
<!-- recommended -->
<range-slider
  :values="[10, 20]"
  :min="0"
  :max="100"
  :step="5"
  @on-slide="updateInputs"
  @on-end="updateResults">
</range-slider>

<!-- avoid -->
<range-slider :config="complexConfigObject"></range-slider>
```

[↑ back to Table of Contents](#table-of-contents)


## Harness your component props

In Vue.js your component props are your API. A robust and predictable API makes your components easy to use by other developers.

Component props are passed via custom HTML attributes. The values of these attributes can be Vue.js plain strings (`:attr="value"` or `v-bind:attr="value"`) or missing entirely. You should **harness your component props** to allow for these different cases.

### Why?

Harnessing your component props ensures your component will always function (defensive programming). Even when other developers later use your components in ways you haven't thought of yet.

### How?

* Use defaults for props values.
* Use `type` option to [validate](http://vuejs.org/v2/guide/components.html#Prop-Validation) values to an expected type.**[1\*]**
* Check if props exists before using it.

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

[↑ back to Table of Contents](#table-of-contents)


## Assign `this` to `component`

Within the context of a Vue.js component element, `this` is bound to the component instance.
Therefore when you need to reference it in a different context, ensure `this` is available as `component`.

In other words: Do **NOT** code things like `var self = this;` anymore if you're using **ES6**. You're safe using Vue components.

### Why?

* Using ES6, there's no need to save `this` to a variable;
* In general, when using arrow functions the lexical scope is kept
* If you're **NOT** using ES6 and, therefore, not using `Arrow Functions`, you'd have to add `this` to a variable. That's the only exception.

### How?

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

<!-- avoid -->
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

[↑ back to Table of Contents](#table-of-contents)


## Component structure

Make it easy to reason and follow a sequence of thoughts. See the How.

### Why?

* Having the component export a clear and grouped object, makes the code easy to read and easier for developers to have a code standard.
* Alphabetizing the properties, data, computed, watches, and methods makes them easy to find.
* Again, grouping makes the component easier to read (name; extends; props, data and computed; components; watch and methods; lifecycle methods, etc.);
* Use the `name` attribute. Using [vue devtools](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=en) and that attribute will make your development/testing easier;
* Use a CSS naming Methodology, like [BEM](https://medium.com/tldr-tech/bem-blocks-elements-and-modifiers-6b3b0af9e3ea#.bhnomd7gw), or [rscss](https://github.com/rstacruz/rscss) - [details?](#use-component-name-as-style-scope);
* Use the template-script-style .vue file organization, as recomended by Evan You, Vue.js creator.

### How?

Component structure:

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
    // share common functionality with component mixins
    mixins: [],
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

[↑ back to Table of Contents](#table-of-contents)

## Component event names

Vue.js provides all Vue handler functions and expressions are strictly bound to the ViewModel. Each component events should follow a good naming style that will avoid issues during the development. See the **Why** below.

### Why?

* Developers are free to use native likes event names and it can cause confusion down the line;
* The freedom of naming events can lead to a [DOM templates incompatibility](https://vuejs.org/v2/guide/components.html#DOM-Template-Parsing-Caveats);

### How?

* Event names should be kebab-cased;
* A unique event name should be fired for unique actions in your component that will be of interest to the outside world, like: upload-success, upload-error or even dropzone-upload-success, dropzone-upload-error (if you see the need for having a scoped prefix);
* Events should either end in verbs in the infinitive form (e.g. client-api-load) or nouns (e.g drive-upload-success) ([source](https://github.com/GoogleWebComponents/style-guide#events));

[↑ back to Table of Contents](#table-of-contents)

## Avoid `this.$parent`

Vue.js supports nested components which have access to their parent context. Accessing context outside your vue component violates the [FIRST](https://addyosmani.com/first/) rule of [component based development](#module-based-development). Therefore you should **avoid using `this.$parent`**.

### Why?

* A vue component, like any component, must work in isolation. If a component needs to access its parent, this rule is broken.
* If a component needs access to its parent, it can no longer be reused in a different context.

### How?

* Pass values from the parent to the child component using attribute/properties.
* Pass methods defined on the parent component to the child component using callbacks in attribute expressions.
* Emit events from child components and catch it on parent component.

[↑ back to Table of Contents](#table-of-contents)

## Use `this.$refs` with caution

Vue.js supports components to have access to other components and basic HTML elements context via `ref` attribute. That attribute will provide an accessible way through `this.$refs` to a component or DOM element context. In most cases, the need to access **other components** context via `this.$refs` could be avoided. This is why you should be careful when using it to avoid wrong component APIs.

### Why?

* A vue component, like any component, **must work in isolation**. If a component does not support all the access needed, it was badly designed/implemented.
* Properties and events should be sufficient to most of your components.

### How?

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
  <button class="primary" @click="$refs.basicModal.hide()">Close</button>
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

[↑ back to Table of Contents](#table-of-contents)


## Use component name as style scope

Vue.js component elements are custom elements which can very well be used as style scope root.
Alternatively the component name can be used as CSS class namespace.

### Why?

* Scoping styles to a component element improves predictability as its prevents styles leaking outside the component element.
* Using the same name for the module directory, the Vue.js component and the style root makes it easy for developers to understand they belong together.

### How?

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

[↑ back to Table of Contents](#table-of-contents)


## Document your component API

A Vue.js component instance is created by using the component element inside your application. The instance is configured through its custom attributes. For the component to be used by other developers, these custom attributes - the component's API - should be documented in a `README.md` file.

### Why?

* Documentation provides developers with a high level overview to a component, without the need to go through all its code. This makes a component more accessible and easier to use.
* A component's API is the set of custom attributes through which its configured. Therefore these are especially of interest to other developers which only want to consume (and not develop) the component.
* Documentation formalises the API and tells developers which functionality to keep backwards compatible when modifying the component's code.
* `README.md` is the de facto standard filename for documentation to be read first. Code repository hosting services (Github, Bitbucket, Gitlab etc) display the contents of the the README's, directly when browsing through source directories. This applies to our module directories as well.

### How?

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


[↑ back to Table of Contents](#table-of-contents)


## Add a component demo

Add a `index.html` file with demos of the component with different configurations, showing how the component can be used.

### Why?

* A component demo proves the component works in isolation.
* A component demo gives developers a preview before having to dig into the documentation or code.
* Demos can illustrate all the possible configurations and variations a component can be used in.

[↑ back to Table of Contents](#table-of-contents)


## Lint your component files

Linters improve code consistency and help trace syntax errors. .vue files can be linted adding the `eslint-plugin-html` in your project. If you choose, you can start a project with ESLint enabled by default using `vue-cli`;

### Why?

* Linting component files ensures all developers use the same code style.
* Linting component files helps you trace syntax errors before it's too late.

### How?

To allow linters to extract the scripts from your `*.vue` files, put script inside a `<script>` component and [keep component expressions simple](#keep-component-expressions-simple) (as linters don't understand those). Configure your linter to allow global variables `vue` and component `props`.

#### ESLint

[ESLint](http://eslint.org/) requires an extra [ESLint HTML plugin](https://github.com/BenoitZugmeyer/eslint-plugin-html#eslint-plugin-html) to extract the script from the component files.

Configure ESLint in a `.eslintrc` file (so IDEs can interpret it as well):
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
eslint src/**/*.vue
```

#### JSHint

[JSHint](http://jshint.com/) can parse HTML (using `--extra-ext`) and extract script (using `--extract=auto`).

Configure JSHint in `.jshintrc` file (so IDEs can interpret it as well):
```json
{
  "browser": true,
  "predef": ["opts", "vue"]
}
```

Run JSHint
```bash
jshint --config modules/.jshintrc --extra-ext=html --extract=auto modules/
```
Note: JSHint does not accept `vue` as extension, but only `html`.

[↑ back to Table of Contents](#table-of-contents)


## Create components when needed

### Why?

Vue.js is a component framework based. Not knowing when to create components can lead to issues like:

* If the component is too big, it probably will be hard to (re)use and maintain;
* If the component is too small, your project gets flooded, harder to make components communicate;

### How?

* Always remember to build your components for your project needs, but you should also try to think of them being able to work out of it. If they can work out of your project, such as a library, it makes them a lot more robust and consistent;
* It's always better to build your components as early as possible since it allows you to build your communications (props & events) on existing and stable components.

### Rules

* First, try to build obvious components as early as possible such as: modal, popover, toolbar, menu, header, etc. Overall, components you know for sure you will need later on. On your current page or globally;
* Secondly, on each new development, for a whole page or a portion of it, try to think before rushing in. If you know some parts of it should be a component, build it;
* Lastly, if you're not sure, then don't! Avoid polluting your project with "possibly useful later" components, they might just stand there forever, empty of smartness. Note it's better to break it down as soon as you realize it should have been, to avoid the complexity of compatibility with the rest of the project;

[↑ back to Table of Contents](#table-of-contents)

## Use mixins wherever possible

### Why?

Mixins encapsulate reusable code and avoid duplication. If two components share the same functionality, a mixin can be used. With mixins, you can focus on the individual component task and abstract common code. This helps to better maintain your application.

### How?

Let's say you have a mobile and desktop menu component whose share some functionality. We can abstract the core 
functionalities of both into a mixin like this.

```js
const MenuMixin = {
  data () {
    return {
      language: 'EN'
    }
  },

  methods: {
    changeLanguage () {
      if (this.language === 'DE') this.$set(this, 'language', 'EN')
      if (this.language === 'EN') this.$set(this, 'language', 'DE')
    }
  }
}

export default MenuMixin
```

To use the mixin, simply import it into both components (I only show the mobile component).

```html
<template>
  <ul class="mobile">
    <li @click="changeLanguage">Change language</li>
  </ul>  
</template>

<script>
  import MenuMixin from './MenuMixin'

  export default {
    mixins: [MenuMixin]
  }
</script>
```

[↑ back to Table of Contents](#table-of-contents)

---

## Wanna help?

Fork it and Pull Request what you think it should be good to have or just create an [Issue](https://github.com/pablohpsilva/vuejs-component-style-guide/issues/new).


<!-- ## Add badge to your project

You can use the *Vue.js Style Guide badge* to link to this guide:

[![Vue.js Style Guide badge](https://cdn.rawgit.com/voorhoede/Vue.js-style-guide/master/Vue.js-style-guide.svg)](https://github.com/voorhoede/Vue.js-style-guide)

### Why?

Inform other developers your project is following the Vue.js Style Guide. And let them know where they can find this guide.

### How?

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

[↑ back to Table of Contents](#table-of-contents)

---

## License

[![CC0](http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

[De Voorhoede](https://twitter.com/devoorhoede) waives all rights to this work worldwide under copyright law, including all related and neighboring rights, to the extent allowed by law.

You can copy, modify, distribute and perform the work, even for commercial purposes, all without asking permission.
 -->


## Translation authors

* [Brazilian Portuguese](README-PTBR.md): Pablo Henrique Silva [github:pablohpsilva](https://github.com/pablohpsilva), [twitter: @PabloHPSilva](https://twitter.com/PabloHPSilva)
* [Russian](https://pablohpsilva.github.io/vuejs-component-style-guide/#/russian): Mikhail Kuznetcov [github:shershen08](https://github.com/shershen08), [twitter: @legkoletat](https://twitter.com/legkoletat)
