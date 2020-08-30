# 類別與結構\(Classes and Structures\)

{% hint style="success" %}
一個屬性佔一行 ，禁止單行宣告多個屬性
{% endhint %}

{% hint style="danger" %}
類別內部呼叫成員禁止加self，除非與參數衝名或是必要狀況
{% endhint %}

```swift
/// 圓形類別
class Circle: Shape {
        
    /// 圓心的x(禁止單行宣告多個變數)
    private var x: Int
    
    /// 圓心的y
    private var y: Int
  
    /// 初始化(初始化時參數丟入相同名稱的變數要加self)
    init(x: Int, y: Int) {
        self.x = x
        self.y = y
    }
}
```

{% hint style="danger" %}
禁止在Class外宣告變數或方法，不易管理和閱讀
{% endhint %}

```swift
// Class外宣告變數
let globalData = 100

/// 圓形類別
class Circle: Shape {
    // ...
}
```



