---
description: 學習如何解決多頁面傳遞資料的問題、或是包裝Model資料中心與VC溝通的問題。
---

# 物件之間的溝通方式

1. Property
   * 單向持有還OK，但如果兩個類別互相擁有比較不建議\(ex.父VC和子VC互相擁有\)
   * 缺點：兩個類別互相耦合，不符合物件導向設計和MVC設計
2. Delegate
   * 推薦使用!!\(UIKit中大量使用\)
3. Closure
   * 推薦使用!!\(URLSession中使用\)
4. KVO
   * 監聽某個數值的改變，視情況使用
5. Notifications Center
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

參考文章：[https://objccn.io/issue-7-4/](https://objccn.io/issue-7-4/)

