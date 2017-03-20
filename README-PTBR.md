# Style Guide para Componentes Vue.js

<p align="center">
  <img src="img/logo.png"/>
</p>

### Traduções
* [Chinês](README-CN.md)
* [Coreano](README-KR.md)
* [Inglês](README.md)
* [Russo](README-RU.md)

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
<script>
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

Enquanto Vue.js suporta passar objetos complexos em JavaScript via esses atributos, você deveria tentar **manter as `props` mais primitivas possíveis**. Tente usar somente [primitivas JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) (strings, numbers, booleans) e functions. Evite objetos complexos.

### Porque?

* Usando um atributo para cada props separadamente, torna o componente com uma API mais expressiva;
* Usando somente primitivas e funções como props, a API do seu componente torna-se similar com a API nativa de elementos HTML(5);
* Usando um atributo para cada prop, outros desenvolvedores podem facilmente entender o que é passado para a instância do componente;
* Quando objetos complexos forem passados, não é aparente quais propriedades e métodos do objetos são realmente usados pelo componente. Isso torna a refatoração mais difícil e pode levar a má práticas.

### Como?

Use um atributo por prop usando primitivas ou funções como valor:

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

Em Vue.js, props são a API dos componentes. Uma API robusta e previsível torna o componente fácil de usar por outros desenvolvedores.

As props dos componentes são passados via atributos HTML. Os valores desses atributos em Vue.js podem ser `:attr="value"` ou `v-bind:attr="value"` ou nunca ser usado. Você deveria **pensar bem nas `props` do seu componente** para permitir casos variados.

### Porque?

Gastar um tempo pensando sobre as props que o seu componente terá, garantirá que o seu componente sempre funcione (defensive programming). Pense neles, especialmente quando considerando o uso por outros desenvolvedores mais tarde que podem ter expectativas diferentes, props que vocês ainda nem pensou que a props do seu componente teria.

### Como?

