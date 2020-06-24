---
description: 當操作任何物件時就是在操作記憶體，到底是如何運作的呢？
---

# Value / Reference Type

## 三種記憶體區間

![&#x4E09;&#x5927;&#x5340;&#x584A;](../.gitbook/assets/memory.jpg)

* static memory：全域或靜態變數，永遠存在的變數\(Swift中用特殊機制Lazy初始化\)
* stack memory：local 變數，自動回收
* heap memory：dynamic 變數，使用者自己決定何時回收\(Swift中用ARC處理\)
* 參考連結：
  * [C 語言程式記憶體配置概念](https://blog.gtwang.org/programming/memory-layout-of-c-program/)
  * [stack vs heap：執行時期儲存兩大要角](https://antrash.pixnet.net/blog/post/70456505)
  * [stack 與 heap 的底層概念](https://nwpie.blogspot.com/2017/05/5-stack-heap.html)
  * [MEMORY IN C – THE STACK, THE HEAP, AND STATIC](https://craftofcoding.wordpress.com/2015/12/07/memory-in-c-the-stack-the-heap-and-static/)

![&#x8A18;&#x61B6;&#x9AD4;&#x5340;&#x584A;&#x7684;&#x64CD;&#x4F5C;](../.gitbook/assets/heap_stack.jpg)

## Swift裡的Value Type與Reference Type <a id="a395"></a>

![](../.gitbook/assets/1_duhwiv0rpm0v97peqvlr7a.png)

* Value Type：Int, String, Array, Dict, Enum, Struct =&gt; Pass By Value，都是新的一份資料
* Reference Type：Class, Closure =&gt; Pass By Reference，都是同一份資料
* 參考連結：[Value Type vs. Reference Type](https://medium.com/@wuufone/%E5%AD%B8%E6%9C%83-swift-%E7%9A%84%E9%97%9C%E9%8D%B5-value-type-vs-reference-type-50d3034596a8)

## 練習題\(可以在playground上跑\)

```swift
struct Car{
    var number = 0
    init(number: Int) {
        self.number = number
    }
}
var car0 = Car(number: 0)
var car1 = Car(number: 1)
var cars = [car0, car1]
var newCars = cars
cars.sort { $0.number > $1.number }
dump(cars)
dump(newCars)
car0.number = 2
dump(cars)
dump(newCars)
cars[0].number = 3
dump(cars)
dump(newCars)
```

```swift
class Car{
    var number = 0
    init(number: Int) {
        self.number = number
    }
}
var car0 = Car(number: 0)
var car1 = Car(number: 1)
var cars = [car0, car1]
var newCars = cars
cars.sort { $0.number > $1.number }
dump(cars)
dump(newCars)
car0.number = 2
dump(cars)
dump(newCars)
cars[0].number = 3
dump(cars)
dump(newCars)
```

