---
theme: default
class: text-lg, text-slate-900
highlighter: shiki
title: Personlink Frontend Study
downlaod: true
favicon: './public/personlink-logo.png'
---

# Personlink Frontend Study

Vue3とTypeScriptの雰囲気を知ろう

---

# アジェンダ

- TypeScriptとは？
- TypeScriptメリット・デメリット
- TypScript型基本
- TypeScriptでの型定義の仕方
- Vueとは
- ツール紹介
- Vue + TSでの実装コードを見る
- Vueを使う上でのTips

<style>
  ul {
    margin-top: 2rem;
    margin-left: 2rem;
  }
</style>
---

# TypeScriptとは？
MicroSoftによって開発されているプログラミング言語の一つ。
AltJS(Alternative Javascript)の一つです。


AltJSとはJavaScriptの代替言語という意味で、現在はTypeScriptが主流でAltJSという言葉そのものを聞かなくなってしまいましたが、CoffeeScriptや、Dartなどが存在しています。

TypeScriptの用途はJavaScriptと同じであり、サーバーサイド、フロントエンドと幅広く使うことができます。

JavaScriptと違う点は、TypeScriptにはtsc(typescript compiler)を備えていて、プログラムによる型チェックが行われるようになります。
型チェックが行われることによって、型に関連されるプログラムはコンパイルエラーとして検出されるようになります。

<style>
  .slidev-layout p {
    opacity: 1;
    margin-top: 30px;
  }
</style>
---

# TypeScriptのメリット・デメリット

## メリット

- **型安全に開発することができる**(型によるバグが発生しづらい)
- プログラム中に型を記述することによって、可読性が上がる(プログラムが読みやすくなる)
- 入力補完がされるようになり、開発効率が上がる

## デメリット

- 環境構築が面倒くさい
- tsconfigの設定が多すぎて、よくわからなくなりがち(とりあえず`strict`は`true`にしておこう)
- 最初のうちは型定義がめんどくさいと感じることがある


<style>
  .slidev-layout h2 {
    margin-top: 30px;
  }

  ul {
    margin-left: 30px;
  }
</style>

---

# TypeScriptの型基本

TypeScriptの型は、プリミティブとオブジェクトに大別されます。

### プリミティブ
プリミティブには、**最も基本的な値**を表す用語として使用されているらしく、現状下記の種類の型が存在します。

- 文字列(string)
- 数値(number)
- 真偽値(boolean)
- BigInt
- null
- undefined
- Symbol

今回はBigInt, Symbolの説明はしませんので、もし気になったら各自で調べてみてください。

<style>
  .slidev-layout {
    font-size: 0.9rem;
  }
  .slidev-layout p {
    opacity: 1;
    margin-top: 30px;
  }

  ul {
    margin-left: 30px;
  }
</style>
---

# TypeScriptの型基本

### オブジェクト
TypeScriptのオブジェクトは**いくつかの値をまとめたデータ**です。

```ts
const obj = {
  foo: 1,
  bar: 'bar'
}
```

上記のように、`{key: value}` 形式で記述することで、値をひとまとめにすることができます。
オブジェクトの一つ一つ中身の値のことを**プロパティ**と呼んだりします。

TypeScriptはプリミティブ型とオブジェクト型に分類されるため、配列もオブジェクトの一つです。
配列の一つ一つ中身の値のことを**要素**と呼んだりします。
---

# TypeScriptでの型定義の仕方

### プリミティブ

```ts
const str: string = 'foo'
const num: number = 1
const bool: boolean = false
const nil: null = null
const undefined: undefined = undefined
```

コロンの後に型を記述することができる。**型注釈**とも呼ばれます。
型注釈を記述することによって、その型以外の値の代入ができなくなります。

型注釈デモ

https://www.typescriptlang.org/play?#code/MYewdgzgLgBAhgLhmArgWwEYFMBOMC8MAzAAwkBQoksGSqmuBMALGZeNDMEtDgJZgA5kwDkAVjIjy5APQyuHWBHR102PITgwA1DAyz5VCCAA2WAHQmQggBTK0ASmlyF1GPdUMN8HXt-AYFwArCEADBkALBkAKhkBLhkAfhkAZBkAvN0B8V0BtBkAtBkBrBkAvxUBspUBVBkAYhkAzBkB1BkA-BkAxBgNXYzNLazt0J2koAE8ABywYFDA+cFE4ERgAHxgRDCkjWF7+sABlODQOsyRZgcIRKXIzJSgcHn2BYU2AMxAQKV4mCl3kFXuvJgBGcnpRZ6k7jAuTJFO4CYIN1CACgVhyD9TExUCYTDssLA+n97nCYSg4W8+CYPl9ET0wAATLBrIlYU4CLCEpi9YkUsBU8i0kHjIbkIA

