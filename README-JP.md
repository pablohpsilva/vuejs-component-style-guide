# Vue.js コンポーネント スタイル ガイド

<p align="center">
  <img src="https://raw.githubusercontent.com/pablohpsilva/vuejs-component-style-guide/master/img/logo.png"/>
</p>

### 翻訳
* [英語](https://pablohpsilva.github.io/vuejs-component-style-guide/#/)
* [ブラジルポルトガル語](https://pablohpsilva.github.io/vuejs-component-style-guide/#/portuguese)
* [中国語](https://pablohpsilva.github.io/vuejs-component-style-guide/#/chinese)
* [韓国語](https://pablohpsilva.github.io/vuejs-component-style-guide/#/korean)
* [ロシア語](https://pablohpsilva.github.io/vuejs-component-style-guide/#/russian)

## 目的

このガイドはあなたの[Vue.js](http://vuejs.org/) のコードを統一する方法を提供します。

* 開発者／チームメンバがより問題を理解し、見つけやすくする。
* IDEがよりコードを解釈し、支援を提供しやすくする。
* すでに使用しているビルドツールを（再）利用しやすくする。
* 別々のコードの塊を蓄え、供給しやすくする。

このガイドは[De Voorhoede](https://github.com/voorhoede)による[RiotJS Style Guide](https://github.com/voorhoede/riotjs-style-guide)に刺激を受け作られました。

## 目次

* [モジュールベースの開発](#モジュールベースの開発)
* [Vueコンポーネントの名前](#vueコンポーネントの名前)
<!-- * [Use `*.vue` extension](#use-vue-extension) -->
* [コンポーネントの記述をシンプルに保つ](#コンポーネントの記述をシンプルに保つ)
* [コンポーネントのpropsをプリミティブに保つ](#コンポーネントのpropsをプリミティブに保つ)
* [コンポーネントのpropsの利用](#コンポーネントのpropsの利用)
* [`this`を`component`に割り当てる](#thisをcomponentに割り当てる)
* [コンポーネント構成](#コンポーネント構成)
* [コンポーネントイベント名](#コンポーネントイベント名)
* [`this.$parent`を避ける](#thisparentを避ける)
* [`this.$refs`は注意して使用する](#thisrefsは注意して使用する)
* [スタイルスコープとしてコンポーネント名を使用する](#スタイルスコープとしてコンポーネント名を使用する)
* [コンポーネントAPIをドキュメント化する](#コンポーネントapiをドキュメント化する)
* [コンポーネントデモの追加](#コンポーネントデモの追加)
* [コンポーネントファイルをLintする](#コンポーネントファイルをlintする)
* [必要に応じてコンポーネントを作成する](#必要に応じてコンポーネントを作成する)
<!-- * [Add badge to your project](#add-badge-to-your-project) -->


## モジュールベースの開発

常に単一の機能を持つ小さなモジュールからアプリケーションを構築しましょう。

モジュールはアプリケーションの自己完結型の小さな部品です。特にVue.jsライブラリはあなたが*view-logicなモジュール*を作れるように設計されています。

### 理由

小さなモジュールにすることで、あなたと他の開発者両方にとって、学びやすく、理解しやすく、維持しやすく、再利用しやすく、そして、
デバッグしやすくなります。

### 方法

各Vueコンポーネント（モジュールのようなもの）は [FIRST](https://addyosmani.com/first/): ひとつのことに集中し (*Focused* ([単一責任](http://en.wikipedia.org/wiki/Single_responsibility_principle)))、独立していて(*Independent*)、 再利用可能で(*Reusable*)、小さく(*Small*) そしてテスト可能 (*Testable*)でなければなりません。

もしあなたのコンポーネントが多くのことをしていて大きすぎる場合、ひとつのことだけをする、より小さなコンポーネントに分けましょう。
経験則から言うと、各コンポーネントは１００行以下のコードになるようにするのがいいでしょう。また、例えばスタンドアローンのデモを追加することによって、
Vueコンポーネントが独立して動作することを確認しましょう。

[↑ 目次に戻る](#目次)


## Vueコンポーネントの名前

各コンポーネントの名前は、

* **意味のある名前で**: 具体的過ぎず、抽象的過ぎず。
* **短く**: ２または３語。
* **発音可能**: それらについて話せるようにしたい。

であるべきです。

さらにVueコンポーネントの名前は、

* **カスタム要素仕様に準拠**: [ハイフンを含み](https://www.w3.org/TR/custom-elements/#concepts)、 予約語は使用しない。
* **`app-` ネームスペース**: 非常に汎用的、あるいは１語であれば、他のプロジェクトでも容易に再利用できる。

であるべきです。

### 理由

* 名前は、コンポーネントについて会話するときに使用されます。したがって、それは短く、意味があり、発音可能でなければなりません。

### 方法

```html
<!-- 推奨 -->
<app-header></app-header>
<user-list></user-list>
<range-slider></range-slider>

<!-- 非推奨 -->
<btn-group></btn-group> <!-- 短いですが, 発音が困難です。代わりに`button-group`を使いましょう -->
<ui-slider></ui-slider> <!-- 全てコンポーネントはuiなので、中身を表していません -->
<slider></slider> <!-- カスタム要素仕様に準拠していません -->
```

[↑ 目次に戻る](#目次)


## コンポーネントの記述をシンプルに保つ

Vue.jsのインラインの記述は100％JavaScriptです。これは非常に強力なことですが、複雑になる可能性もあるということです。
ですので、**シンプルな記述を保つようにしましょう。**.

### 理由

* 複雑なインラインの記述は判読困難です。
* インラインの記述は他の場所で再利用できません。これはコードの重複と腐敗につながります。
* IDEは基本的に式の構文サポート機能を持っていないため、自動補完や検証を行うことができません。

### 方法

もしあまりにも複雑だったり、判読困難な場合は**メソッド、またはcomputedプロパティに移動させましょう**!

```html
<!-- 推奨 -->
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

<!-- 非推奨 -->
<template>
  <h1>
    {{ `${(new Date()).getUTCFullYear()}-${('0' + ((new Date()).getUTCMonth()+1)).slice(-2)}` }}
  </h1>
</template>
```

[↑ 目次に戻る](#目次)


## コンポーネントのpropsをプリミティブに保つ

Vue.jsは複雑なJavaScriptオブジェクトを渡せるようになっていますが, **コンポーネントのpropsはできるだけプリミティブに保つ** ようにするべきです。複雑なオブジェクトの使用を避け、[JavaScriptプリミティブ](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)と関数のみを使うようにしましょう。

### 理由

* 各propの属性を別々に使用することにより、コンポーネントは明確で表現力豊かなAPIを持つことになります。
* propsの値としてプリミティブとファンクションのみを使用することで、コンポーネントのAPIをネイティブHTML（5）のAPIに似たものにできます。
* 各propの属性を使用することで、他の開発者がコンポーネントインスタンスに何が渡されるかを理解しやすくなります。
* 複雑なオブジェクトが渡されると、そのオブジェクトのどのプロパティとメソッドが実際にカスタムコンポーネントで使われるかがわかりにくくなります。これによりコードのリファクタリングが難しくなり、腐敗を招くことになります。

### 方法

プリミティブまたは関数を値としたpropsごとのコンポーネント属性を使用します。

```html
<!-- 推奨 -->
<range-slider
  :values="[10, 20]"
  min="0"
  max="100"
  step="5"
  :on-slide="updateInputs"
  :on-end="updateResults">
</range-slider>

<!-- 非推奨 -->
<range-slider :config="complexConfigObject"></range-slider>
```

[↑ 目次に戻る](#目次)


## コンポーネントのpropsの利用

Vue.jsでは、あなたのコンポーネントのpropsはあなたのAPIです。頑丈で予測しやすいAPIは、他の開発者があなたのコンポーネントを使用するのを簡単にします。

コンポーネントのpropsはカスタムHTML属性を介して渡されます。 これらの属性の値はVue.jsプレーンストリング (`:attr="値"` または `v-bind:attr="値"`)か、または無いこともあります。 あなたは **コンポーネントのpropsを利用** して、それらの異なるケースに対応できるようにしましょう。

### 理由

コンポーネントのpropsを利用することで、あなたのコンポーネントを常に機能するようになります（防御的プログラミング）。それは後で他の開発者が、あなたが想定していない方法でコンポーネントを使用する場合でもです。

### 方法

* propsのデフォルト値を使用します。
* 期待するタイプの値の[検証](http://vuejs.org/v2/guide/components.html#Prop-Validation)のために、`type`オプションを使用します。**[1\*]**
* 使用される前にpropsが存在するかチェックします。

```html
<template>
  <input type="range" v-model="value" :max="max" :min="min">
</template>
<script type="text/javascript">
  export default {
    props: {
      max: {
        type: Number, // [1*] これは'max'propが数値であることを検証します。
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

[↑ 目次に戻る](#目次)


## `this`を`component`に割り当てる

Vue.jsコンポーネントのコンテキスト内では、 `this`はコンポーネントインスタンスにバインドされています。
したがって、別のコンテキストで参照する必要がある場合は、 `this` が`component`として使用できることを確認してください。

言い換えれば: `const self = this;`のようなコーディングはもう **しないで** ください。 Vueコンポーネントの使用は安全です。

### 理由

* ES6を使っている場合、`this`を変数に保存しておく必要はありません。
* 通常、アロー関数を使用すれば静的スコープは保持されます。
* ES6を使用して**いない**ために、`アロー関数`を使用できない場合は、`this`を変数に格納する必要があります。それが唯一の例外です。

### 方法

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

<!-- 非推奨 -->
<script type="text/javascript">
export default {
  methods: {
    hello() {
      return 'hello';
    },
    printHello() {
      const self = this; // 不要
      console.log(self.hello());
    },
  },
};
</script>
```

[↑ 目次に戻る](#目次)


## コンポーネント構成

論理的に考えやすく、思考の流れに従いやすいようにしましょう. 方法を見てください。

### 理由

* コンポーネントを明確かつグループ化されたオブジェクトとすることで、コードを読みやすくし、開発者はコードの基準を簡単に持てるようになります。
* props、data、computed、watches、そしてmethodsをアルファベット順に並べることで、見つけやすくなります。
* 繰り返しになりますが, コンポーネントをグループ化することで読みやすくなります (name、extends、props、dataそしてcomputed、components、 watch、methods、lifecycle methods、など);
* `name`属性を使いましょう. [vue devtools](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=en)とname属性を使うと、開発/テストが容易になります。
* [BEM](https://medium.com/tldr-tech/bem-blocks-elements-and-modifiers-6b3b0af9e3ea#.bhnomd7gw)、または [rscss](https://github.com/rstacruz/rscss)のようなCSSの命名方法論を使いましょう。 - [詳細?](#スタイルスコープとしてコンポーネント名を使用する);
* Vue.jsの製作者Evan Youが推奨するように、テンプレートスクリプト形式の.vueファイル構成を使用しましょう。

### 方法

コンポーネンのト構成:

```html
<template lang="html">
  <div class="Ranger__Wrapper">
    <!-- ... -->
  </div>
</template>

<script type="text/javascript">
  export default {
    // このちいさな要素を忘れないでください
    name: 'RangeSlider',
    // 新しいコンポーネントを合成します
    extends: {},
    // コンポーネントのプロパティ/値
    props: {
      bar: {}, // アルファベット順
      foo: {},
      fooBar: {},
    },
    // 変数
    data() {},
    computed: {},
    // 他のコンポーネントを使用
    components: {},
    // メソッド
    watch: {},
    methods: {},
    // コンポーネントライフサイクルフック
    beforeCreate() {},
    mounted() {},
};
</script>

<style scoped>
  .Ranger__Wrapper { /* ... */ }
</style>
```

[↑ 目次に戻る](#目次)

## コンポーネントイベント名

Vue.jsはすべてのVueハンドラ関数を提供し、式はViewModelに厳密にバインドされています。各コンポーネントのイベントは、開発中の問題を回避するような良い名前付けのスタイルを適用しましょう。 以下の **理由** を参照してください。

### 理由

* 開発者は好きなイベント名を自由に使うことができるため、混乱を招く可能性があります。
* イベントに名前をつける自由は、 [DOMテンプレートの非互換性](https://vuejs.org/v2/guide/components.html#DOM-Template-Parsing-Caveats)につながる可能性があります。

### 方法

* イベント名前はケバブケースにするべきです。
* あなたのコンポーネントで外部で関心を持つ独自のアクションのため、upload-success、upload-error、さらにはdropzone-upload-success、 dropzone-upload-errorのような固有の名前をつける必要があります。(スコープ付きプレフィックスを使用する必要がある場合は)
* イベント名は末尾が不定形の動詞(例 client-api-load)、または名詞(例 drive-upload-success)で終わるべきです。([出典](https://github.com/GoogleWebComponents/style-guide#events));

[↑ 目次に戻る](#目次)

## `this.$parent`を避ける

Vue.jsは、親コンテキストにアクセスできるネストされたコンポーネントをサポートしています。 あなたのVueコンポーネントの外部のコンテキストにアクセスすることは、 [モジュールベースの開発](#module-based-development)の[FIRST](https://addyosmani.com/first/)のルールに違反するため、 ** `this.$parent`の使用を避ける ** べきです。

### 理由

* vueコンポーネントは、他のコンポーネントと同様に、独立して動作する必要があります。 コンポーネントがその親にアクセスする必要がある場合、このルールは壊されます。
* コンポーネントがその親にアクセスする必要がある場合、別のコンテキストで再利用することはできません。

### 方法

* 属性/プロパティを使用して、親から子コンポーネントに値を渡します。
* 属性の式のコールバックを使用して、親コンポーネントで定義されたメソッドを子コンポーネントに渡します。
* 子コンポーネントからイベントを発行し、それを親コンポーネントでキャッチします。

[↑ 目次に戻る](#目次)

## `this.$refs`は注意して使用する

Vue.jsは`ref`属性を介して他のコンポーネントや、基本的なHTML要素のコンテキストにアクセスできるコンポーネントをサポートしています。この属性は`this.$refs`を介して、コンポーネントまたDOM要素のコンテキストにアクセス可能な方法を提供します。ほとんどの場合、 `this.$refs`を介して **他のコンポーネント** のコンテキストにアクセスする必要性は避けることができます。このため、間違ったコンポーネントAPIを避けるためにこれを使用するときは注意が必要です。

### 理由

* vueコンポーネントは、他のコンポーネントと同様に、**独立して動作する必要があります**。 コンポーネントが必要なすべてのアクセスをサポートしていない場合、それは悪い設計/実装です。
* ほとんどのコンポーネントでは、プロパティとイベントで十分です。

### 方法

* 良いコンポーネントAPIを作りましょう。
* 独創的なコンポーネントの目的に常に注意してください。
* 特殊なコードを書かないでください。ジェネリックコンポーネント内に特殊なコードを記述する必要がある場合は、APIが十分に一般的でないか、他のケースを管理するために新しいコンポーネントが必要になるということです。
* すべてのpropsをチェックして、欠けているものがあるかどうか確認します。 もしあれば、課題を作成するかあなたのコンポーネントを強化しましょう。
* すべてのイベントをチェックしましょう。ほとんどのケースで、開発者は子-親のコミュニケーション（イベント）が必要であることを忘れてしまいます。 そのため、（propsを使用した）親-子のコミュニケーションだけを覚えているのです。
* **Propsは下へ、eventsは上へ!** ゴールとして良いAPIと分離を要求された場合、コンポーネントをアップグレードしましょう。
* propsやeventsが消耗し、コンポーネントでの`this.$refs`の利用が利にかなっているときは、それを使用するべきです。 (下の例を参照してください).
* データバインディングやディレクティブで要素を操作できない場合、 (`jQuery`, `document.getElement*`, `document.queryElement`の代わりに) `this.$refs`を使用してDOM要素にアクセスすることは良い方法です。

```html
<!-- 良いです。refは必要ありません。 -->
<range :max="max"
  :min="min"
  @current-value="currentValue"
  :step="1"></range>
```

```html
<!-- this.$refsを使用する場合の良い例です。 -->
<modal ref="basicModal">
  <h4>Basic Modal</h4>
  <button class="primary" @click="$refs.basicModal.hide()">Close</button>
</modal>
<button @click="$refs.basicModal.open()">Open modal</button>

<!-- モーダルコンポーネント -->
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
<!-- 発行される可能性のあるものへのアクセスは避けましょう -->
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

[↑ 目次に戻る](#目次)


## スタイルスコープとしてコンポーネント名を使用する

Vue.jsのコンポーネント要素は、スタイルスコープのルートとして非常によく使用されるカスタム要素です。
あるいは、コンポーネント名はCSSクラスネームスペースとして、使用できます。

### 理由

* コンポーネント要素のスタイルをスコープすると、コンポーネントの外にスタイルが漏れるのを防ぐため、予測可能性が向上します。

* モジュールディレクトリに同じ名前を使用すると、Vue.jsコンポーネントとスタイルルートが同じところに属していることを開発者が理解しやすくなります。

### 方法

BEMとOOCSSに基づくネームスペースプレフィックスとしてコンポーネント名を使いましょう。 **そして** スタイルクラスで`scoped`属性を使いましょう。
`scoped`を使用すると、`<style>`にあるすべてのクラスにシグネチャを追加するようVueコンパイラに指示します。このシグネチャは、コンポーネントを構成するすべてのタグにコンポーネントCSSを適用するようブラウザに（サポートしている場合）強制し、CSSのスタイリングが漏れないようにします。

```html
<style scoped>
  /* 推奨 */
  .MyExample { }
  .MyExample li { }
  .MyExample__item { }

  /* 非推奨 */
  .My-Example { } /* コンポーネント名またはモジュール名にスコープされておらず、BEM準拠でもありません */
</style>
```

[↑ 目次に戻る](#目次)


## コンポーネントAPIをドキュメント化する

Vue.jsコンポーネントのインスタンスは、アプリケーション内のコンポーネント要素を使用して作成されます。インスタンスはそのカスタム属性によって構成されます。他の開発者がコンポーネントを使用するときのため、これらのカスタム属性（コンポーネントのAPI）を`README.md`ファイルに記述しましょう。

### 理由

* ドキュメンテーションは、開発者に、そのコードのすべてを伝えることなく、コンポーネントの概要を高レベルで表示します。これにより、コンポーネントのアクセスが容易になり、使いやすくなります。
* コンポーネントのAPIは、それを構成するカスタム属性のセットです。したがって、これらは、コンポーネントを使用する（開発しない）開発者にとって特に重要です。
* ドキュメンテーションはAPIを形式化し、コンポーネントのコードを変更するときに下位互換性を維持する機能を開発者に教えます。
* `README.md`は最初に読み込まれるドキュメントのデファクトスタンダードファイル名です。 コードリポジトリホスティングサービス（Github、Bitbucket、Gitlabなど）は、ソースディレクトリを参照するときに、READMEファイルの内容を直接表示します。これは私たちのモジュールディレクトリにも当てはまります。

### 方法

コンポーネントのモジュールディレクトリに`README.md`ファイルを追加しましょう。

```
range-slider/
├── range-slider.vue
├── range-slider.less
└── README.md
```

READMEファイルに、モジュールの機能と使用方法を記述しましょう。 vueコンポーネントの場合、APIとしてサポートされているカスタム属性を記述するのが最も役立ちます。


# Range slider

## 機能

range sliderは、スライダーレールのハンドルを開始値と終了値の両方でドラッグして数値範囲を設定できます。

このモジュールはクロスブラウザとタッチサポートのため、noUiSliderを使用します。

## 使用方法

`<range-slider>` 次のカスタムコンポーネント属性をサポートしています:

| 属性 | 型 | 説明
| --- | --- | ---
| `min` | 数値 | レンジの始まりの数値 (下限)。
| `max` | 数値 | レンジの終わりの数値 (上限).
| `values` | 数値[] *任意* | 開始値と終了値を含む配列。  例. `values="[10, 20]"`. デフォルトは`[opts.min, opts.max]`.
| `step` | 数値 *任意* | インクリメント/デクリメントの数値。デフォルトは1。
| `on-slide` | 関数 *任意* | ユーザーが開始（`HANDLE == 0`）または終了（`HANDLE == 1`）ハンドルをドラッグしているときに、`(values, HANDLE)`で呼び出される関数。例. `on-slide={ updateInputs }` `component.updateInputs = (values, HANDLE) => { const value = values[HANDLE]; }`.
| `on-end` | 関数 *任意* | ユーザーがハンドルのドラッグを停止したとき、`(values, HANDLE)`で呼び出される関数。

スライダの外観をカスタマイズするには[noUiSliderドキュメントのStylingセクション](http://refreshless.com/nouislider/more/#section-styling)を参照してください。


[↑ 目次に戻る](#目次)


## コンポーネントデモの追加

構成の異なるコンポーネントのデモを含むindex.htmlファイルを追加し、コンポーネントの使用方法を示しましょう。

### 理由

* コンポーネントのデモは、コンポーネントが単独で動作することを示します。
* コンポーネントのデモは、ドキュメンテーションやコードを掘り起こす前に、開発者にプレビューを提供します。
* デモでは、コンポーネントを使用できるすべての可能な構成とバリエーションを示すことができます。

[↑ 目次に戻る](#目次)


## コンポーネントファイルをLintする

Lintersはコードの一貫性を高め、構文エラーの追跡に役立ちます。.vueファイルはプロジェクトに`eslint-plugin-html`を追加してlintすることができます。vue-cliを使用すれば、ESLintをデフォルトで有効にしてプロジェクトを開始できます。

### 理由

* コンポーネントファイルのLintは、すべての開発者が同じコードスタイルを使用するようにします。
* コンポーネントファイルのLintは、遅くなる前に構文エラーの追跡をするのに役立ちます。

### 方法

lintersがあなたの`*.vue`ファイルからスクリプトを抽出するためには, [スクリプトを`<script>`コンポーネントの中に置き](#use-script-inside-component)、 [コンポーネントの記述をシンプルに保つ](#コンポーネントの記述をシンプルに保つ)ようにしてください (lintersはそれらを理解できないので)。 グローバル変数`vue`とコンポーネントの`props`を許可するようにリンターを設定します。

#### ESLint

[ESLint](http://eslint.org/)には、コンポーネントファイルからスクリプトを抽出するための追加の[ESLint HTML plugin](https://github.com/BenoitZugmeyer/eslint-plugin-html#eslint-plugin-html)が必要です。

ESLintを`.eslintrc`ファイルに設定します。(IDEもそれを解釈することができます):
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

ESLint実行
```bash
eslint src/**/*.vue
```

#### JSHint

[JSHint](http://jshint.com/)は (`--extra-ext`を使用して)HTMLをパースでき、(`--extract=auto`を使用して)スクリプトを抽出できます。

JSHintを`.jshintrc`ファイルに設定します。 (IDEもそれを解釈することができます):
```json
{
  "browser": true,
  "predef": ["opts", "vue"]
}
```

JSHint実行
```bash
jshint --config modules/.jshintrc --extra-ext=html --extract=auto modules/
```
注意: JSHintは`vue`を拡張子として受け入れず, htmlだけを受け入れます。

[↑ 目次に戻る](#目次)


## 必要に応じてコンポーネントを作成する

### 理由

Vue.jsはコンポーネントフレームワークに基づいています。コンポーネントの作成タイミングを知らないと、次のような問題が発生する可能性があります。

* コンポーネントが大きすぎる場合は、おそらく（再）使用し、維持することは困難です。
* コンポーネントが小さすぎると、プロジェクトが浸水し、コンポーネント間の通信が困難になります。

### 方法

* あなたのプロジェクトのニーズに合わせてコンポーネントを構築することを常に忘れないでください。しかし、それらのコンポーネントが取り出せると考えるようにしてください。ライブラリのようにプロジェクトから取り出せれば、より堅牢で一貫性のあるものになります。
* 既存のコンポーネントや安定したコンポーネントにコミュニケーション（propsやevents）を組み込むことができるため、できるだけ早くコンポーネントを構築する方が良いでしょう。

### ルール

* まず、できるだけ早くモーダル、ポップオーバー、ツールバー、メニュー、ヘッダーなどの明白なコンポーネントを作成してみてください。あなたの現在のページ、または全体的に、あなたが後で必要となることをあなたが知っているコンポーネントを。
* 第２に、新しい開発ごとに、ページ全体またはその一部分について、急いで開発する前に考えてみてください。その一部がコンポーネントであることが分かっている場合は、作成してください。
* 最後に、もし確信がなければ、コンポーネントを作らないでください！"後で役立つかもしれない"コンポーネントであなたのプロジェクトを汚染するのを避けましょう。それらは空っぽで、永遠にそこにあるだけかもしれません。プロジェクトの残りの部分との互換性の複雑さを避けるために、その状態になっていたことに気づいたらすぐにそれを分解するよう注意してください。

[↑ 目次に戻る](#目次)

---

## 貢献するには

フォークしてプルリクエストを送るか、[Issue](https://github.com/pablohpsilva/vuejs-component-style-guide/issues/new)を作ってください。


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

[↑ 目次に戻る](#目次)

---

## License

[![CC0](http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

[De Voorhoede](https://twitter.com/devoorhoede) waives all rights to this work worldwide under copyright law, including all related and neighboring rights, to the extent allowed by law.

You can copy, modify, distribute and perform the work, even for commercial purposes, all without asking permission.
 -->


## 翻訳者

* [ブラジルポルトガル語](README-PTBR.md): Pablo Henrique Silva [github:pablohpsilva](https://github.com/pablohpsilva), [twitter: @PabloHPSilva](https://twitter.com/PabloHPSilva)
* [ロシア語](https://pablohpsilva.github.io/vuejs-component-style-guide/#/russian): Mikhail Kuznetcov [github:shershen08](https://github.com/shershen08), [twitter: @legkoletat](https://twitter.com/legkoletat)
