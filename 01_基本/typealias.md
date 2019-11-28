# typealias


## とは
- 型に別名をつける。
- 定義
```Swift
typealias 別名 = 型
```


```Swift
// Int 型
typealias SEISU = Int

let i: SEISU = 1

// タプル型
typealias TUP = (Int, String)

let t: TUP = (1, "a")

// 関数型
typealias Calc = (Int) -> Int

func foo(i: Int, calc: Calc) {
    let result = calc(i)
    print(result)
}
foo(i: 5, calc: {
   (i) in
    return i * 10
});

// 配列型
typealias IntArray = Array<Int>

var arr = IntArray()
arr.append(1)
arr.append(2)
```