* Use valores defaults para suas props;
* Use a opção `type` para [validar](http://vuejs.org/v2/guide/components.html#Prop-Validation) valores para um tipo esperado.**[1\*]**;
* Cheque se a prop existe antes de usá-la.

```html
<template>
  <input type="range" v-model="value" :max="max" :min="min">
</template>
<script>
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

Dentro do contexto de Vue.js, o `this` está ligado diretamente a instância do componente. Sendo assim, quando for preciso referenciá-lo em contexto diferente,
garanta que o `this` está disponível como o próprio componente.

Em outras palavras mais resumidas: **NÃO** codifique usando códigos como `const self = this;` mais. Você está salvo para usar o `this` diretamente dentro do componente Vue.

### Porque?

* Usando `this` diretamente, significa para todos os desenvolvedores que ele é o próprio componente, podendo ser usado em todo o seu componente, facilitando o desenvolvimento.

### Como?

```html
<script>
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
<script>
export default {
  methods: {
    hello() {
      return 'hello';
    },
    printHello() {
      const self = this; // Desnecessário
      console.log(self.hello());
    },
  },
};
</script>
```

[↑ voltar para o Índice](#indice)


## Estrutura do componente

Faz com que seja fácil de entender e de seguir uma sequência de pensamentos. Veja o porque logo abaixo.

### Porque?

* Fazendo com que o componente exporte um objeto limpo e bem programado, torna o código mais fácil para desenvolvedores entender e ter um padrão a seguir;
* Ordenando alfabeticamente as props, data, computed, watches e methods faz com que o conteúdo do mesmo seja mais fácil de ser encontrado;
* Agrupando o objeto exportado do componente com visão do que os atributos fazem, torna a leitura mais fácil de ler e mais fácil de ler e mais organizada (name; extends; props, data and computed; components; watch and methods; lifecycle methods, etc.);
* Use o atributo `name`. Usando o [vue devtools](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=en) e este atributo, tornará seu componente mais fácil de desenvolver, debugar e testar;
* Use metodologias de nomenclatura CSS, como [BEM](https://medium.com/tldr-tech/bem-blocks-elements-and-modifiers-6b3b0af9e3ea#.bhnomd7gw), ou [rscss](https://github.com/rstacruz/rscss) - [details?](#use-o-nome-do-componente-como-escopo-para-o-style);
* Use a ordem **template-script-style** na organização dos seus arquivos .vue, como é recomendado pelo Evan You, criador do Vue.js.

### Como?

Estrutura do componente:

```html
<template lang="html">
  <div class="Ranger__Wrapper">
    <!-- ... -->
  </div>
</template>

<script>
  export default {
    // Não se esqueça desse atributo
    name: 'RangeSlider',
    // componha novos componentes
    extends: {},
    // propriedades/variáveis do componente
    props: {
      bar: {}, // Ordenado Alfabeticamente
      foo: {},
      fooBar: {},
    },
    // variáveis
    data() {},
    computed: {},
    // quando um componente usa outros componentes
    components: {},
    // métodos
    watch: {},
    methods: {},
    // Lifecycle do componente
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

Vue.js liga todas as funções e expressões estritamente ligadas ao ViewModel do componente. Cada um dos componentes deve seguir uma boa nomenclatura que evitará problemas durante o desenvolvimento. Continue lendo.

### Porque?

* Desenvolvedores estão livres para usar nome de eventos nativos, e isso pode causar problemas com o tempo;
* A liberdade para nomear eventos podem levar a [incompatibilidade de templates do DOM](https://vuejs.org/v2/guide/components.html#DOM-Template-Parsing-Caveats);

### Como?

* Eventos devem ser nomeados usando kebab-cased;
* Um único evento deve ser emitido para cada ação no seu componente que pode ser interessante como uma API, por eemplo: upload-success, upload-error ou até mesmo dropzone-upload-success, dropzone-upload-error (se você acha que ter um escopo como prefixo seja necessário);
* Eventos devem terminar em verbos e usar a nomes no infinitivo (exemplo, client-api-load) ou usar substantivos (exemplo, drive-upload-success) ([source](https://github.com/GoogleWebComponents/style-guide#events));

[↑ voltar para o Índice](#indice)

## Evite `this.$parent`

Vue.js suporta componentes aninhados, assim como acesso ao contexto do pai deste componente. Acessar escopo fora do componente em desenvolvimento viola a regra de [FIRST](https://addyosmani.com/first/) de [desenvolvimento baseado em componentes](#desenvolvimento-baseado-em-modulo). Sendo assim, você deveria **evitar `this.$parent`**.

### Porque?

* Um componente Vue, como qualquer outro componente, deve funcionar isoladamente. Se um componente precisa acessar seu pai, essa regra é quebrada;
* Se um componente precisa acessar o seu pai, ele não mais pode ser usado em outro contexto/aplicação.

### Como?

* Passe os valores do pai para o componente via props;
* Passe os métodos definidos no componente pao para o componente filho usando callbacks em expressões;
* Emita eventos do componente filho e escute-os no componente pai.

[↑ voltar para o Índice](#indice)

## Use `this.$refs` com cuidado

Componente em Vue.js permite o acesso do contexto de outros componentes e elementos básicos HTML via o atributo `ref`. Este atributo proverá uma forma de acesso via `this.$refs` do contexto de um componente ou elemento DOM. Na maioria dos casos, a necessidade de acessar **o contexto de outros componentes** via `this.$refs` poderia ser evitado. O uso constante dessa tática levará a uma má API, visto que ele poderia ser usado.

### Porque?

* Um componente deve **funcionar isoladamente**. Se um componente não dá suporte a todos os acessos necessários, provavelmente o componente foi mau implementado/desenvolvido;
* Props e eventos devem ser o suficiente para a grande maioria dos componentes.

### Como?

* Cria uma boa API para o componente;
* Sempre foque na proposta que o componente está oferecendo;
* Nunca escreva código muito específico. Se voce precisa escrever um código específico dentro de um componente genérico, significa que a API ainda não está genérica o suficiente ou talvez você precise de um outro componente;
* Cheque todas as props para ver se falta alguma coisa. Se esse for o caso, melhore-o ou crie uma issue com a proposta de melhoria;
* Cheque todos os eventos. Na maioria dos casos, desenvolvedores esquecem a comunicação entre Pai-Filho (eventos);
* **Props down, events up!** Melhore o seu compoennte quando for necessário com uma boa API e tenha isolamento como um objetivo;
* Usando `this.$refs` em componentes, deveria ser usado somente quando props e eventos não podem mais ser usados ou quando faz mais sentido (veja o exemplo abaixo);
* Usando `this.$refs` para acessar elementos DOM (ao invés de usar `jQuery`, `document.getElement*`, `document.queryElement`) é tranquilo, quando o elemento não pode ser manipulado com data binding ou com uma diretiva.

```html
<!-- ótimo, não precisa de ref -->
<range :max="max"
  :min="min"
  @current-value="currentValue"
  :step="1"></range>
```

```html
<!-- ótimo exemplo para se usar this.$refs -->
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
<!-- evite acessar alguma coisa que poderia ser emitida via um evento -->
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

Elementos dos componentes Vue.js são elementos customizados que podem ser usados como style scope root. Alternativamente, o nome do componente pode ser usado como o namespace de uma classe CSS.

### Porque?

* Scoping styles to a component element improves predictability as its prevents styles leaking outside the component element;
* Usar `<style scope>` num componente, melhora a previsibilidade e previne os estilos de vazar fora do componente;
* Usando o mesmo nome para o diretório do módulo, o componente Vue.js e o estilo ficam mais fáceis de serem vistos e entendidos de onde eles surgiram.

### Como?

Use o nome do componente como um prefixo para gerar um namespace. Basei-se em BEM e OOCSS **e** use o atribute `scoped` no `<style>` do seu componente. O uso de `scope` dirá ao
compilador Vue para adicionar uma assinatura a todas as classes que estão no seu `<style>`. Essa assinatura forçará o browser (se houver suporte) será aplicada a todos os estilos
que compõem o seu componente, levanto a um CSS que não sai do contexto do seu componente.

```html
<style scoped>
  /* recomendado */
  .MyExample { }
  .MyExample li { }
  .MyExample__item { }

  /* evite */
  .My-Example { } /* não tem escopo a um componente nem a um nome de um módulo; não segue nenhuma metodologia */
</style>
```

[↑ voltar para o Índice](#indice)


## Documente a API do seu componente

A instância de um componente Vue.js é criada usando um elemento do componente que está dentro da sua aplicação. A instância é configurada através da configuração dos seus atributos. Para que o componente possa ser usado por outros desenvolvedores, esses atributos - a API do seu componente - devem ser documentados em um arquivo `README.md`.

### Porque?

* Documentação provê aos desenvolvedores uma overview de alto nível, sem ter a necessidade de ler todo o código. Isso torna o componente mais acessível e fácil de usar;
* Uma API de um componente é o conjunto de atributos. O interesse dessa API vem para desenvolvedores que querem usar (e não desenvolver) o componente;
* Documentação formaliza a API e diz aos desenvolvedores quais as funcionalidades esperadas;
* `README.md` é o nome de arquivo padrão para a primeira leitura de uma documentação. O serviço de repositório de código (Github, Bitbucket, Gitlab etc) mostram o conteúdo desses componentes, diretamente quando o usuário navega pelos diretórios.

### Como?

Adicione um arquivo `README.md` ao diretório que contém o componente:

```
range-slider/
├── range-slider.vue
├── range-slider.less
└── README.md
```

Dentro do arquivo README, descreva a funcionaldiade e o uso desse módulo. Para um componente Vue.js é muito útil a descrição das props que ele suporta, pois essas compõem a API do componente. Exemplo:


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

Adicione um `index.html` que tenha demonstrações do componente com configurações diferentes, mostrando como o componente deve ser usado.

### Porque?

* Uma demontração do componente prova que o component funciona isoladamente;
* Uma demontração do componente dá aos desenvolvedores uma prévia antes de entrar na documentação ou no código;
* Demonstrações ilustram todas as possíveis configurações e variações que um componente pode ser usado.

[↑ voltar para o Índice](#indice)


## Lint os arquivos do seu componente

Linters melhoram a consistência de código e ajudam a rastrear erros de sintaxe. Arquivos .vue podem usar Linters quando o plugin `eslint-plugin-html` for adicionado ao projeto. Se você quiser, você pode iniciar um projeto com o ESLInt ativado por padrão usando a ferramenta `vue-cli`;

### Porque?

* Arquivos que usam Lint garantem que todos os desenvolvedores usem o mesmo estilo de código;
* Arquivos que usam Lint ajudam no rastreio de erros antes que seja tarde demais.

### Como?

Para fazer com que linters estraiam o script dos seus arquivos `*.vue`, configure o seu linter para acessar variáveis globais `vue` e props de componentes.

#### ESLint

[ESLint](http://eslint.org/) requer um [plugin ESLint HTML](https://github.com/BenoitZugmeyer/eslint-plugin-html#eslint-plugin-html) para extrair o script de um componente.

Configure o ESLint em um arquivo `.eslintrc` (para que IDEs possam interpretá-lo bem):
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

Rode o ESLint
```bash
eslint src/**/*.vue
```

#### JSHint

[JSHint](http://jshint.com/) parsea o HTML (usando `--extra-ext`) e extrai o script (usando `--extract=auto`).

Configure o JSHint em um arquivo `.jshintrc` (para que IDEs possam interpretá-lo bem):
```json
{
  "browser": true,
  "predef": ["opts", "vue"]
}
```

Rode os JSHint
```bash
jshint --config módulos/.jshintrc --extra-ext=html --extract=auto módulos/
```
Nota: JSHint não aceita as expressões `vue`, porém aceita as de `html`.

[↑ voltar para o Índice](#indice)

---

## Quer ajudar?

Faça um Fork do projeto e depois um Pull Request do que você acha que deve ser interessante ter nesse guia ou crie uma [Issue](https://github.com/pablohpsilva/vuejs-component-style-guide/issues/new).

## Sobre o tradutor

Pablo Henrique Penha Silva

- Github [pablohpsilva](https://github.com/pablohpsilva)
- Twitter [@PabloHPSilva](https://twitter.com/PabloHPSilva)
- Medium [@pablohpsilva](https://medium.com/@pablohpsilva)



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
