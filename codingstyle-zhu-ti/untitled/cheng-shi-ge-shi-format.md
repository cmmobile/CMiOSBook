# 程式格式\(Format\)

{% hint style="success" %}
一個Tab是4空格\(Xcode預設\)
{% endhint %}

{% hint style="success" %}
所有冒號和逗號的左邊不留空，右邊留空
{% endhint %}

```swift
// 冒號左邊不留空，右邊留空
@IBOutlet weak var tableView: UITableView!
    
// 冒號和逗號左邊不留空，右邊留空
private func initTableView(index: Int, name: String) {
    //....
}
```

{% hint style="success" %}
三元判斷式，冒號左右都留空
{% endhint %}

```swift
// 冒號左右都留空
let dataCount = isGetDataDone ? listCount : 0
```

{% hint style="danger" %}
程式碼禁止結尾加上分號
{% endhint %}

```swift
// 禁止加上分號
let count = 0
```

{% hint style="warning" %}
請用extension區隔不同的protocol或是功能
{% endhint %}

```swift
// MARK: - UIScrollViewDelegate
extension PageControlScrollView: UIScrollViewDelegate {
    //....
}

// MARK: - UITableViewDataSource
extension PageControlScrollView: UITableViewDataSource {
    //....
}

// MARK: - 股票相關
extension PageControlScrollView{
    //股票相關的方法...
}

// MARK: - 動畫相關
extension PageControlScrollView{
    //動畫相關的方法...
}
```

