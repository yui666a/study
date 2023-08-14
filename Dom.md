# DOMとは？

## 参考リンク
* https://developer.mozilla.org/ja/docs/Web/API/Document_Object_Model/Introduction 
* https://www.javadrive.jp/javascript/dom/index1.html 

## 概要

* Document Object Model の略
*	ウェブ文書のためのプログラミングインターフェイス
  * ウェブ文書なので、 html のほかに xml も扱うことができる
* 文書をノードとオブジェクトで表現することで、プログラミング言語をページに接続する役割を持つ
* 特定のプログラミング言語から独立し、文書の構造表現を単一の一貫した API から利用できるように設計されている（＝JSに依存しているわけではない）

## 基本的なデータ型

```
Node = {
  Document: Element | Attr | CharacterData;   // tree 構造のための要素
  Element: 実際に表示させるものについての情報を持つ;（"hello" などのstringなど）
  NodeList: Node[];
  Attr: 属性のインターフェイスを実装したオブジェクトの参照;
  NamedNodeMap: 辞書型のNode
}
```

* Dom: Node[];  // これが DOM
* 要素Node: html タグや body, p タグなど
* テキストNode: 「〇〇の作り方 3選」などのテキスト
* 属性Node: id や class など
* DOM ← Node ← Element の継承関係
* 空白や改行は 「空白Node」として存在する
  * 下の例では、1つ目と2つ目では空白Nodeの存在の有無が異なり Node数に違いがあるため、注意する必要がある

```
<div><p>ABC</p></div>
```

```
<div>
  <p>
    ABC
  </p>
</div>
```

## 感想
* TS の型指定で表現するものも、DOM API の一種のことがある？
  * ReactNode や HtmlElement など
    * ReactNode は仮想DOMの方？

# 仮想DOMとは？使用するメリットと合わせて

## 参考リンク
* https://zenn.dev/ryofrontenginer/articles/edfce254795efc#%E4%BB%AE%E6%83%B3dom%E3%81%A3%E3%81%A6%E4%BD%95%E3%82%82%E3%81%AE%E3%81%AA%E3%81%AE%EF%BC%9F 

## 概要
*	Javascriptのオブジェクトで Dom を模倣したもの
*	React や Vue で使われている
*	これまでのDOMでは、ページ遷移や状態が変化した場合、１から全て書き直していた
*	本物のDOMをコピーしたJS の「変更前仮想DOM」と「状態変化後仮想DOM」 の差分検知(diff)することで、局所的な再レンダリング(patch)に抑え、描画速度に寄与しやすくなる
o	仮想DOMを使う場合は、「差分検知 + 再レンダリング」が必要なため、逆に処理コストが増えてしまう可能性もある

## メリット
*	UI（見た目）とロジックを分離することができる
*	状態を管理することが簡単になる
*	UIとロジックを繋ぐ処理が簡単になる
*	フレームワークを使ってコンポーネント化しているので、クリック時のイベント等が増えても管理（メンテナンス）がしやすい（？https://zenn.dev/furum/articles/4adf46d19d3051#%E3%83%A1%E3%83%AA%E3%83%83%E3%83%88 ）
*	ミニアプリだと、普通のDOM操作の方が早い場合もあるらしい
*	最近よく見る Solid や Svelte は Virtual DOM を使っていないことを売りにしている（https://engineering.monstar-lab.com/jp/post/2022/05/27/Is-Virtual-DOM-Outdated/ ）

