# FEでレンダリングする方法と特徴

`キーワード CSR, SSR, SG, SSG, Next.js, Hydration, ISR, React Server Component, Vercel, Code Spliting, パフォーマンス, Dynamic Routing`

## 参考リンク
* https://web.dev/rendering-on-the-web/?hl=ja 
* https://nextjs.org/docs/pages/building-your-application/rendering 
* https://zenn.dev/akino/articles/78479998efef55
* https://blog.arthur1.dev/entry/2023/06/04/184745 

## 概要

### CSR

* 生のReact
* 速度はクライアントのデバイススペックに依存する

### SSR

* デフォルトのNext.js
* サーバー側でレンダリングするようになったため、速度は安定するが、都度レンダリングしている。
* 秘匿情報が含まれたページがCDNにキャッシュされた場合、個人情報の漏洩などに繋がる。（Aさんの情報がBさんにも見えちゃう場合が）その為、色々な事を考慮するとキャッシュ設定が面倒。
https://tech.012grp.co.jp/entry/2021/03/25/125014 

### SSG

* S3,GCSなどに置けるため安上がり？
* サーバサイドですでに生成したhtml,js を使う。
* 各ページごとにhtmlを生成する必要があるため、ファイル数が増える？
* でも、ビルド時にデータを取得してきて生成しているため、あらかじめAmazon ECなどのような大量の全データを全データを取ってくる事はありえない
* ツイッターみたいにデータが頻繁に変わるようなアプリは適さない？

### ダイナミックrouting 

* 直にリンクに対応させるためには今は  
  * `404` → パスパラメータから取得してリダイレクト
* をしている？

### SG

* クライアントからリクエストがあったときに初めて生成する
* その後他のユーザから同じリクエストがあった時は生成したjs, html を返す。
* １度しか生成しない
* Node 環境のサーバが必要になる
* でも、ビルド時にデータを取得してきて生成しているため、あらかじめAmazon ECなどのような大量の全データを全データを取ってくる事はありえない
* ツイッターみたいにデータが頻繁に変わるようなアプリは適さない？

### SGとSSGの違い

* SSG はビルドして export しているため、node 環境も必要ない
* SGは node が必要な環境でgetStaticProps 関数を用い、ページ生成に必要なデータをビルド時にあらかじめリクエストしておく手法

* SSG: 静的なhtmlがページ数分生成されている
* SG: 静的なhtmlがページ数分生成されていない　[id].tsx などにも対応できる

Next.js は SG と SSRを部分的にも選択することができる

### Hydration 

* SSRにおいてサーバサイドでReactが生成したDOMを、renderToString()などで「文字列化」するのが脱水化(Dehydrate)で、それをうけとったブラウザがHTMLとしてパースして復活させたDOMの最後の段階でイベントを受けとれる生きたDOMとして再開させることをhydrateと呼びます。
* https://zenn.dev/highgrenade/scraps/04564fa1496b82


SSRはインタラクティブなイベントがあったときに、都度サーバに読み取らないといけなかった。
* CSRでデータを書き換えられるようにしたもの
* どういうこと？？？
* selective hydration
  * react suspenseにも関係する
    * `<Suspense component={<Loading />} ></Suspense>` みたいなやつ
  * island architecture にも関係する

### ISR: Incremental Static Regeneration

* 定期的にサーバ側でデータをフェッチして最新のhtml に置き換えておく
* SSG の挙動に加えて、一定時間ごとにバックグラウンドでデータの再取得およびページの再レンダリングを行い、HTML を再生成 (regenerate) する手法

本当の最新にならないことに注意

* SSR よりサーバの負担が低い
* node 環境がないと不可能？　ーー＞　現段階ではVercel依存。らしい
　yahoo トップページ などがいい感じに使える？


### React Server Component

* `.server.js` `.client.js` `.js` がある
* react hooks が使えなくなることに注意
* Code Splitting が生きるのは Reactだと思う。 SSGしたら意味ないのでは？
* Dynamic Routing:必要になったときにリクエストする方法。 SGは元からできている気がする。

ダイナミックimportとダイナミックルーティングの違い
* 必要になったときに都度取ってくるのがダイナミックimport
* ダイナミックルーティング：必要になるかわからないページはビルド時にそのままにしておく。 [id].tsxなど
