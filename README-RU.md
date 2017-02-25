Этот документ предалагает для вас ряд правил по разработке компонетов vue.js  которые:

 * потом будет легче для вашей команды (или для вас в будущем) понять или найти что и как работает
 * ваш редактор кода (IDE, среда разработки) поймет с меньшим количеством ошибок и предложит лучшие подсказки
 * улучшит переиспользование в данном проекте и в других ваших проектах
 * лучше кешируются и выделяются в отдельные компоненты

## Содержание

* [Разработка модулями](#module-based-development)
* [Наименование копонентов vue](#vue-component-names)
* [Выражения в компонентах должны быть простыми](#keep-expressions-simple)
* [Оставляйте свойства простыми](#keep-component-props-primitive)
* [Следите за свойствами компонента](#harness-your-component-props)
* [Определяйте `this` как `component`](#assign-this-to-component)
* [Структура компонента](#component-structure)
* [Именование событий](#component-event-names)
* [Избегайте `this.$parent`](#avoid-thisparent)
* [Используйте `this.$refs` осторожно](#use-thisrefs-with-caution)
* [Используйте ограниченные стили](#use-component-name-as-style-scope)
* [Документируйте API компонента](#document-your-component-api)
* [Добавляйте демо](#add-a-component-demo)
* [Форматируйте код файлов](#lint-your-component-files)
