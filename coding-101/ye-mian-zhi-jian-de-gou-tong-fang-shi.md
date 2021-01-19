---
description: 學習如何解決多頁面傳遞資料的問題、或是包裝Model資料中心與VC溝通的問題。
---

# 物件之間的溝通方式

* Property
  * 單向持有還OK，例如：DataManager 擁有 DataModel
  * 如果兩個類別互相擁有，則比較不建議，例如：父VC和子VC互相擁有
  * 缺點：兩個類別互相耦合，不符合物件導向設計原則和MVC設計
* Static 物件或是 Singleton 模式
  * 此資料必須確定是有唯一性才可以使用，不然請少用此模式
  * 缺點：專案全部物件都能存取，安全性不足
  * 缺點：同時存取相同資料時，可能會有多執行緒的問題
* Delegate
  * 推薦使用!!\(UIKit中大量使用\)
* Closure
  * 推薦使用!!\(URLSession中使用\)
* KVO
  * 監聽某個數值的改變，視情況使用
* Notifications Center
  * 常用案例：觀察鍵盤顯示或消失事件
  * 缺點：資料流很難控制，發動時機不確定，任何地點和時間都可以呼叫，真的有需求才使用

```swift
/// 加入觀察者
NotificationCenter.default.addObserver(self, selector: #selector(keyboardWillBeShown(note:)), name: Notification.Name.UIKeyboardWillShow, object: nil)

/// 發動通知
NotificationCenter.default.post(name: Notification.Name.UIKeyboardWillShow, object: nil, userInfo: userInfo)

/// 移除觀察者
NotificationCenter.default.removeObserver(self, name: Notification.Name.UIKeyboardWillShow, object: nil)
```



