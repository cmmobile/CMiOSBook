# 閉包表達式\(Closures\)

Closure當作一個沒有名稱的Function，所以Closure一樣可以定義一段程式碼的區塊，也可以接受參數與回傳資料

```swift
///這是Function
func 方法名(參數1, 參數2) -> 回傳型別{
    程式碼區塊
}

///這是Closure
{ (參數1, 參數2) -> 回傳型別 in
    程式碼區塊
}
```

```swift
/// 宣告帶有Closure參數的方法
func getMemberBonus(completionHandler: ((Int)->Void)?){
    if let bonus = bonus{
        completionHandler?(bonus)
    }
}

/// 呼叫方法範例1 <--- 一般的寫法
getMemberBonus(completionHandler: {
    bonus in 
    printf("我有\(bonus)購物金")
})

/// 呼叫方法範例2 <--- TrailingClosure寫法
getMemberBonus{ bonus in 
    printf("我有\(bonus)購物金")
}
```

{% hint style="success" %}
如果單一Closure當結尾參數時，請使用TrailingClosure
{% endhint %}

{% hint style="danger" %}
如果多個Closure當結尾參數時，請分開並完整寫出、禁止使用TrailingClosure
{% endhint %}

```swift
//單一Closure當結尾參數 - 使用TrailingClosure
UIView.animateWithDuration(1.0) {
  self.myView.alpha = 0
}

//多個Closure當結尾參數 - 禁止使用TrailingClosure
UIView.animateWithDuration(1.0,
  animations: {
    self.myView.alpha = 0
  },
  completion: { finished in
    self.myView.removeFromSuperview()
  }
)
```



