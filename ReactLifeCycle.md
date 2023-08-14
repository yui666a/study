# Goal

* reactのライフサイクルを理解する
* reactでのコンポーネントの書き方のトレンドがクラス → 関数へと移り変わっていった理由を知る
* 関数コンポーネントの良さを知る
* 再レンダリングの仕組みを理解する
* 再レンダリングを最適化する術を理解する

# reactでいうライフサイクルとは？

`キーワード: mounting, updating, unmounting, コンポーネント, 仮想dom, useEffect`


## 参考リンク
* https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/ 
* https://ja.reactjs.org/docs/react-component.html#the-component-lifecycle 
* https://zenn.dev/yodaka/articles/7c3dca006eba7d 


## クラスコンポーネント
https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/ 


*	マウント
  * 一番最初にコンポーネントが初期化され、DOMが描画され、その後のフック（componentDidMount）が呼び出される一連の流れのこと。
  * つまり、この時点で１度目の描画がされている
  * 使用する関数
    * constructor (初期化)
    * render （描画）
    * componentDidMount (タイミングを指定した関数)
      * DOMが描画された後に実行される。
        * つまりここからは仮想DOMの話になる？？
      * APIリクエスト、描画されたDOMにアクセスする必要のある処理などをここに書く
      * 更新時には実行されない。
* 更新
  * propsやstateが変更された時に、DOMが再描画され、
  * その後のフック（componentDidUpdate）が呼び出される一連の流れのこと。
  * 使用する関数
    * render
    * componentDidUpdate (タイミングを指定した関数)
      * state,propsが変化した更新時に、DOMが描画された後に実行される
      * マウント時には実行されない
* アンマウント
  * componentWillUnmountが呼ばれた後、マウントが解除（インスタンスが死に、DOMが画面から消える）されるまでの一連の流れのこと。
  * 使用する関数
    * componentWillUnmount
      * コンポーネントが破棄される前に呼ばれる
      * setIntervalなどの後片付けに使われる

## 関数コンポーネント

### 下で説明するための用語
1. 関数の中  
  setStateやメソッドの定義など初期化を行う  
  この時点でDOMは描画されていないと思いきや、更新時は描画されていることがあるが、それはすでにReactの手を離れた描画済みのものであり、ここでDOMを参照することはできない。  
  副作用は必ずuseEffectを使う（相曽は未確認）  
2. 関数の return  
  ブラウザにDOMを表示する。  
  もしくはDOMに描画済みのデータを更新する。  
3. useEffect の中  
  副作用を起こす（依存配列が指定されている場合はそれによって実行されるかされないかが変わる）  
  componentDidMountとcomponentDidUpdateが一緒になった的な位置付けだが厳密には違う。  
4. useEffect の中の return  
  副作用のクリーンアップがされる  
  componentWillUnmount的な位置付けだが厳密には違う。  

[このHook Flowの画像](https://raw.githubusercontent.com/donavon/hook-flow/master/hook-flow.png)をもとに説明する。  

### Step
* マウント
  * 関数の中
  * 関数のreturn
  * useEffect
* 更新
  * 関数の中
  * 関数のreturn
  * useEffect の return
  * useEffect
* アンマウント
  * useEffect の return


## クラスコンポーネント と 関数コンポーネントの違い
* constructorはマウント時にしか呼ばれないが、initializeは更新時にも呼ばれる
* componentWillUnmountはアンマウント時にしか呼ばれないが、
useEffect の中の return は更新時にeffectが実行されるならその前に呼ばれる
* 関数コンポーネントはクラスコンポーネントのrender関数そのものらしい
  * https://www.robinwieruch.de/react-function-component/#:~:text=Note%3A%20If%20you%20are%20familiar,returns%20JSX%20in%20the%20end.&text=%7D-,export%20default%20App%3B,function%20as%20Child%20Component%20now 

# reactでのコンポーネントの書き方はクラス → 関数へと移り変わってきた理由は？
`キーワード: lifecycle、hooks、state、useEffect`

## 参考リンク
* https://qiita.com/shane/items/b936550820de9a88ad60
* https://zenn.dev/swata_dev/articles/7f8ef4333057d7 
* https://zenn.dev/yodaka/articles/7c3dca006eba7d 

1.	コードが綺麗で簡潔になる
2.	life cycle を気にする必要が少なくなる

関数コンポーネントは初めはあまり聞かなかった印象（React公式ドキュメントもつい最近までクラスコンポーネントだった）

クラスコンポーネントでは以下の２つの管理が可能だったが、関数コンポーネントではできなかった。

* State
* Lifecycle Hooks

しかし、useState や useEffect などの React Hooksが導入されたことで、関数コンポーネントでも管理が可能になり、「コードが綺麗で簡潔になる」メリットが大きくなったため、遷移していった？


# reactで再レンダリングされるタイミングは？

`キーワード: state, props, 親コンポーネント`

## 参考リンク
* https://zenn.dev/b1essk/articles/react-re-rendering 
* https://dev.to/adevnadia/react-re-renders-guide-why-components-re-render-4ml 


* stateが更新された時
  * useState で保持する state が更新されたときに関数コンポーネント全体が再実行される。
* propsが更新された時
  * 親から渡される props が更新された時も全体が再実行される
* 親コンポーネントが再レンダリングされた時
  * 自分コンポーネント内で状態として認識することはできない

# useMemo, useCallback, React.memoについて。またそれぞれの役割の違いについて

`キーワード: useMemo, useCallback, React.memo`

## 参考リンク
* https://qiita.com/soarflat/items/b9d3d17b8ab1f5dbfed2 
* https://sagantaf.hatenablog.com/entry/2021/11/13/192110 
* https://blog.uhy.ooo/entry/2021-02-23/usecallback-custom-hooks/ 
* https://qiita.com/uhyo/items/5258e04aba380531455a 

## useMemo

関数コンポーネントの中の処理やコンポーネントの一部をメモ化する

> 今いる関数の外を見に行くな。そのuseMemoが今または将来に役に立つ可能性が1%でもあるなら使え

## memo 
関数コンポーネント自体をメモ化する

親コンポーネントから渡されるpropsが変化すると memo 再レンダリングされる

## useCallback
memo した関数を使うときには useCallbackを合わせて使う

親コンポーネントから関数を渡すとき、関数が再作成されてしまっているため memo の意味がなくなってしまう。 親コンポーネントの関数を useCallback することで回避する

基本的には memo と useCallback は親子のコンポーネントで ペアで使うことが多い
