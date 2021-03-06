# Vue.jsの基礎

> Vue CLIを使ってVue.jsの基本的な構文を理解します。

Vue.jsの基礎を学習します。<br>
公式リファレンスをベースによく使う構文をピックアップしています。<br>
その他の詳細は[公式リファレンス](https://jp.vuejs.org/v2/guide/)、[APIリファレンス](https://jp.vuejs.org/v2/api/)を確認してください。<br>

# Vue CLIの導入
Vueのテンプレートを用いて学習します。<br>
テンプレートを容易に構築できるVue CLIを使います。

```bash
# Vue CLI 3.0をインストール
$ npm install -g @vue/cli

# パーミッションで怒られたらsudoをつけてインストールしてください
$ sudo npm install -g @vue/cli

# Vue CLIのバージョン確認し、インストールしたことを確認
$ vue --version
```
## テンプレートを作成
Vue CLIを使ってテンプレートを作成します。

```bash
# テンプレートを作成
$ vue create frontendkadai2

Vue CLI v3.0.1
? Please pick a preset: default (babel, eslint)
```

localhostを起動して画面を確認します。
```bash
$ yarn serve
```

[http://localhost:8080](http://localhost:8080) をブラウザで確認し、以下のような画面が確認できればOKです。<br>
<a href="https://imgur.com/S55Q23B"><img src="https://i.imgur.com/S55Q23B.png" width="300" height="300" /></a>

## ディレクトリ構成
```
frontendkadai2/
  ├── node_modules
  ├── public
  │     ├── favicon.ico
  │     └── index.html
  ├── src
  │     ├── assets	 
  │     ├── components
  │     │     └──  HelloWorld.vue
  │     ├── App.vue
  │     └── main.js
  ├── .gitignore
  ├── babel.config.js
  ├── package.json
  ├── README.md
  └── yarn.lock
```

| ファイル | 概要 |
|:---|:---|
|node_modules |導入したライブラリ|
|public |エントリーポイントとなるディレクトリ|
|src/assets |画像などを格納|
|src/components |コンポーネントVueを格納|
|src/App.vue: |エントリーポイントVue|
|src/main.js |vueのプラグインなどの設定を行う|
|.gitignore |Gitリモートリポジトリへのデプロイ対象外のファイルを設定|
|babel.config.js |babelの設定|
|package.json |npm（又はyarn）の操作、導入ライブラリの設定|
|yarn.lock |導入したライブラリのバージョンと依存関係の管理ファイル|

## .vueの構成
HTML、script、styleの構成でコーディングします。
```html
<template>
<!-- HTMLを記述します。-->
</template>

<script>
// JavaScript（TypeScript）記述します。
export default {}
</script>

<style scoped>
/* CSS（SCSS、Stylus）を記述します。 */
</style>
```

# 基礎
## データバインディング
データのバインディングは {{}} を使います。<br>
テキスト入力に伴い値を更新する場合は双方向バインディングのv-modelを使用します。<br>
```html
<template>
  <div>
    <input type="text" v-model="name"/>
    <div>私は{{ name }}です</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: 'エンジニア'
    }
  }
}
</script>
```

データの更新を反映させたくない場合はv-onceを使います。
```html
<template>
  <div>
    <input type="text" v-model="name"/>
    <div v-once>私は{{ name }}です</div>
  </div>
</template>
```
## HTMLを挿入
データをHTMLとして扱いたい場合はv-htmlを使います。
```html
<template>
  <div v-html="htmlData"/>
</template>

<script>
export default {
  data() {
    return {
      htmlData: `<b><font color="red">私の名前はエンジニアです</font></b>`
    }
  }
}
</script>
```
## 属性
属性はv-bind（省略して : ）を使います。<br>
以下の例はボタンの押下許可の設定を行います。<br>
```html
<template>
  <div>
    <button v-bind:disabled="isDisabled">ボタン1</button>
    <button :disabled="isDisabled">ボタン2</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isDisabled: false
    }
  }
}
</script>
```

## ディレクティブ
v-から始まる構文をディレクティブと言います。<br>
```html
<template>
  <div>
    <div v-if="isShow">見えていますか</div>
    <span v-for="(item, index) in items" :key="index">{{ item }}</span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isShow: true,
      items: ["こ", "ん", "に", "ち", "わ"]
    }
  }
}
</script>
```
## イベントをバインド
v-on（又は@）を使います。
```html
<template>
  <div>
    <div>
      <button v-on:click="onClick(1)">ボタン1</button>
      <button @click="onClick(2)">ボタン2</button>
    </div>
    <div>
      ボタンタグ = {{ btnTag }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      btnTag: 0
    }
  },
  methods: {
    onClick(tag) {
      this.btnTag = tag
    }
  }
}
</script>
```
## 修飾子
.で繋ぐとイベントの処理を指定できます。
```html
<template>
  <div>
    <div @click="onClick(100)">
      <button @click.stop="onClick(3)">ボタン3</button>
    </div>
    <div>
      ボタンタグ = {{ btnTag }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      btnTag: 0
    }
  },
  methods: {
    onClick(tag) {
      this.btnTag = tag
    }
  }
}
</script>
```
stopを指定しない場合はボタン3を押すとdivのイベントも処理されますが、stopを指定するとボタン3のイベントだけ処理されます。<br>
その他の詳細は[こちら](https://jp.vuejs.org/v2/guide/events.html)を参照。

## データ
変数や配列はdataで定義します。
```html
<template>
  <div>
    <div>
      変数は {{ name }}
    </div>
    <div>
      配列の中身は
      <span v-for="(item, index) in items" :key="index">{{ item }}</span>
    </div>
    <div>
      JSON配列の中身はこちら
      <div v-for="(item) in infos" :key="item.id">
        {{ item.name }}は{{ item.hobby }}が趣味
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "エンジニア",
      items: ["こ", "ん", "に", "ち", "わ"],
      infos: [
        { id: "0", name: "エンジニア", hobby: "ランニング" },
        { id: "1", name: "プログラマー", hobby: "ウォーキング" },
        { id: "2", name: "デザイナー", hobby: "水泳" }
      ]
    }
  }
}
</script>
```

## メソッド
メソッドはmethodsで定義します。
```html
<template>
  <div>
    <button @click="onLink">Googleへゴー！</button>
  </div>
</template>

<script>
export default {
  methods: {
    onLink() {
      location.href = "https://www.google.com/"
    }
  }
}
</script>
```
## インスタンス生成と削除のタイミング
### created
Vueインスタンスが生成されたタイミングで呼ばれます。<br>
DOM（Document Object Modelの略称、HTMLタグの要素にアクセスできる仕組みのこと）を操作できる前に呼ばれます。<br>
```js
export default {
  created() {
    console.log("DOM操作不可")
  }
}
```
### mounted
Vueインスタンスが生成されたタイミングで処理されます。<br>
このタイミングではDOMが操作できる状態です。
```js
export default {
  mounted() {
    console.log("DOM操作可能")
  }
}
```
### destroyed
Vueインスタンスが破棄されたタイミングで呼ばれます。<br>
```js
export default {
  destroyed() {
    console.log("さようなら！")
  }
}
```

その他の詳細は[こちら](https://jp.vuejs.org/v2/guide/instance.html)。
## 監視
データを監視する場合はwatchを使います。
```html
<template>
  <div>
    <input type="text" v-model="name"/>
    <div>あたなの名前は {{ name }}</div>
    <div>文字数: {{ nameCount }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "エンジニア",
      nameCount: 0
    }
  },
  mounted() {
    // nameCountの初期値を"エンジニア"の文字数で設定
    this.nameCount = this.name.length
  },
  watch: {
    name(newVal, oldVal) {
      // newValが変更後の値、oldValが変更前の値.
      this.nameCount = newVal.length
    }
  }
}
</script>
```

JSONデータはdeep: trueを設定すると監視されます。
```HTML
<template>
  <div>
    <input type="text" placeholder="id" v-model="info.id"/>
    <input type="text" placeholder="name" v-model="info.name"/>
    <input type="text" placeholder="hobby" v-model="info.hobby"/>
    <p>{{ watchItem }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      info: {
        id: 0,
        name: "エンジニア",
        hobby: "ランニング"
      },
      watchItem: ""
    }
  },
  watch: {
    info: {
      handler(val) {
        this.watchItem = `${val.id}, ${val.name}, ${val.hobby}`
      },
      deep: true
    }
  }
}
</script>
```

JSONデータの中の任意の値だけ監視することもできます。
```HTML
<template>
  <div>
    <input type="text" placeholder="id" v-model="info.id"/>
    <input type="text" placeholder="name" v-model="info.name"/>
    <input type="text" placeholder="hobby" v-model="info.hobby"/>
    <p>{{ watchItem }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      info: {
        id: 0,
        name: "エンジニア",
        hobby: "ランニング"
      },
      watchItem: ""
    }
  },
  watch: {
    'info.name': {
      handler(val) {
        this.watchItem  = val
      }
    }
  }
}
</script>
```
## 算出プロパティ
computedを使うことで処理をまとめることができます。<br>
v-ifやclassで使うと便利です。<br>
```HTML
<template>
  <div>
    <div v-if="dateJudgement">まだ2018年ですね！</div>
    <div v-bind:class="fontColor">エラーだったら赤色になるよ</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      error: null
    }
  },
  computed: {
    dateJudgement() {
      const date = new Date()
      return date.getFullYear() === 2018 ? true : false
    },
    fontColor() {
      return this.error === null ? "fontColor__blue" : "fontColor__red"
    }
  }
}
</script>

<style scoped>
.fontColor__blue {
  color: blue
}
.fontColor__red {
  color: red
}
</style>
```

## フィルター
filtersを使ってデータを容易に加工できます。書き方はデータとfilters関数の間に | を挟みます。<br>
```HTML
<template>
  <div>{{ nowDate | year }}</div>
</template>

<script>
export default {
  data() {
    return {
      nowDate: new Date()
    }
  },
  filters: {
    year(val) {
      // valはnowDateが入る
      return val.getFullYear()
    }
  }
}
</script>
```
## コンポーネント
.vueのソースコードをコンポーネント化してコードの取り外しを容易にします。<br>
アラートダイアログや入力フォームなど頻繁に使用する画面はコンポーネント化することでコードを流用することができます<br>

InputForm.vueをコンポーネントとして表示します。<br>
### InputForm.vue
```html
<template>
  <div class="inputForm">
    <p><input type="text" placeholder="名前" v-model="info.name"/></p>
    <p>
      <input type="radio" value="男性" v-model="info.sex" checked/>男性
      <input type="radio" value="女性" v-model="info.sex"/>女性
    </p>
    <p><textarea rows="5" cols="30" placeholder="自己紹介" v-model="info.profile"/></p>
    <button @click="onClick">表示</button>
    <div v-if="isShow">
      <p>{{ info.name }}</p>
      <p>{{ info.sex }}</p>
      <p>{{ info.profile }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'InputForm',
  data() {
    return {
      isShow: false,
      info: {
        name: "",
        sex: "男性",
        profile: ""
      }
    }
  },
  watch: {
    info: {
      handler() {
        this.isShow = false
      },
      deep: true
    }
  },
  methods: {
    onClick() {
      this.isShow = true
    }
  }
}
</script>

<style scoped>
.inputForm {
  margin: 10px;
  background: wheat;
}
</style>
```

表示したいvueで実装したInputForm.vueをcomponentsとして定義し、HTML上に記述することで表示されます。
```html
<template>
  <div>
    <input-form />
  </div>
</template>

<script>
import InputForm from './components/InputForm.vue'

export default {
  components: {
    InputForm
  }
}
</script>
```

## コンポーネントへ値の受け渡し
値の受け渡しにはpropsとemitを使います。<br>
propsには型と初期値を定義できます。v-modelでバインディングすることでpropsのvalueに値を渡すことができます。

### InputForm.vue
```html
<template>
  <div class="inputForm">
    <p><input type="text" placeholder="名前" v-model="value.name"/></p>
    <p>
      <input type="radio" value="男性" v-model="value.sex" checked/>男性
      <input type="radio" value="女性" v-model="value.sex"/>女性
    </p>
    <p><textarea rows="5" cols="30" placeholder="自己紹介" v-model="value.profile"/></p>
    <button @click="onInit">値を初期化</button>
    <button @click="onClick">表示</button>
    <div v-if="isShow">
      <p>{{ value.name }}</p>
      <p>{{ value.sex }}</p>
      <p>{{ value.profile }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'InputForm',
  props: {
    value: {
      type: Object,
      default: null
    }
  },
  data() {
    return {
      isShow: false,
    }
  },
  watch: {
    info: {
      handler() {
        this.isShow = false
      },
      deep: true
    }
  },
  methods: {
    onInit() {
      const value = {
        name: "",
        sex: "",
        profile: ""
      }
      // emitで親（コンポーネントの呼び元）がバインドしているデータを更新することができる。
      this.$emit("input", value)
    },
    onClick() {
      this.isShow = true
    }
  }
}
</script>

<style scoped>
.inputForm {
  margin: 10px;
  background: wheat;
}
</style>
```

```html
<template>
  <div>
    <input-form v-model="info"/>
    <div>
      呼び元の値
      <p>{{ info.name }}</p>
      <p>{{ info.sex }}</p>
      <p>{{ info.profile }}</p>
    </div>
  </div>
</template>

<script>
import InputForm from './components/InputForm.vue'

export default {
  components: {
    InputForm
  },
  data() {
    return {
      info: {
        name: "エンジニア",
        sex: "男性",
        profile: "プログラムを書くのが好きです。"
      }
    }
  }
}
</script>
```


# 課題
## 課題1
Vue CLIを導入してテンプレートを作成してください。<br>
localhostを起動し、以下のような画面が表示されることを確認してください。<br>
<a href="https://imgur.com/S55Q23B"><img src="https://i.imgur.com/S55Q23B.png" width="300" height="300" /></a>

## 課題2
[課題1](##課題1)で作成した環境でToDoアプリを作成してください。<br>
ToDoアプリの仕様はこちら。<br>
- 入力フォーム、追加ボタン、削除ボタンの実装
- テキスト入力後、追加ボタンを押すと一覧表示される。なお追加ボタン押下後入力フォームを空にする
- チェックされたToDoは削除ボタンで削除できること

完成画面イメージ<br>
<a href="https://imgur.com/7tVTAVj"><img src="https://i.imgur.com/7tVTAVj.png" /></a>

## 課題３
[課題2](##課題2)で作ったToDoフォームをコンポーネント化して実装してください。<br>

[答え](../frontendkadai2)
