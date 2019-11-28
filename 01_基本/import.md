# import


## import とは

```Swift
import モジュール名
import シンボルの種類 モジュール名.シンボル名

シンボルの種類：var, let, class, struct など
```
- import とは自身のモジュール外のモジュールのシンボルにアクセスできるようにするもの。
> developers だと「現在のファイル外にアクセス」とあるが、Xcode のプロジェクト内では import 宣言なしで使えるので「自身のモジュール外」と書いた。
- 「import モジュール名」の場合はそのモジュール内のシンボル全てをインポート
- モジュール内のあるシンボルだけインポートしたい場合は「import 種類 モジュール名.シンボル名」を使う。

<small>**例1.** Foundation フレームワークをまるごとインポート</small>
```Swift
import class Foundation
```

<small>**例2.** Foundation フレームワークの NSString クラスだけインポート</small>
```Swift
import class Foundation.NSString
```

## リファレンス
- [Swift オープンソース化ではじめて知った import の書き方](https://qiita.com/sora0077@github/items/11b436bea78868a185b5)
