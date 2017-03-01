Этот документ предалагает для вас ряд правил по разработке компонетов vue.js  которые:

 * потом будет легче для вашей команды (или для вас в будущем) понять или найти что и как работает
 * ваш редактор кода (IDE, среда разработки) поймет с меньшим количеством ошибок и предложит лучшие подсказки
 * улучшит переиспользование в данном проекте и в других ваших проектах
 * лучше кешируются и выделяются в отдельные компоненты

## Содержание

* [Модульная разработка](#module-based-development)
* [Наименование копонентов vue](#vue-component-names)
* [Выражения в компонентах должны быть простыми](#keep-expressions-simple)
* [Оставляйте свойства простыми](#keep-component-props-primitive)
* [Правильно используйте свойства компонента](#harness-your-component-props)
* [Определяйте `this` как `component`](#assign-this-to-component)
* [Структура компонента](#component-structure)
* [Именование событий](#component-event-names)
* [Избегайте `this.$parent`](#avoid-thisparent)
* [Используйте `this.$refs` осторожно](#use-thisrefs-with-caution)
* [Используйте ограниченные стили](#use-component-name-as-style-scope)
* [Документируйте API компонента](#document-your-component-api)
* [Добавляйте демо](#add-a-component-demo)
* [Форматируйте код файлов](#lint-your-component-files)


## Модульная разработка

 Всегда старайтесь чтобы ваше приложение состояло из небольших модулей, каждый из которых умеет выполнять только одну функцию, но делает это хорошо.

 Модуль по определению это небольшая ограниченная часть приложения. "Строительный блок", самодостаночный функционально. Организация Vue.js позволяет создавать подобные модули оринетируясь на визуальные компоненты.

### Почему?

Модули небольших размеров легче взять для использования - понять что они делают, дорабатывать или переиспользовать. И вам и всей вашей команде.

### Как?

Старайтесь чтобы каждый Vue компонент соответствовал принципам [FIRST](https://addyosmani.com/first/): 
 - Решающий одну задачу, 
 - Назависимый, 
 - Переиспользуемый, 
 - Небольшой, 
 - Простой в тестировании.

[↑ наверх](#table-of-contents)


## Наименование компонентов vue

Имя каждого компонента должно соответствовть следующим критериям:

* **Понятное**: в меру детальным, в меру абстрактным
* **Короткое**: не более 2-3 слов
* **Произносимое**: чтобы его можно было упомнянуть в обсуждении

### Почему?

* Имя компонента используется людьми и должно облегчать коммуникацию

### Как?

```html
<!-- правильно -->
<app-header></app-header>
<user-list></user-list>
<range-slider></range-slider>

<!-- неправильно -->
<btn-group></btn-group> <!-- короткое, но произносить - язык сломаешь -->
<ui-slider></ui-slider> <!-- все коменпоненты - так или иначе UI элементы, приставка не нужна -->
<slider></slider> <!--не соответствует спецификации HTML5 -->
```

[↑ наверх](#table-of-contents)


## Выражения в компонентах должны быть простыми

Инлайновые выражение которые вы можете использовать в шаблонах Vue - это самые обычные javascript выражения. Это дает максимальную свободу и мощность, однако изза этого они могут стать слишком сложными. Не злоупотребляйте этим - **оставляйте инлайновые выражения простыми**.

### Почему?

 * Сложные выражения сложнее прочесть и понять
 * Инлайновые выражения нелоьзя переиспользовать, это очевидно ведет дупликации кода и ухуджению его качества.
 * Редакторы и IDE обычно не могут парисить такие выражения а значит у вас не будет автодополнения и валидации.

### Как?


Простое правило - если код инлайнового javascript выражения становится слишком сложным - **выносите его как отдельный метод в блок methods или computed-свойство, соответственно в блок computed**.

```html
<!-- правильно -->
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

<!-- неправильно -->
<template>
	<h1>
		{{ `${(new Date()).getUTCFullYear()}-${('0' + ((new Date()).getUTCMonth()+1)).slice(-2)}` }}
	</h1>
</template>
```

[↑ наверх](#table-of-contents)



## Оставляйте свойства простыми

Хотя Vue и подерживает передачу атрибуто ввиде сложных объектов, старайтесь избегать этого. Старайтесь ограничиться [простыми типами JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) и функциями для этого. Не передавайте сложные объекты в компоненты-наследники.

### Почему?

* Используя для каждого свойства отдельный атрибут - API вашего компонента будет более наглядным
* Такой подход совместим с API к которому мы все привыкли у нативных HTML(5) элементов
* Отдельные атрибуты которые вы создали будет легче понять другим членам команды
* При передаче сложных объектов сразу не видно какие из его свойст далее используются - это затруднит рефакторинг.

### Как?

Используйте отдельные атрибуты для каждой опции и передавайте в нее примитив (флаг, строку, число) или функцию.

```html
<!-- правильно -->
<range-slider
  :values="[10, 20]"
  min="0"
  max="100"
  step="5"
  :on-slide="updateInputs"
  :on-end="updateResults">
</range-slider>

<!-- неправильно -->
<range-slider :config="complexConfigObject"></range-slider>
```

[↑ наверх](#table-of-contents)






## Ограничивайте используйте свойства компонента

В Vue свойства компонента (props) являются отражением его API. Ясное, понятное API делает для других разработчиков проще использовать ваши коменпоненты.

Свойства передаются с использованием специальных атрибутов тега. Этих атрибуты могут быть либо указаны как пустые значения (`:attr`), либо присвоены строкам (`:attr="value"` или `v-bind:attr="value"`). Обратите внимание на подобные возможности при описании свойств.

### Почему?

Грамотное использование свойств обеспечивает что компонент всегда будет отрабатывать без ошибок. Даже если в последствии ваши компоненты будут использоваться так как вы не предполагали изначлаьно.

### Как?

* Используйте свойства по-умолчанию для указания значений свойств
* Используйте свойство `type` для [валидации](http://vuejs.org/v2/guide/components.html#Prop-Validation) значений свойства.
* Всегда проверяйте что свойство определено прежде чем использовать его

```html
<template>
  <input type="range" v-model="value" :max="max" :min="min">
</template>
<script type="text/javascript">
  export default {
    props: {
      max: {
        type: Number, // это обеспечивает проверку что свойство max будет типа Number
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

[↑ наверх](#table-of-contents)


## Определяйте `this` как `component`

В контексте кода компонента Vue `this` всегда означает экземпляр самого компонента. Таким образом если вам понадобится обратиться к ней в другом контексте сделайте так чтобы `this` означало  `component`.

Тоесть, **не используйте** устаревшие конструкции присваивания вроде `const self = this;`. Можно и нужно использовать `component` в Vue компонентах для этого.

### Почему?

* Присваивая `this` к переменной названной `component` напрямую укажет тем кто это будет использовать что это означает сам компонент.

### Как?


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

<!-- неправильно -->
<script type="text/javascript">
export default {
  methods: {
    hello() {
      return 'hello';
    },
    printHello() {
      const self = this; // не нужно
      console.log(self.hello());
    },
  },
};
</script>
```

[↑ наверх](#table-of-contents)


## Структура компонента

Добейтесь чтобы порядок описания компонента было понятно и логично читать.

### Почему?

* 
* 
* 
* 
* 
* 

### Как?

Структура компонента, описание свойст в логичном порядке:


```html
<template lang="html">
	<div class="Ranger__Wrapper">
		<!-- ... -->
	</div>
</template>

<script type="text/javascript">
  export default {
		// обязательно не забываем имя к.
    name: 'RangeSlider',
    // можем использовать композицию уже существующих к.
    extends: {},
    // перечисление свойств и переменных
    props: {
			bar: {}, // еще лучше если по-алфавиту
			foo: {},
			fooBar: {},
		},
    data() {},
    computed: {},
    // когда внутри используются другие к.
    components: {},
    // методы
    watch: {},
    methods: {},
    // методы жизненного цикла
    beforeCreate() {},
    mounted() {},
};
</script>

<style scoped>
  .Ranger__Wrapper { /* ... */ }
</style>
```


[↑ наверх](#table-of-contents)



## Именование событий

Vue

### Почему?

* 
* 

### Как?

* 
* 
* 

[↑ наверх](#table-of-contents)



## Избегайте использования `this.$parent`

### Почему?

* 
* 

### Как?

* 
* 
* 

[↑ наверх](#table-of-contents)



## Используйте `this.$refs` осторожно

### Почему?


* 
* 

### Как?


* 
* 
* 
* 
* 
* 
* 
* 


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


[↑ наверх](#table-of-contents)



## Используйте ограниченные стили

### Почему?

* 
* 


### Как?


```html
<style scoped>
	/* правильно */
	.MyExample { }
	.MyExample li { }
	.MyExample__item { }

	/* неправильно */
	.My-Example { } /* не ограничен именем компонента или модуля, не соответствует спецификации BEM например */
</style>
```



[↑ наверх](#table-of-contents)



## Документируйте API компонента 

### Почему?

* 
* 
* 
* 

### Как?

Add a `README.md` file to the component's module directory:

```
range-slider/
├── range-slider.vue
├── range-slider.less
└── README.md
```

Within the README file, describe the functionality and the usage of the module. For a vue component its most useful to describe the custom attributes it supports as those are its API:


[↑ наверх](#table-of-contents)


## Добавляйте демо

Добавьте `index.html` файл с демо вида компонента в разных конфигурациях показывая как к. может быть использован.

### Почему?

* Наличие демо показывает что модуль работает отдельно
* Демо помогает другим разработчикам посмотреть на результаты - что получится прежде чем разбираться с кодом и/или документацией
* Демо к. исчерпывающе показывает различные варианты конфигурации использования компонента

[↑ наверх](#table-of-contents)


## Форматируйте код ваших файлов

Линтеры улучшают качество код и это помогает отлавливать синтаксические ошибки. `.vue` файлы можно обрабатывать плагином `eslint-plugin-html`. Если вы используете `vue-cli` то ESLint является доступной опцией по-умолчанию.

### Почему?

* Использование линтера обеспечивает что все разработчики читают и пишут код с одинаковыми правилами форматирования
* Использование линтера помогает обнаружить и избежать ошибок раньше чем код файла сохранен.


### Как?

Чтобы линтеры могли выделять JS-код из ваших `*.vue` файлов он должент быть внутри тега `<script></script>`. Также, старатесь сохранять инлайн выражения простыми (см. выше) так как литнет не может их распарсить.

Также настройте линтер что определена глобальная переменная `vue` в секции  `opts`.

#### ESLint

[ESLint](http://eslint.org/) для использования требуется [плагин ESLint HTML](https://github.com/BenoitZugmeyer/eslint-plugin-html#eslint-plugin-html) чтобы экстрагировать JS из стилей .vue компонентов.

Настройки ESLint соханяем в файле `modules/.eslintrc` (так чтобы редактор кода также мог его интерпретировать):
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

Запуск ESLint
```bash
eslint modules/**/*.vue
```

#### JSHint

[JSHint](http://jshint.com/) может парсить HTML (используя `--extra-ext`) и выделять код в script (используя `--extract=auto`).

Настройки JSHint соханяем в файле `modules/.jshintrc` (так чтобы редактор кода также мог его интерпретировать):

```json
{
  "browser": true,
  "predef": ["opts", "vue"]
}
```

Запуск JSHint
```bash
jshint --config modules/.jshintrc --extra-ext=html --extract=auto modules/
```
Внимание: JSHint не работает с `.vue`, только `.html`.

[↑ наверх](#table-of-contents)

---


[↑ наверх](#table-of-contents)

# Хотите поучаствовать?

Сделайте форк этого репозитория и далее предлагайте PR. Или просто создайте [тикет](https://github.com/pablohpsilva/vuejs-component-style-guide/issues).
