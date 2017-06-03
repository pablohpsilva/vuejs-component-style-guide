# Vue.js 컴포넌트 스타일 가이드

<p align="center">
  <img src="https://raw.githubusercontent.com/pablohpsilva/vuejs-component-style-guide/master/img/logo.png"/>
</p>

### 번역
* [브라질 포르투갈어](https://pablohpsilva.github.io/vuejs-component-style-guide/#/portuguese)
* [중국어](https://pablohpsilva.github.io/vuejs-component-style-guide/#/chinese)
* [일본어](https://pablohpsilva.github.io/vuejs-component-style-guide/#/japanese)
* [영어](https://pablohpsilva.github.io/vuejs-component-style-guide/#/)
* [러시아어](https://pablohpsilva.github.io/vuejs-component-style-guide/#/russian)

## 목적

이 가이드는 [Vue.js](http://vuejs.org/) 코드를 다음과 같이 체계적으로 구성하는 방법을 제공합니다.

* 개발자 혹은 팀원이 쉽게 이해하고 찾을 수 있습니다.
* IDE가 쉽게 코드를 해석하고 도움을 줄 수 있습니다.
* 이미 사용하고 있는 빌드 툴을 재사용하기 쉽게 사용할 수 있습니다.
* 쉽게 캐싱이 가능하고 코드를 분리할 수 있습니다.

이 가이드는 [De voorhoede](https://github.com/voorhoede)가 작성한 [RiotJS 스타일 가이드](https://github.com/voorhoede/riotjs-style-guide)에서 영감을 얻었습니다.

## 목차

* [모듈 기반 개발](#모듈-기반-개발)
* [Vue 컴포넌트 이름](#vue-컴포넌트-이름)
  <!-- * [Use `*.vue` extension](#use-vue-extension) -->
* [컴포넌트 표현식을 단순하게 사용하기](#컴포넌트-표현식을-단순하게-사용하기)
* [컴포넌트 props를 원시 자료형으로 사용하기](#컴포넌트-props를-원시-자료형으로-사용하기)
* [컴포넌트 props를 잘 사용하기](#컴포넌트-props를-잘-사용하기)
* [`this`를 `component`에 지정하기](#this를-component에-지정하기)
* [컴포넌트 구조](#컴포넌트-구조)
* [컴포넌트 이벤트 이름](#컴포넌트-이벤트-이름)
* [`this.$parent` 피하기](#thisparent-피하기)
* [`this.$refs`를 주의하여 사용하기](#thisrefs를-주의하여-사용하기)
* [범위 스타일에 컴포넌트 이름 사용하기](#범위-스타일에-컴포넌트-이름-사용하기)
* [컴포넌트 API를 문서화 하기](#컴포넌트-api를-문서화-하기)
* [컴포넌트 예제 추가하기](#컴포넌트-예제-추가하기)
* [컴포넌트 파일을 Lint하기](#컴포넌트-파일을-lint하기)
  <!-- * [Add badge to your project](#add-badge-to-your-project) -->


## 모듈 기반 개발

항상 한 가지 일을 잘 수행하는 작은 모듈들로 당신의 어플리케이션을 구성하는 것이 좋습니다.

모듈은 어플리케이션의 독립적인 한 부분 입니다. 여기서 Vue.js는 *뷰-로직 모듈*을 작성할 수 있도록 특별히 디자인된 라이브러리입니다.

### 왜 그렇게 하나요?

작은 모듈은 배우고, 이해하고, 유지하고, 재사용과 디버그하는 것이 더 쉽습니다. 당신이 다른 개발자와 협업하는 경우에도요.

### 어떻게 하나요?

각 Vue 컴포넌트(모듈과 같습니다)는 [FIRST](https://addyosmani.com/first/)여야 합니다: *Focused* ([단일 책임 원칙](http://en.wikipedia.org/wiki/Single_responsibility_principle)), *Independent(독립적인)*, *Reusable(재사용 가능한)*, *Small(작은)*, *Testable(테스트 가능한)*.

컴포넌트가 너무 많거나 너무 커지면 한 가지 일을 하는 작은 컴포넌트로 나눠서 구성합니다. 일반적으로 각 컴포넌트 파일을 100라인 미만으로 유지하는 것이 좋습니다. 또한, Vue 컴포넌트가 독립적으로 작동하는지 확인하세요. 예를 들면 독립 실행이 가능한 데모를 만드는 것입니다.

[↑ 목차로 돌아가기](#목차)

## Vue 컴포넌트 이름

각 컴포넌트의 이름은 다음과 같아야합니다.

* **의미있는**: 지나치게 추상적이거나 구체적이면 안됩니다.
* **짧은**: 2~3단어여야 합니다.
* **발음 가능한**: 우리는 컴포넌트에 대해 이야기할 수 있어야합니다.

또한, Vue 컴포넌트 이름은 다음과 같아야합니다.

* **사용자 정의 엘리먼트 스펙을 준수해야 합니다**: [하이픈(-)을 포함하고](https://www.w3.org/TR/custom-elements/#concepts), 예약어를 사용하면 안됩니다.
* **`app-` 네임스페이스**: 매우 일반적이고 한 단어라면 다른 프로젝트에서 쉽게 재사용할 수 있습니다.

### 왜 그렇게 하나요?

* 이름은 컴포넌트끼리 통신하는 데 사용됩니다. 그래서 컴포넌트 이름은 짧고, 의미있으면서 발음 할 수 있어야합니다.

### 어떻게 하나요?

```html
<!-- 권장합니다 -->
<app-header></app-header>
<user-list></user-list>
<range-slider></range-slider>

<!-- 피하세요! -->
<btn-group></btn-group> <!-- 짧지만 발음할 수 없습니다. `button-group`으로 사용하는 것이 좋습니다. -->
<ui-slider></ui-slider> <!-- 모든 컴포넌트는 UI 엘리먼트이기 때문에 의미가 없습니다. -->
<slider></slider> <!-- 사용자 정의 엘리먼트 스펙을 준수하지 않았습니다. -->
```

[↑ 목차로 돌아가기](#목차)

## 컴포넌트 표현식을 단순하게 사용하기

Vue.js의 인라인 표현식은 100% 자바스크립트입니다. 이 기능은 매우 강력하지만 잘못 사용하면 매우 복잡해질 수 있습니다. 따라서 **표현식을 단순하게 사용**해야 합니다.

### 왜 그렇게 하나요?

* 복잡한 인라인 표현식은 읽기 어렵습니다.
* 인라인 표현식은 다른 곳에서 재사용할 수 없습니다. 그렇기 때문에 중복 코드가 생길 수 있습니다.
* IDE는 대부분 표현식 문법을 지원하지 않기 때문에 IDE에서 자동완성 혹은 유효성 검사를 할 수 없습니다.

### 어떻게 하나요?

만약 표현식이 너무 복잡하다면 **methods 혹은 computed 속성으로 옮기세요!**

```html
<!-- 권장합니다 -->
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

<!-- 피하세요! -->
<template>
  <h1>
    {{ `${(new Date()).getUTCFullYear()}-${('0' + ((new Date()).getUTCMonth()+1)).slice(-2)}` }}
  </h1>
</template>
```

[↑ 목차로 돌아가기](#목차)


## 컴포넌트 props를 원시 자료형으로 사용하기

Vue.js는 다양한 속성을 통해 복잡한 JavaScript 객체를 전달할 수 있지만 **컴포넌트 옵션을 가능한 원시 자료형으로 유지하는 것이 좋습니다**. 즉, [JavaScript 원시 값](https://developer.mozilla.org/ko/docs/Glossary/Primitive) (string, number, boolean) 및 함수만을 사용하고 복잡한 Object는 피하는게 좋습니다.

### 왜 그렇게 하나요?

* 개별적으로 `prop` 속성을 사용함으로써 컴포넌트는 명확한 API를 가집니다.
* 원시 자료형과 함수만을 `props` 값으로 사용하면 컴포넌트 API가 네이티브 HTML(5) 엘리먼트의 API와 유사해집니다.
* 개별적으로 `prop` 속성을 사용하면 다른 개발자가 컴포넌트 인스턴스로 전달되는 내용을 쉽게 이해할 수 있습니다.
* 복잡한 Object를 전달할 때 전달한 Object의 속성과 메서드가 실제로 사용자 정의 컴포넌트에서 사용되는지 분명하지 않습니다. 이렇게 사용하면 코드를 리팩토링하기가 어려워지고 코드가 더러워 질 수 있습니다.

### 어떻게 하나요?

원시 자료형 혹은 함수를 사용하여 `props` 컴포넌트 속성을 사용하세요.

```html
<!-- 권장합니다 -->
<range-slider
  :values="[10, 20]"
  min="0"
  max="100"
  step="5"
  :on-slide="updateInputs"
  :on-end="updateResults">
</range-slider>

<!-- 피하세요! -->
<range-slider :config="complexConfigObject"></range-slider>
```

[↑ 목차로 돌아가기](#목차)


## 컴포넌트 props를 잘 사용하기

Vue.js에서는 컴포넌트 props가 당신의 API 입니다. 강력하고 예측가능한 API를 만들면 다른 개발자가 컴포넌트를 쉽게 사용할 수 있습니다.

컴포넌트 props는 사용자 정의 HTML 어트리뷰트에 의해 전달됩니다. 이러한 어트리뷰트의 값은 Vue.js 일반 문자열(`:attr="value"` 또는 `v-bind:attr="value"`)이거나 누락됩니다. 당신은 이러한 경우를 고려하여 **당신의 컴포넌트 props를 잘 사용해야 합니다**.

### 왜 그렇게 하나요?

컴포넌트 props를 잘 사용하면 당신의 컴포넌트는 항상 작동할 것 입니다(방어적 프로그래밍). 나중에 다른 개발자가 당신이 생각하지 못한 방법으로 컴포넌트를 사용하더라도 마찬가지 입니다.

### 어떻게 하나요?

* props에 기본 값을 사용하세요.
* 값을 [validate](https://kr.vuejs.org/v2/guide/components.html#Prop-검증) 하기위해 `type` 옵션을 사용하세요. **[1\*]**
* 중복된 `props`가 있는지 확인하세요.

```html
<template>
  <input type="range" v-model="value" :max="max" :min="min">
</template>
<script type="text/javascript">
  export default {
    props: {
      max: {
        type: Number, // [1*] 'max' prop은 Number로만 사용할 수 있습니다.
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

[↑ 목차로 돌아가기](#목차)


## `this`를 `component`에 지정하기

Vue.js 컴포넌트 엘리먼트의 컨텍스트 내에서 `this`는 컴포넌트 인스턴스에 바인딩됩니다.
그러므로 다른 컨텍스트에서 그것을 참조할 필요가 있을 때 `this`를 `component`로써 사용 가능합니다.

다른 말로하면 더이상 `const self = this;`와 같은 코드를 **사용하지 마세요**. Vue 컴포넌트를 사용하면 안전합니다.

### 왜 그렇게 하나요?

* `this`를 `component`라는 변수에 지정됨으로써 개발자에게 변수가 사용되는 곳마다 컴포넌트 인스턴스에 바인딩되었다는 것을 알려줍니다.

### 어떻게 하나요?

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

<!-- 피하세요! -->
<script type="text/javascript">
export default {
  methods: {
    hello() {
      return 'hello';
    },
    printHello() {
      const self = this; // 불필요합니다.
      console.log(self.hello());
    },
  },
};
</script>
```

[↑ 목차로 돌아가기](#목차)


## 컴포넌트 구조

생각의 순서를 따라 컴포넌트 구조를 만들어보세요. `어떻게 하나요?`를 참고하세요.

### 왜 그렇게 하나요?

* 컴포넌트를 명확하고 그룹화하여 `export`하면 개발자가 쉽게 표준적인 코드에 대해서 이해하고 코드를 작성할 수 있습니다.
* 속성, 데이터, 계산된 속성, 감시자 및 메서드를 알파벳순으로 표시하면 쉽게 찾을 수 있습니다.
* 다시말해, 그룹화하면 컴포넌트를 더 쉽게 읽을 수 있습니다. (name; extends; props; data와 computed; components; watch와 method; 라이프사이클 메서드 등)
* `name` 속성을 사용하세요. [vue devtools](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=en)과 그 속성을 사용하면 개발/테스트가 더 쉬워집니다.
* [BEM](https://medium.com/tldr-tech/bem-blocks-elements-and-modifiers-6b3b0af9e3ea#.bhnomd7gw) 또는 [rscss](https://github.com/rstacruz/rscss)와 같은 CSS 네이밍 규칙을 사용하세요. - [자세히 보기](#범위-스타일에-컴포넌트-이름-사용하기)
* Vue.js 작성자(Evan You)가 권장하는 템플릿 스크립트 스타일인 .vue 파일을 사용하십시오.

### 어떻게 하나요?

아래 컴포넌트 구조를 참고하세요.

```html
<template lang="html">
  <div class="Ranger__Wrapper">
    <!-- ... -->
  </div>
</template>

<script type="text/javascript">
  export default {
    // 이름 적는 것을 잊지마세요
    name: 'RangeSlider',
    // compose new components
    extends: {},
    // 컴포넌트 어트리뷰트 그룹
    props: {
      bar: {}, // 알파벳순으로 정렬합니다
      foo: {},
      fooBar: {},
    },
    // 컴포넌트 변수 그룹
    data() {},
    computed: {},
    // 컴포넌트가 다른 컴포넌트를 사용할 경우
    components: {},
    // 컴포넌트 메서드 그룹
    watch: {},
    methods: {},
    // 컴포넌트 라이프사이클 메서드 그룹
    beforeCreate() {},
    mounted() {},
};
</script>

<style scoped>
  .Ranger__Wrapper { /* ... */ }
</style>
```

[↑ 목차로 돌아가기](#목차)

## 컴포넌트 이벤트 이름

Vue.js는 모든 Vue 핸들러 함수를 제공하며 표현식은 ViewModel에 엄격하게 바인딩됩니다. 각 컴포넌트 이벤트는 개발 도중 문제를 피할 수있는 좋은 네이밍 스타일을 따라야합니다. 아래 **왜 그렇게 하나요?**를 참조하십시오.

### 왜 그렇게 하나요?

* 개발자가 이벤트 이름을 자유롭게 작성할 경우 혼란을 발생시킬 수 있습니다.
* 이벤트 이름을 자유롭게 작성할 수 있기 때문에 [DOM 템플릿의 비 호환성](https://kr.vuejs.org/v2/guide/components.html#DOM-템플릿-구문-분석-경고)이 발생할 수 있습니다.

### 어떻게 하나요?

* 이벤트 이름은 `kebab-cased`여야 합니다(하이픈으로 구분).
* 특별한 이벤트 이름은 `upload-success`, `upload-error` 또는 `dropzone-upload-success`, `dropzone-upload-error`(만약 범위가 지정된 접두어가 필요하다면)와 같이 외부 세계에 관심을 가질 수 있는 컴포넌트의 특별한 동작에 대해 실행되야 합니다.
* 이벤트는 부정사의 형태(e.g. client-api-load)이거나 명사(e.g. drive-upload-success)에서 동사로 끝나야 합니다. ([출처](https://github.com/GoogleWebComponents/style-guide#events))

[↑ 목차로 돌아가기](#목차)

## `this.$parent` 피하기

Vue.js는 부모 컨텍스트에 접근 할 수 있는 중첩 컴포넌트를 지원합니다. 하지만 Vue 컴포넌트 외부의 컨텍스트에 접근하면 [컴포넌트 기반 개발](#module-based-development)의 [FIRST](https://addyosmani.com/first/) 규칙을 위반하게 됩니다. 따라서 **`this.$parent`를 사용을 피해야합니다**.

### 왜 그렇게 하나요?

* Vue 컴포넌트는 다른 모든 컴포넌트와 마찬가지로 독립적으로 작동해야합니다. 컴포넌트가 부모에 접근해야할 경우 이 규칙은 무너지게 됩니다.
* 컴포넌트가 부모에 접근하는 경우 다른 곳에서 재사용할 수 없습니다.

### 어떻게 하나요?

* 어트리뷰트/프로퍼티를 사용하여 부모에서 자식 컴포넌트로 값을 전달합니다.
* 어트리뷰트 표현식에서 콜백을 사용하여 부모 컴포넌트에 정의된 메소드를 자식 컴포넌트로 전달하십시오.
* 자식 컴포넌트에서 이벤트를 `emit`하여 부모 컴포넌트에서 이벤트를 발생시킵니다.

[↑ 목차로 돌아가기](#목차)

## `this.$refs`를 주의하여 사용하기

Vue.js는 컴포넌트가 `ref` 어트리뷰트를 통해 다른 컴포넌트와 기본 HTML 엘리먼트에 접근 할 수 있도록 지원합니다. 이 어트리뷰트는 `this.$refs`를 통해 컴포넌트 또는 DOM 엘리먼트에 접근 할 수 있게 해줍니다. 대부분의 경우 `this.$refs`를 통해 **다른 컴포넌트**에 접근하는 것을 피할 수 있습니다. 이것은 잘못된 컴포넌트 API를 피하기 위해서 사용할 때 주의해야합니다.

### 왜 그렇게 하나요?

* Vue 컴포넌트는 **독립적으로 작동해야합니다**. 컴포넌트가 필요한 모든 접근을 지원하지 않으면 잘못 설계/구현된 것 입니다.
* 대부분의 컴포넌트는 프로퍼티와 이벤트가 충분해야합니다.

### 어떻게 하나요?

* 좋은 컴포넌트 API를 작성합니다.
* 항상 컴포넌트의 목적에 벗어나지 않도록 코드를 작성합니다.
* 특정한 코드(예: 특정 디바이스에 관한 코드)를 쓰지 마십시오. 일반적인 컴포넌트 내에 특정한 코드를 작성해야하는 경우 해당 API가 충분히 일반적인 것이 아니거나 새 컴포넌트가 필요할 수도 있음을 의미합니다.
* `props`를 모두 검사하여 놓친 부분이 있는지 확인하세요. 만약 놓친 부분이 있다면 이슈를 만들거나 직접 컴포넌트를 수정하세요.
* 모든 이벤트를 확인하세요. 대부분의 경우 개발자는 자식에서 부모로의 통신(이벤트)하는 것이 필요하다는 것을 잊었습니다. 그 이유는 부모에서 자식으로의 통신(`props` 사용)만 기억하기 때문입니다.
* **`props`는 아래로, 이벤트는 위로!** 좋은 API와 분리가 필요할 때 당신의 컴포넌트를 개선하세요.
* `props`와 이벤트를 사용하기 힘들고 `ref`를 사용하는 것에 의미가 있는 경우 컴포넌트에 `this.$refs`를 사용해야 합니다. (아래 예제를 참고하세요)
* 엘리먼트를 데이터 바인딩이나 디렉티브로 조작할 수 없는 경우 DOM 엘리먼트에 접근하기 위해 `jQuery`, `document.getElement*`, `document.queryElement`를 사용하는 대신 `this.$refs`를 사용하는 것은 좋은 방법입니다.

```html
<!-- 좋아요, ref를 사용하지 않아도 됩니다 -->
<range :max="max"
  :min="min"
  @current-value="currentValue"
  :step="1"></range>
```

```html
<!-- this.$refs를 사용하는 좋은 예제 입니다 -->
<modal ref="basicModal">
  <h4>Basic Modal</h4>
  <button class="primary" @click="$refs.basicModal.close()">Close</button>
</modal>
<button @click="$refs.basicModal.open()">Open modal</button>

<!-- Modal 컴포넌트 -->
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
<!-- `emit`으로 처리할 수 있다면 피하세요! -->
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

[↑ 목차로 돌아가기](#목차)


## 범위 스타일에 컴포넌트 이름 사용하기

Vue.js 컴포넌트 엘리먼트는 범위 스타일의 루트로 잘 사용 될 수 있는 사용자 지정 엘리먼트입니다.
또한 컴포넌트 이름을 CSS 클래스 네임스페이스로 사용할 수 있습니다.

### 왜 그렇게 하나요?

* 컴포넌트에 범위 스타일을 적용하면 컴포넌트 외부에 스타일이 적용되지 않으므로 예측 가능성이 향상됩니다.
* Vue.js 컴포넌트와 스타일의 루트 이름를 모듈 디렉토리와 동일한 이름으로 사용하면 개발자가 이해하기 좀 더 쉽습니다.

### 어떻게 하나요?

BEM 및 OOCSS에 기반한 네임스페이스 접두어로 컴포넌트 이름을 사용하고 `style`에 `scoped` 어트리뷰트를 사용하세요.
`scoped`를 사용하면 Vue 컴파일러는 `<style>`이 가진 모든 클래스에 특정 서명을 추가합니다. 이 서명은 브라우저가(지원한다면) 컴포넌트를 구성하는 모든 태그에 컴포넌트 CSS를 적용하고 CSS가 누출되지 않도록 도와줍니다.

```html
<style scoped>
  /* 권장합니다 */
  .MyExample { }
  .MyExample li { }
  .MyExample__item { }

  /* 피하세요! */
  .My-Example { } /* 컴포넌트나 모듈 이름으로 scoped가 지정되지 않고 BEM 문법과 일치하지 않습니다. */
</style>
```

[↑ 목차로 돌아가기](#목차)


## 컴포넌트 API를 문서화 하기

Vue.js 컴포넌트 인스턴스는 어플리케이션 내부의 컴포넌트 엘리먼트를 사용하여 만듭니다. 인스턴스는 사용자 정의 어트리뷰트를 통해 구성됩니다. 다른 개발자가 컴포넌트를 사용하는 경우 이러한 사용자 지정 어트리뷰트(컴포넌트의 API)를 `README.md` 파일에 문서화해야합니다.

### 왜 그렇게 하나요?

* 문서는 개발자가 코드 전체를 검토할 필요 없이 컴포넌트에 대한 간략한 개요를 제공합니다. 문서를 만들면 컴포넌트를 쉽게 접근하고 사용할 수 있습니다.
* 컴포넌트의 API는 사용자 지정 어트리뷰트의 집합을 통해 구성됩니다. 그러므로 이것들은 컴포넌트를 사용하기 원하는 (다시 개발하지 원치 않는) 다른 개발자들에게 중요합니다.
* 문서는 API를 형식을 갖추게하고 개발자에게 컴포넌트의 코드를 수정할 때 하위 호환성을 유지할 수 있도록 돕습니다.
* `README.md`는 먼저 읽어야할 문서를 위한 사실상 표준적인 파일명입니다. 코드 저장소를 호스팅하는 서비스(Github, Bitbucket, Gitlab 등)는 소스 디렉토리를 탐색할 때 README.md 파일의 내용을 표시합니다. 이것은 모듈 디렉토리에도 똑같이 적용됩니다.

### 어떻게 하나요?

`README.md` 문서를 컴포넌트의 모듈 디렉토리에 추가하세요.

```
range-slider/
├── range-slider.vue
├── range-slider.less
└── README.md
```

`README.md` 파일에서 모듈의 기능 및 사용법을 적으세요. Vue 컴포넌트의 경우 API와 마찬가지로 지원하는 사용자 정의 어트리뷰트에 대해서 설명하는 것이 좋습니다.


# 범위 슬라이더

## 기능

범위 슬라이더를 사용하면 슬라이더 핸들을 드래그하여 시작과 끝 값내에 값을 설정할 수 있습니다.

이 모듈은 크로스 브라우징 및 터치 지원을 위해서 [noUiSlider](http://refreshless.com/nouislider/)를 사용합니다.

## 사용 방법

`<range-slider>`는 다음과 같은 사용자 정의 컴포넌트 어트리뷰트를 지원합니다 :

| 어트리뷰트 | 타입 | 설명
| --- | --- | ---
| `min` | Number | 범위가 시작되는 값 (최소값).
| `max` | Number | 범위가 끝나는 값 (최대값).
| `values` | Number[] *optional* | 시작과 끝값을 포함하는 배열. 예) `values="[10, 20]"`. 기본값은 `[opts.min, opts.max]` 입니다.
| `step` | Number *optional* | 숫자를 증가/감소 시킬 값. 기본값은 1입니다.
| `on-slide` | Function *optional* | 사용자가 시작(`HANDLE == 0`) 또는 끝(`HANDLE == 1`)에 핸들을 위치하면 호출되는 함수힙니다. 예) `on-slide={ updateInputs }`, `component.updateInputs = (values, HANDLE) => { const value = values[HANDLE]; }`.
| `on-end` | Function *optional* | 사용자가 핸들 드래그를 멈추면 `(values, HANDLE)` 함수가 호출됩니다.

슬라이더 모양을 사용자 정의하려면 [noUiSlider 문서의 Styling 섹션](http://refreshless.com/nouislider/more/#section-styling)을 참고하세요.

[↑ 목차로 돌아가기](#목차)


## 컴포넌트 예제 추가하기

`index.html` 파일을 추가하여 컴포넌트의 사용 방법을 보여줍니다.

### 왜 그렇게 하나요?

* 컴포넌트 예제는 컴포넌트가 독립적으로 작동함을 증명합니다.
* 컴포넌트 예제를 작성하면 개발자가 문서 또는 코드를 살펴보기 전에 작동 모습을 확인 할 수 있습니다.
* 컴포넌트 예제는 컴포넌트를 사용할 수 있는 모든 설정을 보여줍니다.

[↑ 목차로 돌아가기](#목차)


## 컴포넌트 파일을 Lint하기

Lint는 코드 일관성을 개선하고 구문 오류를 추적하는 것을 도와줍니다. `.vue` 파일은 프로젝트에 `eslint-plugin-html`을 당신의 프로젝트에 추가할 수 있습니다. 만약 사용하길 원하면 당신은 `vue-cli`를 사용하여 ESLint가 활성화된 프로젝트를 시작할 수 있습니다.

### 왜 그렇게 하나요?

* 컴포넌트 파일을 Lint하면 모든 개발자가 동일한 코드 스타일을 작성할 수 있도록 보장합니다.
* 컴포넌트 파일을 Lint하면 구문 오류를 빠르게 수정할 수 있습니다.

### 어떻게 하나요?

Lint가 `*.vue` 파일에서 스크립트를 추출할 수 있도록 하려면 [스크립트를 컴포넌트의 `<script>` 안에 넣고](#use-script-inside-component) [컴포넌트의 표현식을 단순하게 사용해야 합니다](#컴포넌트-표현식을-단순하게-사용하기)(Lint가 표현식을 이해하지 못합니다). 그리고 Lint에 글로벌 변수 `vue`와 컴포넌트 `props`를 허용하도록 설정해야합니다.

#### ESLint

[ESLint](http://eslint.org/)는 컴포넌트에서 스크립트를 추출하기 위해서 [ESLint HTML 플러그인](https://github.com/BenoitZugmeyer/eslint-plugin-html#eslint-plugin-html)이 필요합니다.

`.eslintrc` 파일을 작성하여 ESLint를 설정하세요. (IDE는 그것을 해석할 수 있습니다)
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

ESLint를 실행해보세요.
```bash
eslint src/**/*.vue
```

#### JSHint

[JSHint](http://jshint.com/)은 `--extra-ext`를 사용하여 HTML을 파싱하고 `--extract=auto`를 사용하여 스크립트를 추출할 수 있습니다.
0
`.jshintrc` 파일을 작성하여 JSHint를 설정하세요. (IDE는 그것을 해석할 수 있습니다)
```json
{
  "browser": true,
  "predef": ["opts", "vue"]
}
```

JSHint를 실행해보세요.
```bash
jshint --config modules/.jshintrc --extra-ext=html --extract=auto modules/
```
참고: JSHint는 `vue`를 확장자로 사용하지 않고 `html`만을 사용합니다.

[↑ 목차로 돌아가기](#목차)

## 필요하다면 컴포넌트 만들기

### 왜 그렇게 하나요?

Vue.js는 컴포넌트 기반 프레임워크 입니다. 컴포넌트 만드는 시기를 모르면 다음과 같은 문제가 발생할 수 있습니다.

* 컴포넌트가 너무 크다면, 사용 및 유지보수가 어려울 수 있습니다.
* 컴포넌트가 너무 작다면, 프로젝트가 너무 커지고 컴포넌트간 통신이 어려울 수 있습니다.

### 어떻게 하나요?

* 프로젝트 요구 사항에 맞는 컴포넌트를 작성하는 것을 잊으면 안되지만 특정 기능을 컴포넌트를 사용하여 만들 수 있다고 생각해야 합니다. 라이브러리처럼 작성 할 수 있다면 훨씬 강력하고 일관성이 있습니다.
* 가능한 빨리 컴포넌트를 만들면 기존 컴포넌트에 안정적으로 통신(props 및 event)을 구현 할 수 있습니다.

### 규칙

* 첫째, 가능한 빠르게 현재 작성 중인 페이지 또는 전체적으로 모달, 팝 오버, 툴바, 메뉴, 헤더 등 필요할 것이 확실한 컴포넌트를 작성하세요.
* 둘째, 전체 페이지 또는 그 일부분에 대해 새롭게 개발하는 것을 서두르기 전에 생각해보세요. 당신이 작성해야할 그 일부분이 확실히 컴포넌트가 될 수 있을때 작성하세요.
* 마지막으로, 확실하지 않다면 만들지마세요! "나중에 쓸모있는 컴포넌트"로 프로젝트를 오염시키지 마십시오. 그 컴포넌트는 영원히 사용하지 않을 수도 있습니다. 프로젝트의 컴포넌트와의 호환성의 복잡성을 피하기 위해 필요할 때 컴포넌트를 만드는게 좋습니다.

[↑ 목차로 돌아가기](#목차)


---

## 도움을 주시겠습니까?

저장소를 Fork하고 Pull Request를 보내주시거나 [이슈](https://github.com/pablohpsilva/vuejs-component-style-guide/issues/new)를 작성해주시면 감사하겠습니다.


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

[↑ 목차로 돌아가기](#목차)

---

## 라이센스

[![CC0](http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

[De Voorhoede](https://twitter.com/devoorhoede)는 법이 허용하는 한도 내에서 모든 관련 저작권 및 이웃 권리를 포함하여 저작권법에 따라 전세계 모든 저작물에 대한 권리를 포기합니다.

허가없이 상업적 목적으로 복사, 수정, 배포 및 작업을 수행 할 수 있습니다.
-->

## 번역자들

* [한국어](https://pablohpsilva.github.io/vuejs-component-style-guide/#/korean): Lee Sun-Hyoup [github:kciter](https://github.com/kciter), [facebook: sunhyoup.lee](https://www.facebook.com/sunhyoup.lee)
* [브라질 포르투갈어](README-PTBR.md): Pablo Henrique Silva [github:pablohpsilva](https://github.com/pablohpsilva), [twitter: @PabloHPSilva](https://twitter.com/PabloHPSilva)
* [러시아어](https://pablohpsilva.github.io/vuejs-component-style-guide/#/russian): Mikhail Kuznetcov [github:shershen08](https://github.com/shershen08), [twitter: @legkoletat](https://twitter.com/legkoletat)
