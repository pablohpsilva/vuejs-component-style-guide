# Vue.js Component Style Guide

## Purpose

This guide provides a uniform way to structure your [Vue.js](http://vuejs.org/) code. Making it

* easier for developers / team members to understand and find things.
* easier for IDEs to interpret the code and provide assistance.
* easier to (re)use build tools you already use.
* easier to cache and serve bundles of code separately.

This guide is inspired by the [RiotJS Style Guide](https://github.com/voorhoede/riotjs-style-guide) by De Voorhoede.

<!-- ## Demos

Our [Vue.js demos](https://github.com/voorhoede/Vue.js-demos#Vue.js-demos-) are a companion to this guide, illustrating the guidelines with practical examples. -->

## Table of Contents

* [Module based development](#module-based-development)
* [vue component names](#component-module-names)
* [1 module = 1 directory](#1-module--1-directory)
* [Use `*.vue` extension](#use-vue-extension)
* [Use `<script>` inside component](#use-script-inside-component)
* [Keep component expressions simple](#keep-component-expressions-simple)
* [Keep component options primitive](#keep-component-options-primitive)
* [Harness your component options](#harness-your-component-options)
* [Assign `this` to `component`](#assign-this-to-component)
* [Component structure](#component-structure)
* [Avoid fake ES6 syntax](#avoid-fake-es6-syntax)
* [Avoid `component.parent`](#avoid-componentparent)
* [Use `each ... in` syntax](#use-each--in-syntax)
* [Use component name as style scope](#use-component-name-as-style-scope)
* [Document your component API](#document-your-component-api)
* [Add a component demo](#add-a-component-demo)
* [Lint your component files](#lint-your-component-files)
<!-- * [Add badge to your project](#add-badge-to-your-project) -->


## Module based development

Always construct your app out of small modules which do one thing and do it well. 

A module is a small self-contained part of an application. The Vue.js library is specifically designed to help you create *view-logic modules*.

### Why?

Small modules are easier to learn, understand, maintain, reuse and debug. Both by you and other developers.

### How?

Each vue component (like any module) must be [FIRST](https://addyosmani.com/first/): *Focused* ([single responsibility](http://en.wikipedia.org/wiki/Single_responsibility_principle)), *Independent*, *Reusable*, *Small* and *Testable*.

If your module does too much or gets too big, split it up into smaller modules which each do just on thing.
As a rule of thumb, try to keep each component file less than 100 lines of code.
Also ensure your vue component works in isolation. For instance by adding a stand-alone demo.

[↑ back to Table of Contents](#table-of-contents)


## vue component names

A vue component is a specific type of module. 

Each component name must be:

* **Meaningful**: not overspecific, not overly abstract.
* **Short**: 2 or 3 words.
* **Pronouncable**: we want to be able talk about them.

Vue component names must also be:

* **Custom element spec compliant**: [include a hyphen](https://www.w3.org/TR/custom-elements/#concepts), don't use reserved names.
* **`app-` namespaced**: if very generic and otherwise 1 word, so that it can easily be reused in other projects.

### Why?

* The name is used to communicate about the module. So it must be short, meaningful and pronounceable.
* The `component` element is inserted into the DOM. As such, they need to adhere to the spec.

### How?

```html
<!-- recommended -->
<app-header></app-header>
<user-list></user-list>
<range-slider></range-slider>

<!-- avoid -->
<btn-group></btn-group> <!-- short, but unpronouncable. use `button-group` instead -->
<ui-slider></ui-slider> <!-- all components are ui elements, so is meaningless -->
<slider></slider> <!-- not custom element spec compliant -->
```

[↑ back to Table of Contents](#table-of-contents)


## Use `<script type="text/javascript">` inside component

You should **always use `<script type="text/javascript">`** around scripting.

### Why?

* Improves IDE support (signals how to interpret).
* Tells developers what the script is. Don't go for coffee scripting.

### How?

```html
<!-- recommended -->
<template>
	<h1>The year is {{ this.year }}</h1>
</template>
<script type="text/javascript">
  export default {
    data() {
      return {
        year: 2017,
      };
    },
  };
</script>

<!-- avoid -->
<template>
	<h1>The year is {{ this.year }}</h1>
</template>
<script>
  export default {
    data() { // <-- IDE might not interpret this
      return {
        year: 2017,
      };
    },
  };
</script>
```

[↑ back to Table of Contents](#table-of-contents)


## Keep expressions simple

Vue.js's inline expressions are 100% Javascript. This makes them extremely powerful, but potentially also very complex. Therefore you should **keep expressions simple**.

### Why?

* Complex inline expressions are hard to read.
* Inline expressions can't be reused elsewehere. This can lead to code duplication and code rot.
* IDEs typically don't have support for expression syntax, so your IDE can't autocomplete or validate.

### How?

If it gets too complex or hard to read **move it to methods or computed properties**!

```html
<!-- recommended -->
<template>
	{{ `${year}-${month}` }}
</template>
<script>
  export default {
    computed: {
      month() {
        return twoDigits((new Date()).getUTCMonth() + 1);
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
	{{ `${(new Date()).getUTCFullYear()}-${('0' + ((new Date()).getUTCMonth()+1)).slice(-2)}` }}
</template>
```

[↑ back to Table of Contents](#table-of-contents)


## Keep component options primitive

While Vue.js supports passing complex JavaScript objects via these attributes, you should try to **keep the component options as primitive as possible**. Try to only use [JavaScript primitives](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) (strings, numbers, booleans) and functions. Avoid complex objects.

### Why?

* By using an attribute for each option separately the component has a clear and expressive API.
* By using only primitives and functions as option values our component APIs are similar to the APIs of native HTML(5) elements. Which makes our custom elements directly familiar.
* By using an attribute for each option, other developers can easily understand what is passed to the component instance.
* When passing complex objects it's not apparent which properties and methods of the objects are actually being used by the custom components. This makes it hard to refactor code and can lead to code rot.

### How?

Use a component attribute per option, with a primitive or function as value:

```html
<!-- recommended -->
<range-slider
  :values="[10, 20]"
  min="0"
  max="100"
  step="5"
  :on-slide="updateInputs"
  :on-end="updateResults">
</range-slider>
	
<!-- avoid -->
<range-slider :config="complexConfigObject"></range-slider>
```

[↑ back to Table of Contents](#table-of-contents)


## Harness your component options

In Vue.js your component options are your API. A robust and predictable API makes your components easy to use by other developers.

Component options are passed via custom HTML attributes. The values of these attributes can be Vue.js plain strings (`:attr="value"` or `v-bind:attr="value"`) or missing entirely. You should **harness your component options** to allow for these different cases.

## Why?

Harnessing your component options ensures your component will always function (defensive programming). Even when other developers later use your components in ways you haven't thought of yet.

## How?

* Use defaults for option values.
* Use type conversion to cast option values to expected type.
* Check if option exists before using it.

```html
<template>
  <input type="range" :value="value" :max="max" :min="min">
</template>
<script type="text/javascript">
  export default {
    props: {
      max: {
        type: Number,
        default: 10,
      },
      min: {
        type: Number,
        default: 0,
      },
      value: {
        type: Number,
        default: 4,
      },
    },
  };
</script>
```

[↑ back to Table of Contents](#table-of-contents)


## Assign `this` to `component`

Within the context of a Vue.js component element, `this` is bound to the component instance.
Therefore when you need to reference it in a different context, ensure `this` is available as `component`.

### Why?

* By assigning `this` to a variable named `component` the variable tells developers it's bound to the component instance wherever it's used.

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

* Alphabetizing the properties and methods makes them easy to find.

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
    name: 'Ranger',
    // compose new components
    extends: {},
    // component properties
    props: {},
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

## Avoid `this.$parent`

Vue.js supports nested components which have access to their parent context. Accessing context outside your vue component violates the [FIRST](https://addyosmani.com/first/) rule of [module based development](#module-based-development). Therefore you should **avoid using `this.$parent`**.

### Why?

* A vue component, like any module, must work in isolation. If a component needs to access its parent, this rule is broken.
* If a component needs access to its parent, it can no longer be reused in a different context. 

### How?

* Pass values from the parent to the child component using attribute/properties.
* Pass methods defined on the parent component to the child component using callbacks in attribute expressions.

[↑ back to Table of Contents](#table-of-contents)


## Use component name as style scope

Vue.js component elements are custom elements which can very well be used as style scope root.
Alternatively the module name can be used as CSS class namespace.

### Why?

* Scoping styles to a component element improves predictability as its prevents styles leaking outside the component element.
* Using the same name for the module directory, the Vue.js component and the style root makes it easy for developers to understand they belong together.

### How?

Use the component name as a namespace prefix based on BEM and OOCSS.

```css
/* recommended */
.MyExample { }
.MyExample li { }
.MyExample__item { }

/* avoid */
.My-Example { } /* not scoped to component or module name, not BEM compliant */
```

[↑ back to Table of Contents](#table-of-contents)


## Document your component API

A Vue.js component instance is created by using the component element inside your application. The instance is configured through its custom attributes. For the component to be used by other developers, these custom attributes - the component's API - should be documented in a `README.md` file.

### Why?

* Documentation provides developers with a high level overview to a module, without the need to go through all its code. This makes a module more accessible and easier to use.
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

```markdown
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
```

[↑ back to Table of Contents](#table-of-contents)


## Add a component demo

Add a `index.html` file with demos of the component with different configurations, showing how the component can be used.

### Why?

* A component demo proves the module works in isolation.
* A component demo gives developers a preview before having to dig into the documentation or code.
* Demos can illustrate all the possible configurations and variations a component can be used in. 

[↑ back to Table of Contents](#table-of-contents)


## Lint your component files

Linters improve code consistency and help trace syntax errors. .vue files can be linted adding the `eslint-plugin-html` in your project. If you choose, you can start a project with ESLint enabled by default using `vue-cli`;

### Why?

* Linting component files ensures all developers use the same code style.
* Linting component files helps you trace syntax errors before it's too late.

### How?

To allow linters to extract the scripts from your `*.vue` files, [put script inside a `<script>` component](#use-script-inside-component) and [keep component expressions simple](#keep-component-expressions-simple) (as linters don't understand those). Configure your linter to allow global variables `vue` and component `opts`.

#### ESLint

[ESLint](http://eslint.org/) requires an extra [ESLint HTML plugin](https://github.com/BenoitZugmeyer/eslint-plugin-html#eslint-plugin-html) to extract the script from the component files.

Configure ESLint in `modules/.eslintrc` (so IDEs can interpret it as well):
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
eslint modules/**/*.vue
```

#### JSHint

[JSHint](http://jshint.com/) can parse HTML (using `--extra-ext`) and extract script (using `--extract=auto`). 

Configure JSHint in `modules/.jshintrc` (so IDEs can interpret it as well):
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