<style>
  .slidev-layout h3 {
    margin-top: 30px;
  }
</style>

---

# TypeScriptでの型定義の仕方
### オブジェクト

```ts
type Obj = {
  foo: string
  bar: number
}

interface Obj {
  foo: string
  bar: number
}

const obj: Obj = {
  foo: 'foo',
  bar: 1
}
const str: string[] = []
const num: number[] = []
const objArray: Obj[] = []
```

`interface`か`type`を使って、オブジェクトの型宣言ができます。

昔のバージョンでは、`interface`でしかオブジェクトの型宣言ができませんでしたが、`type`でも型宣言を行うことができます。

`type`の方が型宣言においてできることが多く、プロジェクトによっては、型宣言は`type`のみを使うところもあるそうです。

<style>
  .slidev-layout h3 {
    margin: 15px 0;
  }
  .slidev-layout {
    font-size: 0.9rem;
  }

</style>

---

# Vueとは 

ユーザーインターフェース(ページの見た目)を構築するためのフレームワークです。

HTMLで見た目を記述し、JavaScriptを使ってサイトに動きをつけていきます。

Vueがすごいのは、宣言的に見た目を構築していけるところにあります。

`button`等のUIに対して下記のように、直接実現したいことを記述することで、その処理を行うことができます。

```vue
<button @click="alert('click')">
  click me
</button>
```

<SampleButton />

<Counter />

<style>
  .slidev-layout p {
    opacity: 1;
  }
  .slidev-layout h1 {
    margin-bottom: 30px;
  }
  .slidev-layout button {
    margin-top: 30px;
  }
</style>

---

# ツール紹介

- Vite(高速なホットリロード、ビルド、ローカルサーバ立ち上げを提供してくれるツール)
- Tailwindcss(インラインでスタイリングできる)
- eslint(コード規約を制定してくれるツール。違反するとエディタ状にエラー警告が表示される)
- Vue3(Composition API)
- VueRouter(SPA構成のアプリケーションにおいて、コンポーネントごとに擬似的なURLを生成してViewを返却してくれるやつ)
- Pinia(TypeScriptで書かれているtype safetyなStore)
- Vitest(Vite上で動作するテストツール(jestよりも高速))
- slidev(今みなさんがみているスライドを作成するためのツール)

---

# Vue + TSでの実装コードを見る

着目して欲しい点
- `v-model`
  - Formによく使用する機能です。これを使用すると**双方向データバインディング**を付与することができます。
  - 双方向データバインディングを使用することで、Formに入力される値がそのままView(見た目)にも反映されます。そのためリアルタイムで値の監視ができるようになり、バリデーションチェックや不正な値が入力された際に役に立ちます。
- `prop`
  - 親コンポーネントが持っている情報を子コンポーネントに渡すことができ、子コンポーネント側でその情報を受け取る際に使用するもの
- `emit`
  - 子から親に対してイベントを実行させたいときに使用するもの
- `ref`, `reactive`
  - 特定の変数に対して、リアルタイムに画面を描画させたいときに使用するもの
- `v-on:*`, `@*`
  - 要素(buttonやコンポーネント)に対してイベントリスナを付与することができる。例えば`v-on:click`ならクリックイベントリスナーを付与できる

<Todo />

<style>
  .slidev-layout h1 {
    margin-bottom: 30px;
  }
  .slidev-layout {
    font-size: 0.9rem;
  }
</style>
---

# 参考リソース
- Vue3公式ドキュメント(https://v3.ja.vuejs.org/)

# 勉強会のために作成したリポジトリ
- https://github.com/yuto-tomita/vue3-ts-todo
- https://github.com/yuto-tomita/person-link-front-end-study-session-materials

<style>
  h1 {
    margin-top: 30px;
  }
</style>
---
