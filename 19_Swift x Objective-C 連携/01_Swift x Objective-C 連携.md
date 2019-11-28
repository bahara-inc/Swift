# Objective-C との連携

## 目次
- Objective-C と Swift の連携
- ブリッジングヘッダ
- 自動生成ヘッダ
- @objc ディレクティブ
- Objective-C から利用できない Swift のコード
- Objective-C から Swift のクラスを利用
- Swift から Objective-C のクラスを利用



## メモ
- 1 つのファイルで Objective-C と Swift の言語は使えない。ファイル別なら使える。

## Objective-C と Swift の連携
- プロジェクト内に Objective-C と Swift のコードを共存させ互いを使うことができるが、1 つのソースファイル内で Objective-C と Swift を一緒には使えない。
- 連携はお互いを参照できるようにするヘッダファイルを用意するだけでできる。
	- Objective-C から Swift を使う
```
Objective-C → ヘッダファイル（ブリッジングヘッダ）→ Swift
```
	- Swift から Objective-C を使う
```
Swift → ヘッダファイル（自動生成ヘッダ）→ Objective-C
```
- ヘッダファイルは Objective-C と Swift でそれぞれ別のものが必要。
	- <font color="teal">**ブリッジングヘッダ**</font>
		- Objective-C のコードを Swift から参照できるようにするヘッダファイル。
	- <font color="teal">**自動生成ヘッダ**</font>
		- Swift のコードを Objective-C から参照できるようにするヘッダファイル。

## ブリッジングヘッダ
- Objective-C のコードを Swift から参照できるようにするヘッダファイル。
- ブリッジングヘッダを使うには、<u>ファイルの用意</u>と<u>プロジェクトにファイル名を設定</u>する必要がある。

#### 作成
- このヘッダファイルを作成する方法は 2 つ。
	1. 自分でファイルを作る。
	2. Swift ファイルを新規で作る時にでるダイアログで YES を押して自動で作る。
- ファイル名は何でもよいが自動生成すると「<u>{プロジェクト名}-Bridging-Header.h</u>」という名前になる。
- ファイルの中身は Swift から参照したい Objective-C のヘッダファイルの import が並ぶ形になっている。<br>
<small>**例.** ブリッジングヘッダの例</small><br>
*MyApp-Bridging-Header.h*
```Objective-C
#import "MyViewController.h"
#import "MyData.h"
```
><small>ここに書くのはプラットフォームのやつはよくて、独自で作ったやつだけ。プラットフォームのやつはどこかにヘッダが別である？</small>

#### 設定
- プロジェクト設定にある「<u>Build Setting > Objective-C Bridging-Header</u>」の項目にヘッダファイルのファイル名を指定すればよい。

## 自動生成ヘッダ
- Swift のコードを Objective-C から参照できるようにするヘッダファイル。
- 自動生成ヘッダを使うには<u>明示的な作成も設定も不要</u>。自動生成ヘッダは<u>ビルド時に作業領域に自動で作られ</u>、プロジェクト内には表示されない。
- 自動生成ヘッダに Objective-C から参照したい Swift コードを登録するには Swift 側のコードの該当箇所に「<font color="teal">**@objc ディレクティブ**</font>」を付けるだけでよい。
- 実装ファイル(\*.m) にインポートする。ヘッダファイル（\*.h）にインポートしてはいけない。


## @objc ディレクティブ
- Swift 側のコードにつけて、それを Objective-C で利用できるようにするもの。
- `@objc` を利用するには Foundation フレームワークが必要。
- `@objc` は次の宣言につけられる。
	- クラス
	- プロトコル
	- メソッド
	- プロパティ
	- イニシャライザ
	- など
- `NSObject` を継承したクラスはクラスに `@objc` が暗黙的に付く。（付いたと同じになるが正しい？）
- `@IBOutlet`、`@IBAction` をつけたプロパティ、メソッドは暗黙的に `@objc` がつけられる。

><small>NSObject を継承しないクラスに @objc つけたら「Only Classes that inherit from NSObject can be declared @objc」ってなったけど NSObject のサブクラスじゃないと連携できなくなった？</small>

## Objective-C から利用できない Swift のコード
- ジェネリクス
- タプル
- Swift で定義された次のもの
	- 構造体
	- private メンバ
	- など


## Objective-C から Swift のクラスを利用
- Swift コード側の作業
	1. Objective-C から使いたい Swift コードの該当クラスやメンバに `@objc` を付ける。
- Objective-C 側の作業
	1. 実装ファイル（\*.m）に自動生成ヘッダを import する。
	2. Swift のクラスを使ってコードを書く。

<small>**例.** Swift の MySwiftClass を Objective-C で使う</small>

*MySwiftClass.swift*
```Swift
import Foundation

@objc(MySwiftClass)	// 親が NSObject なのでこの @objc は省略できる
class MySwiftClass: NSObject {
    func say() {
        print("hoge")
    }
}
```
*ViewController.m*
```Swift
#import "ViewController.h"
#import "Sample-Swift.h"
...
- (void)viewDidLoad {
	[super viewDidLoad];

	MySwiftClass *cls = [[MySwiftClass alloc] init];
    [cls say];
}
```

## Objective-C から Swift のプロトコルを利用

comming soon...

## Swift から Objective-C のクラスを利用
- Objective-C コード側の作業
	- とくになし
-  Swift コード側の作業
	1. ブリッジングヘッダに Objective-C で使いたいコードのヘッダを import する。
	2. Objective-C のクラスを使ってコードを書く。


<small>**例.** Objective-C の MyObjcClass を Swift で使う</small>

*MyObjcClass.h*
```objc
#import <Foundation/Foundation.h>

@interface MyObjcClass : NSObject
- (void)hoge;
@end
```

*MyObjcClass.m*
```objc
#import "MyObjc.h"

@implementation MyObjcClass
-(void)hoge
{
    NSLog(@"aaaa");
}
@end
```

*Sample-Bridging-Header.h*
```objc
#import "MyObjcClass.h"
```

*Sample.swift*
```swift
...
func test() {
    let a = MyObjcClass()
    a.hoge()
}
```


## Tips
- Objective-C の ヘッダを Swift 文法にして見る。
	- ヘッダファイルを選択し、「View > Standard Editor > Show Related Items」
