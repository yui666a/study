# Reactとは？

## 参考リンク
* https://ja.legacy.reactjs.org/ 
* https://developer.mozilla.org/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/React_getting_started 

## 概要
* UIを構築するためのライブラリー（フレームワークではない）
* Webアプリとして使うためには、React と ReactDomが必要
  * モバイルには React Native
  * XR には React360 がある
* React の目標は「開発者が UI を構築しているときに発生するバグを最小限に抑えること」
* これまでに述べてきた DOM などレンダリングに関することを開発者が気にせずに、UIだけ考えられるようにバックで処理してくれる
* インタラクティブな UI をwebで作ろうとすると、これまでは全画面が再レンダリングされていたが、 React が変更部分を自動で検知することで関連するコンポーネントだけを更新・描画する
  * だから、コンポーネントを細かく分けたり、useMemo を使うことが重要になる

# VueやReactなどのライブラリはそれ以前のjQueryなどのライブラリと比べて何が画期的？

## 参考リンク
* https://note.com/tabelog_frontend/n/n1094967cb6bd 
* https://zenn.dev/arei/articles/f59e263aa3edf2 
* https://scrapbox.io/nwtgck/%E5%91%BD%E4%BB%A4%E7%9A%84%E3%81%A8%E5%AE%A3%E8%A8%80%E7%9A%84%E3%82%92%E7%90%86%E8%A7%A3%E3%81%99%E3%82%8B%E3%81%9F%E3%82%81%E3%81%AE%E3%82%B7%E3%83%B3%E3%83%97%E3%83%AB%E3%81%AA%E4%BE%8B 

## 概要
*	命令的制御から宣言的制御に変わった ことが大きかった

## 宣言的制御
*	状態を定義する書き方
*	実装がシンプル（変更容易性の向上）
*	実装がシンプルだとテストもシンプル（障害発生率の低下）
*	変更に強い（開発生産性の向上）
*	バグが起こりにくい


``` *.vue

<template>
  <div>
    <input type="text" :value="message" @input="inputFunc">

    <p>「{{ message }}」という値が入力されました。</p>
    <p>{{ message.length }}</p>
  </div>
</template>

export default {
  data() {
    return {
      message: ""
    };
  },
  methods: {
    inputFunc(event) {
      this.message = event.target.value
    }
  }
};
```

=> Template のように実際の生成されるhtml に近いものを実装することができて、イメージしやすい

## 命令的制御

*	動作の手順（状態の前後関係）を指定する書き方
*	細かな挙動まで指定できるので、命令的制御の方が柔軟性はある
*	手数が多くて実装が大変（開発生産性の低下）
*	手数が多くて実装が大変だとテストも大変 （障害発生率の上昇）
*	変更が大変（変更容易性の低下）

``` *.js
const input = document.querySelector(".input");
input.addEventListener("input", inputFunc);

const p = document.getElementsByTagName("p");
function inputFunc(event) {
  const message = event.target.value;
  // 入力反映
  const p1 = p[0];
  p.innerText = "「" + message + "」という値が入力されました。";

  // 文字数カウント
  const p2 = p[1];
  p2.innerText = message.length;
}
```


# 現在、ReactはVueなどを大きく突き放し、デファクトスタンダードと化している理由を考察してみる

## 参考リンク
* https://npmtrends.com/angular-vs-react-vs-vue 
* https://zenn.dev/marokanatani/articles/compare_vue_and_react 
* https://qiita.com/yoichiwo7/items/236b6535695ea67b4fbe 
* https://nextjs.org/ 

## トレンドチャート
* npm trends の 2022/11 ~ 2022/12 の異常なvue トレンドはバグなので、気にしない
* 現在 React は vue の 5 倍近くのダウンロードがされている

## Vue

* 小〜中規模向け
* FE初心者でも初速が早い

## React
* 中〜大規模アプリにも対応しやすい
* JSX を採用しているため これまでの基本的な HTML/CSSとの乖離が少ない
* TS のサポートがVueより強い
* 単方向データバインディングがメインのため、双方向データバインディングができるvueより使いにくい反面、保守性が高まる
* 開発サポート系のモジュールはReactを先行してサポートしがち（メジャーだからでは？）
https://zenn.dev/sa2knight/articles/why_react_folks_dont_choose_vue 

## 結論
Vue3 と React FC を比較すると、そこまで大差はない気がする。
しかし、Vue2の未成熟な時の印象を持ったミドルエンジニア以上は vue を避けがち、新しくJSライブラリを導入する人はメジャーで技術記事も多いことから React を使うため格差が広がっていくのではないかと思った。
