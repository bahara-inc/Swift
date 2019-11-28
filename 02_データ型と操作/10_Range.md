# 範囲

## 目次
- 基本
- 範囲型のまとめ
- 範囲型でできること
- 部分文字列と範囲


## 用語
- オフセット(offset)
	- 基準となるある点からの相対的な位置


## 基本
- 「a から b まで」のように範囲を表す。
- 範囲を表す型は 5 つあり、型変数に a, b の要素の型をとる。下記の宣言を見るとわかる通り「要素が `Comparable` であれば範囲型のインスタンスを生成できる。
	```Swift
	struct Range<Bound> where Bound : Comparable
	struct ClosedRange<Bound> where Bound : Comparable
	struct PartialRangeThrough<Bound> where Bound : Comparable
	struct PartialRangeFrom<Bound> where Bound : Comparable
	struct PartialRangeUpTo<Bound> where Bound : Comparable
	```
- 要素が `Strideable` かつステップが整数の場合はその `Range` / `ClosedRange` に別名が使える。
	```Swift
	typealias CountableClosedRange<Bound> = ClosedRange<Bound> where Bound : Strideable, Bound.Stride : SignedInteger
	typealias CountableRange<Bound> = Range<Bound> where Bound : Strideable, Bound.Stride : SignedInteger
	```
	```Swift
	let r1: CountableRange<Int> = 1..<5
	let r2: Range<Int> = 1..<5
	```
- 範囲型に関わるプロトコル
	- Comparable
		- 関係演算子（<、<=、>=、>）を使用して比較することができる。
		- 適合するには、関係演算子を実装。
	- Strideable
		- 1 次元の値で「n ずつ」のように値をステップすることができる。
		- `Strideable` は `Comparable` を継承してる。
		- 適合するには 2 つを実装
			- `advanced(by:)` : 指定した by だけオフセットした値を返す
			- `distance(to:)` : to までの距離を `Stride` 型として返す
- 範囲型のインスタンスは『 **範囲演算子** 』で生成できる。範囲演算子は 2 種類あるのと、それぞれに要素が片方だけの記述がある。
	- 範囲演算子
		- 閉鎖範囲演算子
			- a...b
			- ...b
			- a...
		- 半開放範囲演算子
			- a..<b
			- ..<b


## 範囲型のまとめ

|範囲|範囲演算子|型|
|:--|:--|:--|
|a 〜 b|a...b|ClosedRange|
|a 〜 b（b含まない）|a..<b|Range|
|a 〜 |a...|PartialRangeFrom|
|〜 b|...b|PartialRangeThrough|
|〜 b（b含まない）|..<b|PartialRangeUpTo|

```Swift
print(type(of: "a"..."c"))  // ClosedRange<String>
print(type(of: 1...5))  // ClosedRange<Int>
print(type(of: 1..<5))  // Range<Int>
print(type(of: ...5))  // PartialRangeThrough<Int>
print(type(of: ..<5))  // PartialRangeUpTo<Int>
print(type(of: 1...))  // PartialRangeFrom<Int>
```


## 範囲型でできること
- 範囲の最初と最後の要素の取得
	```Swift
	print(x.first) // Optional(1)
	print(x.last) // Optional(5)
	```
- 範囲にある要素が含まれるかの判定
	```Swift
	print(x.contains(3)) // true
	```
- 範囲の要素でループ（範囲が Countable の場合のみできる）
	```Swift
	for a in 1..<5 {
    	print(a)
	}
	```


## 部分文字列と範囲
- `String` には下記の添字があって、`String.Index` を要素とした範囲で部分文字列を取得できる。
	```Swift
	subscript(Range<String.Index>) -> Substring
	```

```Swift
let str = "abcde"
let a = str.index(str.startIndex, offsetBy: 2)  // 先頭からオフセット2の String.Index
let b = str.index(str.startIndex, offsetBy: 4)  // 先頭からオフセット4の String.Index
print(str[a...b]) // cde
```
