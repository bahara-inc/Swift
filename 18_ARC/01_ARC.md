# ARC

## 目次
- ARC とは
- 参照の種類
- 循環参照
- weak/unowned どっちを使う？


## ARC とは
- 「Automatic Reference Counting」 の略
- 「参照カウント」とは、あるインスタンスを参照している参照の数。参照が増えるたびに 1 ずつ増え、逆に参照が減るとその数も減る。参照カウントはその参照に nil を代入することで減る。
<small>**例.** 参照カウントの増減</small>
```Swift
var f1: Foo? = Foo() // 参照カウントは 1（f1 が Foo のインスタンスを参照）
var f2 = f1			 // 参照カウントは 2（f2 が Foo のインスタンスを参照）
f2 = nil			 // 参照カウントは 1（f2 の持っている参照を削除）
```
- 構造体や列挙型に参照カウントはない。あくまで参照型のみ。
- 参照カウントが 0 になるとそのインスタンスは破棄される。でイニシャライザも呼ばれる。
```Swift
class Foo {
    deinit {
        print("foo deinit")
    }
}
var f: Foo? = Foo()
f = nil		// ここで "foo deinit" が出力される
```

## 参照の種類
- 参照には 3 種類ある。
	- **強参照**
		- 参照カウント変わる。
	- **弱参照**
		- 参照カウント変わらない。
		- インスタンスが破棄されると参照に nil が入る。（なので型はオプショナルである必要がある）
	- **アンオウンド参照**
		- 参照カウント変わらない。
		- インスタンスが破棄されても nil にならない。ただし、破棄されたあとで参照にアクセスするとエラーになる。
- 参照の定義は何もつけないと強参照、`week` をつけると弱参照、`unowned` をつけるとアンオウンド参照。
<small>**例1.** 強参照、弱参照、アンオウンド参照</small>
```Swift
var f1 = Foo()	// 強参照
week var f2: Foo? = f1	// 弱参照
unowned var f3 = f2	// アンオウンド参照　※ f3 = Foo() だとすでに deinit されているとしてエラーになる
```
<small>**例2.** 生成したインスタンスを week に代入してもカウント0なのですぐに破棄される</small>
```Swift
week var f: Foo? = Foo()
print(f)	// nil
```

> アンオウンドって参照もちたくないけど、week にしてオプショナルで扱いたくない場合で、かつインスンタンス破棄されたあとで使うことがない状況で便利ってもの？


## 循環参照
- 2つのクラス A, B がお互いの強参照を保持している状況。この場合、お互いが参照を持っていることで破棄できずメモリリークになる。
※ 正確には、プログラム内から A, B へのアクセスが断たれた時点でリークとなる。アクセスできる間はお互いが持っている参照に nil をセットできるので。
- 循環参照を持った状態で、A と B が保持している参照をプログラム内で断ち切れなくなった段階でメモリリークになる。

<small>**例.** Foo と Bar が循環参照</small>
```Swift
class Foo {
    var bar: Bar?
}
class Bar {
    var foo: Foo?
}

var f = Foo()
var b = Bar()
f.bar = b 	// Foo のインスタンスが Bar のインスタンスを持つ
b.foo = f	// Bar のインスタンスが Foo のインスタンスを持つ
```


## weak/unowned どっちを使う？
- 参照の nil 化はオーバーヘッドがあるため、weak じゃなく unowned で済むならこちらを使うことが　Apple のドキュメントでは推奨されているらしい（書いてある場所をはっけんできていない）
- 下記ページの「Weak and Unowned References」には次のようにある。
	- キャプチャした参照は nil になるべきでない。つまりキャプチャ参照は unowned がいいって。

https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AutomaticReferenceCounting.html



## リファレンス
- [The Swift Programming Language (Swift 4.0.3): Automatic Reference Counting](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AutomaticReferenceCounting.html)
