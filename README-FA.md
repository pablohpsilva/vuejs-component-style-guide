# راهنمای سبک نگارش کامپوننت ویو جی اس

<p align="center">
  <img src="https://raw.githubusercontent.com/pablohpsilva/vuejs-component-style-guide/master/img/logo.png"/>
</p>

### ترجمه ها

- [پرتغالی برزیلی](https://pablohpsilva.github.io/vuejs-component-style-guide/#/portuguese)
- [چینی](https://pablohpsilva.github.io/vuejs-component-style-guide/#/chinese)
- [ژاپنی](https://pablohpsilva.github.io/vuejs-component-style-guide/#/japanese)
- [کره ای](https://pablohpsilva.github.io/vuejs-component-style-guide/#/korean)
- [روسی](https://pablohpsilva.github.io/vuejs-component-style-guide/#/russian)
- [فارسی](https://pablohpsilva.github.io/vuejs-component-style-guide/#/farsi)

## هدف

این راهنما یک راه هم شکل برای ساختار دادن به کد [ویو جی اس](http://vuejs.org/) شما است و موارد زیر را ممکن می سازد:

- برای توسعه دهندگان و اعضای تیم فهمیدن و پیدا کردن کد ها آسان تر است.
- برای محیط های یکپارچه توسعه، تفسیر کد و فراهم کردن پشتیبانی آسان تر است.
- استفاده از ابزار های بیلد آسان تر است.
- نهان سازی و ارائه بسته های کد به طور جداگانه آسان تر است.

این راهنما از مخزن [RiotJS Style Guide](https://github.com/voorhoede/riotjs-style-guide) که توسط [De Voorhoede](https://github.com/voorhoede) نوشته شده است الهام گرفته شده است.

## فهرست مطالب

- [توسعه مبتنی بر ماژول](#توسعه-مبتنی-بر-ماژول)
- [نام های کامپوننت ویو](#نام-های-کامپوننت-ویو)
- [کد های کامپوننت را ساده نگه دارید](#کد-های-کامپوننت-را-ساده-نگه-دارید)
- [پراپ های کامپوننت را ساده نگه دارید](#پراپ-های-کامپوننت-را-ساده-نگه-دارید)
- [از پراپ های کامپوننت استفاده بهینه کنید](#از-پراپ-های-کامپوننت-استفاده-بهینه-کنید)
- [`this` را به `کامپوننت` نسبت دهید](#this-را-به-کامپوننت-نسبت-دهید)
- [ساختار کامپوننت](#ساختار-کامپوننت)
- [نام های ایونت کامپوننت](#نام-های-ایونت-کامپوننت)
- [از استفاده کردن از `this.$parent` پرهیز کنید](#avoid-thisparent)
- [از `this.$refs` با احتیاط استفاده کنید](#use-thisrefs-with-caution)
- [از نام کامپوننت به عنوان محدوده style استفاده کنید](#use-component-name-as-style-scope)
- [برای ای پی آی کامپوننت خود مستند بنویسید](#برای-ای-پی-آی-کامپوننت-خود-مستند-بنویسید)
- [دمو کامپوننت را اضافه کنید](#دمو-کامپوننت-را-اضافه-کنید)
- [فایل های کامپوننت خود را لینت کنید](#فایل-های-کامپوننت-خود-را-لینت-کنید)
- [کامپوننت ها را زمانی بسازید که به آن ها نیاز دارید](#کامپوننت-ها-را-زمانی-بسازید-که-به-آن-ها-نیاز-دارید)
  <!-- * [از پسوند `*.vue` استفاده کنید](#از-پسوند-vue-استفاده-کنید) -->
  <!-- * [به پروژه خود نشان اضافه کنید](#به-پروژه-خود-نشان-اضافه-کنید) -->

## توسعه مبتنی بر ماژول

همیشه برنامه خود را با استفاده از ماژول های کوچکی که فقط یک کار و آن کار را هم درست انجام می دهند، بسازید.

ماژول یک بخش کوچک و مستقل از یک برنامه است. کتابخانه ویو جی اس به طور مشخص برای کمک کردن برای ساخت _view-logic ماژول های_ طراحی شده است .

### چرا؟

یادگیری، درک کردن، نگهداری، استفاده مجدد و رفع مشکلات ماژول های کوچک، چه برای خود شما و چه برای توسعه دهندگان دیگر راحت تر است.

###

هر کامپوننت ویو (مثل یک ماژول) باید [در ابتدا](https://addyosmani.com/first/): _دارای یک هدف مشخص_ ([تک وظیفه‌ای](http://en.wikipedia.org/wiki/Single_responsibility_principle))، _مستقل_، _قابل استفاده مجدد_، _کوچک_ و _با قابلیت تست_ باشد.

اگر کامپوننت شما کار های زیادی انجام می دهد و یا خیلی بزرگ شده است آن را به کامپوننت های کوچکتر تقسیم کنید به طوری که هر کدام فقط یه کار را انجام دهند و بر حسب تجربه تلاش کنید که فایل هر کامپوننت کمتر از 100 خط کد باشد.
همچنین مطمئن شوید کامپوننت های شما به صورت مستقل از هم کار می کنند، برای مثال یک دمو مستقل برای آن قرار دهید.

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

## نام های کامپوننت ویو

نام هر کامپوننت باید موارد زیر را شامل شود:

- **پر معنا**: نه بیش از حد دقیق و نه بیش از حد خلاصه شده و انتزاعی باشد.
- **کوتاه**: 2 یا 3 کلمه.
- **قابل تلفظ**: زیرا ما می خواهیم درباره آن حرف بزنیم و اسم آن را به زبان بیاوریم.

همچنین نام هر کامپوننت ویو باید مطابق موارد زیر باشد:

- **مطابق با تعریف عنصر سفارشی**: [شامل خط فاصله باشد](https://html.spec.whatwg.org/multipage/custom-elements.html#valid-custom-element-name), از نام های رزرو شده استفاده نکنید.
- **`app-` namespaced**: نام باید عمومی و از طرف دیگر شامل یک کلمه باشد که بتوان به راحتی از آن در پروژه های دیگر استفاده مجدد کرد.

### چرا؟

- از نام برای ارتباط برقرار کردن با کامپوننت استفاده می شود. پس باید کوتاه، پر معنا و قابل تلفظ باشد.

### با چه روشی؟

```html
<!-- پیشنهادی -->
<app-header></app-header>
<user-list></user-list>
<range-slider></range-slider>

<!-- پرهیز کنید -->
<btn-group></btn-group>

<!-- کوتاه است، اما قابل تلفظ کردن نیست. از مثال زیر به عنوان جایگزین استفاده کنید -->
<!-- `button-group` -->
<ui-slider></ui-slider>
<!-- همه کامپوننت های عضوی از عصنر های رابط کاربری هستند، پس این نام گذاری بی معنی است -->
<slider></slider>
<!-- مطابق با تعریف عنصر سفارشی نیست -->
```

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

## کد های کامپوننت را ساده نگه دارید

کد های خطی ویو جی اس تماما جاوا اسکریپت هستند. و این قضیه آن ها را به شدت قدرتمند می سازد، اما ذاتا پیچیده هستند. بنابرین شما باید **کد های خطی را ساده نگه دارید**.

### چرا؟

- خواندن کد های خطی پیچیده، سخت است.
- کد های خطی پیچیده نمی توانند در جای دیگر مورد استفاده مجدد قرار بگیرند که این می تواند منجر به تکرار و پوسیدگی کدها شود.
- محیط های یکپارچه توسعه، معمولا پشتیبانی برای سینتکس های کد های خطی ندارد، بنابراین نمی توانند به طور خودکار تکمیل یا اعتبار سنجی شوند.

### با چه روشی؟

اگر کد خیلی پیچیده شد و یا خواندن آن سخت شد، **شما می بایست کد را به methods یا computed انتقال دهید**!

```html
<!-- پیشنهادی -->
<template>
  <h1>{{ `${year}-${month}` }}</h1>
</template>
<script type="text/javascript">
  export default {
    computed: {
      month() {
        return this.twoDigits(new Date().getUTCMonth() + 1)
      },
      year() {
        return new Date().getUTCFullYear()
      },
    },
    methods: {
      twoDigits(num) {
        return ('0' + num).slice(-2)
      },
    },
  }
</script>

<!-- پرهیز کنید -->
<template>
  <h1>
    {{ `${(new Date()).getUTCFullYear()}-${('0' + ((new
    Date()).getUTCMonth()+1)).slice(-2)}` }}
  </h1>
</template>
```

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

## پراپ های کامپوننت را ساده نگه دارید

در حالی که ویو جی اس به خاطر ویژگی هایش از ارسال آبجکت های پیچیده جاوا اسکریپتی پشتیبانی می کند شما باید تلاش کنید تا **پراپ های کامپوننت را تا جای ممکن ساده نگه دارید**. تلاش کنید تا فقط از [داده های اولیه جاوا اسکریپت](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) که شامل (رشته ها، اعداد، بولین) و توابع می شوند، استفاده کنید. و از آبجکت های پیچیده پرهیز کنید.

### چرا؟

- زمانی از یک اتریبیوت به طور جداگانه برای هر پراپ استفاده می کنیم، کامپوننت ای پی آی واضح و صریحی دارد;
- با استفاده کردن از داده های اولیه و توابع به عنوان مقادیر پراپ ها، ای پی آی های کامپوننت ما شبیه به ای پی آی های عنصر های بومی اچ تی ام ال 5 می شود;
- با استفاده کردن از اتریبیوت ها به ازای هر پراپ، بقیه توسعه دهندگان به راحتی می توانند بفهمند که چه چیز هایی به نمونه کامپوننت ارسال شده است.
- زمانی که آبجکت های پیچیده را ارسال می کنید، واضح نیست که چه ویژگی ها و متود هایی از آبجکت واقعا مورد استفاده کامپوننت های سفارشی قرار گرفته است. این باعث می شود ریفکتور کردن کد سخت شود و کد ها به سمت پوسیده شدن بروند.

### با چه روشی؟

به ازای هر اتریبیوت کامپوننت، از پراپ ها استفاده کنید، که مقدارشان داده اولیه یا تابع باشد:

```html
<!-- پیشنهادی -->
<range-slider
  :values="[10, 20]"
  :min="0"
  :max="100"
  :step="5"
  @on-slide="updateInputs"
  @on-end="updateResults"
>
</range-slider>

<!-- پرهیز کنید -->
<range-slider :config="complexConfigObject"></range-slider>
```

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

## از پراپ های کامپوننت استفاده بهینه کنید

در ویو جی اس پراپ های کامپوننت ای پی آی شما هستند. یک ای پی آی منسجم و قابل پیش بینی، استفاده از کامپوننت شما را برای بقیه توسعه دهندگان راحت می کند

پراپ های کامپوننت از طریق اتریبیوت های اچ تی ام ال ارسال می شوند. مقادیر این اتریبیوت ها می تواند رشته های ساده ویو جی است (`:attr="value"` or `v-bind:attr="value"`) باشد یا به طور کامل صرف نظر شود. شما باید **پراپ های کامپوننت کنترل کنید** تا بتوانید سناریو های مختلف را اجازه دهید.

### چرا؟

کنترل کردن پراپ های کامپوننت به شما اطمینان می دهند کامپوننت شما همیشه به درستی کار کند (برنامه نویسی تدافعی). حتی اگر وقتی بقیه توسعه دهندگان، زمانی دیگر از کاموپوننت های شما به طریقی استفاده کنند که شما فکرش را نکرده بودید.

### با چه روشی؟

- از پیش فرض ها برای مقادیر پراپ ها استفاده کنید
- از گزینه `type` برای [اعتبارسنجی](http://vuejs.org/v2/guide/components.html#Prop-Validation) مقادیر به منظور دریافت نوع مورد انتظار.**[1\*]**
- قبل از استفاده کردن از پراپ ها، چک کنید تا ببینید پراپ ها وجود داشته باشند.

```html
<template>
  <input type="range" v-model="value" :max="max" :min="min" />
</template>
<script type="text/javascript">
  export default {
    props: {
      max: {
        type: Number, // [1*] این خط باعث می شود که نوع این پراپ حتما عدد باشد.
        default() {
          return 10
        },
      },
      min: {
        type: Number,
        default() {
          return 0
        },
      },
      value: {
        type: Number,
        default() {
          return 4
        },
      },
    },
  }
</script>
```

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

## `this` را به `کامپوننت` نسبت دهید

در داخل فضای عنصر کامپوننت ویو جی اس، `this` به نمونه کامپوننت اشاره می کند.
بنابراین زمانی که شما نیاز دارید در فضای دیگری ارجاع دهید، اطمینان پیدا کنید که `this` به عنوان `کامپوننت` در دسترس باشد.

به عبارت دیگر: اگر میخواهید از استاندارد **ES6** استفاده کنید، چیز هایی شبیه `var self = this;` را دیگر **ننویسید**. شما با این کار، از کامپوننت های ویو به صورت ایمن استفاده می کنید.

### چرا؟

- با استفاده از در استاندارد ES6، نیازی نیست `this` را در یک متغیر ذخیره کنیدک
  -در کل، زمانی که از توابع arrow استفاده می کنیم اسکوپ لکسیکال حفظ می شود.
- اگر شما از استاندارد ES6 استفاده **نمی کنید**، بنابراین از توابع Arrow هم استفاده نمی کنید شما می بایست `this` را به یک متغیر نسبت دهید. این تنها حالت استثنا می باشد.

### با چه روشی؟

```html
<script type="text/javascript">
  export default {
    methods: {
      hello() {
        return 'hello'
      },
      printHello() {
        console.log(this.hello())
      },
    },
  }
</script>

<!-- پرهیز کنید -->
<script type="text/javascript">
  export default {
    methods: {
      hello() {
        return 'hello'
      },
      printHello() {
        const self = this // غیر ضروری
        console.log(self.hello())
      },
    },
  }
</script>
```

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

## ساختار کامپوننت

این دستورالعمل ها را دنبال کنید تا چگونگی آن را دریابید.

### چرا؟

- داشتن خروجی کامپوننت منجر به یک آبجکت گروه بندی شده و تمیز می شود، که باعث می شود خواندن کد آسان شود و توسعه دهندگان کد استانداردی را داشته باشند.
- چینش properties، data، computed، watches، و methods بر اساس حروف الفبا باعث می شود پیدا کردن آن ها آسان تر شود.
- دوباره می گوییم، گروه بندی کردن باعث می شود خوانایی کامپوننت بیشتر شود. (name; extends; props، data و computed; components; watch و methods; lifecycle methods، و غیره.);
- Use the `name` attribute. Using [vue devtools](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=en) and that attribute will make your development/testing easier;
- از متودولوژی های نامگذاری سی اس اس استفاده کنید ، مانند [BEM](https://medium.com/tldr-tech/bem-blocks-elements-and-modifiers-6b3b0af9e3ea#.bhnomd7gw), یا [rscss](https://github.com/rstacruz/rscss) - [details?](#use-component-name-as-style-scope);
- از ساختار فایل .vue و template-script-style استفاده کنید, همان طور که توسط Evan You خالق ویو پیشنهاد شده است.

### با چه روشی؟

ساختار کامپوننت:

```html
<template lang="html">
  <div class="RangeSlider__Wrapper">
    <!-- ... -->
  </div>
</template>

<script type="text/javascript">
  export default {
    // این دوست کوچک مان (نام کامپوننت) را فراموش نکنید
    name: 'RangeSlider',
    // قابلیت های مشترک را در این ویژگی کامپوننت بنویسید
    mixins: [],
    // کامپوننت جدیدی را با این کامپوننت ترکیب کنید
    extends: {},
    // متغیر ها و ویژگی ها
    props: {
      bar: {}, // به ترتیب حروف الفبای انگلیسی، پراپ ها را بنویسید
      foo: {},
      fooBar: {},
    },
    // متغیر ها
    data() {},
    computed: {},
    // زمانی کامپوننت از کامپوننت های دیگر استفاده می کند
    components: {},
    // متود ها
    watch: {},
    methods: {},
    // قلاب های چرخه حیات کامپوننت
    beforeCreate() {},
    mounted() {},
  }
</script>

<style scoped>
  .RangeSlider__Wrapper {
    /* ... */
  }
</style>
```

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

## Component event names

Vue.js provides all Vue handler functions and expressions are strictly bound to the ViewModel. Each component events should follow a good naming style that will avoid issues during the development. See the **Why** below.

### چرا؟

- Developers are free to use native likes event names and it can cause confusion down the line;
- آزادی برای نام گذاری ایونت ها میتواند به مورد روبهرو منجر شود [DOM ناسازگاری الگو های](https://vuejs.org/v2/guide/components.html#DOM-Template-Parsing-Caveats);

### با چه روشی؟

- نام ایونت ها باید kebab-cased باشد;
- A unique event name should be fired for unique actions in your component that will be of interest to the outside world, like: upload-success, upload-error or even dropzone-upload-success, dropzone-upload-error (if you see the need for having a scoped prefix);
- Events should either end in verbs in the infinitive form (e.g. client-api-load) or nouns (e.g drive-upload-success) ([source](https://github.com/GoogleWebComponents/style-guide#events));

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

## Avoid `this.$parent`

Vue.js supports nested components which have access to their parent context. Accessing context outside your vue component violates the [FIRST](https://addyosmani.com/first/) rule of [component based development](#module-based-development). Therefore you should **avoid using `this.$parent`**.

### چرا؟

- A vue component, like any component, must work in isolation. If a component needs to access its parent, this rule is broken.
- If a component needs access to its parent, it can no longer be reused in a different context.

### با چه روشی؟

- Pass values from the parent to the child component using attribute/properties.
- Pass methods defined on the parent component to the child component using callbacks in attribute expressions.
- Emit events from child components and catch it on parent component.

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

## Use `this.$refs` with caution

Vue.js supports components to have access to other components and basic HTML elements context via `ref` attribute. That attribute will provide an accessible way through `this.$refs` to a component or DOM element context. In most cases, the need to access **other components** context via `this.$refs` could be avoided. This is why you should be careful when using it to avoid wrong component APIs.

### چرا؟

- A vue component, like any component, **must work in isolation**. If a component does not support all the access needed, it was badly designed/implemented.
- Properties and events should be sufficient to most of your components.

### با چه روشی؟

- Create a good component API.
- Always focus on the component purpose out of the box.
- Never write specific code. If you need to write specific code inside a generic component, it means its API isn't generic enough or maybe you need a new component to manage other cases.
- Check all the props to see if something is missing. If it is, create an issue or enhance the component yourself.
- Check all the events. In most cases developers forget that Child-Parent communication (events) is needed, that's why they only remember the Parent-Child communication (using props).
- **Props down, events up!** Upgrade your component when requested with a good API and isolation as goals.
- Using `this.$refs` on components should be used when props and events are exhausted and having it makes sense (see the example below).
- Using `this.$refs` to access DOM elements (instead of doing `jQuery`, `document.getElement*`, `document.queryElement`) is just fine, when the element can't be manipulated with data bindings or for a directive.

```html
<!-- good, no need for ref -->
<range :max="max" :min="min" @current-value="currentValue" :step="1"></range>
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
      }
    },
    methods: {
      open() {
        this.active = true
      },
      hide() {
        this.active = false
      },
    },
    // ...
  }
</script>
```

```html
<!-- از دسترسی به چیزی که می تواند امیت شود پرهیز کنید -->
<template>
  <range :max="max" :min="min" ref="range" :step="1"></range>
</template>

<script>
  export default {
    // ...
    methods: {
      getRangeCurrentValue() {
        return this.$refs.range.currentValue
      },
    },
    // ...
  }
</script>
```

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

## Use component name as style scope

Vue.js component elements are custom elements which can very well be used as style scope root.
Alternatively the component name can be used as CSS class namespace.

### چرا؟

- Scoping styles to a component element improves predictability as its prevents styles leaking outside the component element.
- Using the same name for the module directory, the Vue.js component and the style root makes it easy for developers to understand they belong together.

### با چه روشی؟

Use the component name as a namespace prefix based on BEM and OOCSS **and** use the `scoped` attribute on your style class.
The use of `scoped` will tell your Vue compiler to add a signature on every class that your `<style>` have. That signature will force your browser (if it supports) to apply your components
CSS on all tags that compose your component, leading to a no leaking css styling.

```html
<style scoped>
  /* پیشنهادی */
  .MyExample {
  }
  .MyExample li {
  }
  .MyExample__item {
  }

  /* پرهیز کنید */
  .My-Example {
  } /* not scoped to component or module name, not BEM compliant */
</style>
```

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

## برای ای پی آی کامپوننت خود مستند بنویسید

A Vue.js component instance is created by using the component element inside your application. The instance is configured through its custom attributes. For the component to be used by other developers, these custom attributes - the component's API - should be documented in a `README.md` file.

### چرا؟

- Documentation provides developers with a high level overview to a component, without the need to go through all its code. This makes a component more accessible and easier to use.
- A component's API is the set of custom attributes through which its configured. Therefore these are especially of interest to other developers which only want to consume (and not develop) the component.
- Documentation formalises the API and tells developers which functionality to keep backwards compatible when modifying the component's code.
- `README.md` is the de facto standard filename for documentation to be read first. Code repository hosting services (Github, Bitbucket, Gitlab etc) display the contents of the the README's, directly when browsing through source directories. This applies to our module directories as well.

### با چه روشی؟

Add a `README.md` file to the component's module directory:

```
range-slider/
├── range-slider.vue
├── range-slider.less
└── README.md
```

Within the README file, describe the functionality and the usage of the module. For a vue component its most useful to describe the custom attributes it supports as those are its API:

# Range slider

## قابلیت

The range slider lets the user to set a numeric range by dragging a handle on a slider rail for both the start and end value.

This module uses the [noUiSlider](http://refreshless.com/nouislider/) for cross browser and touch support.

## نحوه استفاده

`<range-slider>` supports the following custom component attributes:

| attribute  | type               | description                                                                                                                                                                                                                                  |
| ---------- | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `min`      | Number             | number where range starts (lower limit).                                                                                                                                                                                                     |
| `max`      | Number             | Number where range ends (upper limit).                                                                                                                                                                                                       |
| `values`   | Number[] _اختیاری_ | Array containing start and end value. E.g. `values="[10, 20]"`. Defaults to `[opts.min, opts.max]`.                                                                                                                                          |
| `step`     | Number _اختیاری_   | Number to increment / decrement values by. Defaults to 1.                                                                                                                                                                                    |
| `on-slide` | Function _اختیاری_ | Function called with `(values, HANDLE)` while a user drags the start (`HANDLE == 0`) or end (`HANDLE == 1`) handle. E.g. `on-slide={ updateInputs }`, with `component.updateInputs = (values, HANDLE) => { const value = values[HANDLE]; }`. |
| `on-end`   | Function _اختیاری_ | Function called with `(values, HANDLE)` when user stops dragging a handle.                                                                                                                                                                   |

For customising the slider appearance see the [Styling section in the noUiSlider docs](http://refreshless.com/nouislider/more/#section-styling).

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

## دمو کامپوننت را اضافه کنید

Add a `index.html` file with demos of the component with different configurations, showing how the component can be used.

### چرا؟

- A component demo proves the component works in isolation.
- A component demo gives developers a preview before having to dig into the documentation or code.
- Demos can illustrate all the possible configurations and variations a component can be used in.

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

## فایل های کامپوننت خود را لینت کنید

Linters improve code consistency and help trace syntax errors. .vue files can be linted adding the `eslint-plugin-html` in your project. If you choose, you can start a project with ESLint enabled by default using `vue-cli`;

### چرا؟

- لینت کردن فایل های کامپوننت این اطمینان را می دهد همه توسعه دهندگان از یک استایل کدی مشابه استفاده کنند
- لینت کردن فایل ها کمک می کند که ارور های سینتکسی را قبل از اینکه به مشکل بخورید پیدا کنید

### با چه روشی؟

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

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

## کامپوننت ها را زمانی بسازید که به آن ها نیاز دارید

### چرا؟

Vue.js is a component framework based. Not knowing when to create components can lead to issues like:

- If the component is too big, it probably will be hard to (re)use and maintain;
- If the component is too small, your project gets flooded, harder to make components communicate;

### با چه روشی؟

- Always remember to build your components for your project needs, but you should also try to think of them being able to work out of it. If they can work out of your project, such as a library, it makes them a lot more robust and consistent;
- It's always better to build your components as early as possible since it allows you to build your communications (props & events) on existing and stable components.

### قوانین

- First, try to build obvious components as early as possible such as: modal, popover, toolbar, menu, header, etc. Overall, components you know for sure you will need later on. On your current page or globally;
- Secondly, on each new development, for a whole page or a portion of it, try to think before rushing in. If you know some parts of it should be a component, build it;
- Lastly, if you're not sure, then don't! Avoid polluting your project with "possibly useful later" components, they might just stand there forever, empty of smartness. Note it's better to break it down as soon as you realize it should have been, to avoid the complexity of compatibility with the rest of the project;

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

## از mixins هر زمانی که ممکن بود استفاده کنید

### چرا؟

Mixins encapsulate reusable code and avoid duplication. If two components share the same functionality, a mixin can be used. With mixins, you can focus on the individual component task and abstract common code. This helps to better maintain your application.

### با چه روشی؟

Let's say you have a mobile and desktop menu component whose share some functionality. We can abstract the core
functionalities of both into a mixin like this.

```js
const MenuMixin = {
  data() {
    return {
      language: 'EN',
    }
  },

  methods: {
    changeLanguage() {
      if (this.language === 'DE') this.$set(this, 'language', 'EN')
      if (this.language === 'EN') this.$set(this, 'language', 'DE')
    },
  },
}

export default MenuMixin
```

برای استفاده از میکسین، به سادگی آن را در داخل دو کامپوننت خود ایمپورت کنید (من فقط کامپوننت موبایل را نشان می دهم)

```html
<template>
  <ul class="mobile">
    <li @click="changeLanguage">تغییر زبان</li>
  </ul>
</template>

<script>
  import MenuMixin from './MenuMixin'

  export default {
    mixins: [MenuMixin],
  }
</script>
```

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

---

## می خواهید کمک کنید?

فورک کنید و برای چیزی که فکر می کنید بهتر است بیان شود پول ریکوئست دهید یا یک [Issue](https://github.com/pablohpsilva/vuejs-component-style-guide/issues/new) بسازید.

<!-- ## Add badge to your project

You can use the *Vue.js Style Guide badge* to link to this guide:

[![Vue.js Style Guide badge](https://cdn.rawgit.com/voorhoede/Vue.js-style-guide/master/Vue.js-style-guide.svg)](https://github.com/voorhoede/Vue.js-style-guide)

### چرا؟

Inform other developers your project is following the Vue.js Style Guide. And let them know where they can find this guide.

### با چه روشی؟

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

[↑ برگشت به فهرست مطالب](#فهرست-مطالب)

---

## License

[![CC0](http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

[De Voorhoede](https://twitter.com/devoorhoede) waives all rights to this work worldwide under copyright law, including all related and neighboring rights, to the extent allowed by law.

You can copy, modify, distribute and perform the work, even for commercial purposes, all without asking permission.
 -->

## مترجم ها

- [پرتغالی برزیلی](README-PTBR.md): Pablo Henrique Silva [github:pablohpsilva](https://github.com/pablohpsilva), [twitter: @PabloHPSilva](https://twitter.com/PabloHPSilva)
- [روسی](https://pablohpsilva.github.io/vuejs-component-style-guide/#/russian): Mikhail Kuznetcov [github:shershen08](https://github.com/shershen08), [twitter: @legkoletat](https://twitter.com/legkoletat)
- [فارسی](README-FA.md): Alireza Arabshahi [github:AlirezaArabshahi](https://github.com/AlirezaArabshahi), [twitter: @iialireza7](https://twitter.com/iialireza7)
