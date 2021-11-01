---
description: 理解程式是如何管控記憶體並且正確地操作物件，可以避免記憶體的浪費。
---

# ARC + Retain Cycle

## 課程文件

* 參考連結：
  * [Automatic Reference Counting](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html)
  * [Swift Retain Cycle](https://ithelp.ithome.com.tw/articles/10196788)
* 範例連結：
  * [iOS\_RetainCycleProject](https://github.com/cmmobile/iOS\_RetainCycleProject)

## 如何找Retain Cycle

### 方法一：使用 deinit，確認物件是否被消滅

```swift
/// 人物
class People {
    let name: String = "John"
    var macbook: Macbook? = nil
    deinit {
        print("\(name) deinit.")
    }
}

/// 筆電
class Macbook {
    let name: String = "Macbook"
    var owner: People? = nil
    deinit {
        print("\(name) deinit.")
    }
}
```

### 方法二：使用 Xcode 的 Debug Memory Graphic

![Debug Memory Graphic](../.gitbook/assets/jie-tu-20200701-xia-wu-5.05.42.png)

## 如何解決Retain Cycle

### 情境一： 兩個類別互相擁有，加上weak即可!!!

```swift
/// 人物
class People {
    let name: String = "John"
    var macbook: Macbook? = nil
    deinit {
        print("\(name) deinit.")
    }
}

/// 筆電
class Macbook {
    let name: String = "Macbook"
    weak var owner: People? = nil // <----- 必須加上weak才可以被釋放
    deinit {
        print("\(name) deinit.")
    }
}
```

### 情境二：Delegate和Protocol，加上weak即可!!!

```swift
protocol DataManagerDelegate: AnyObject {
    func didGetData()
}
class DataManager{
    weak var delegate: DataManagerDelegate? // <----- 必須加上weak才可以被釋放
    func getData(){
        delegate?.didGetData()
    }
}

class ViewContorller: UIViewController{
      private var dataManager = DataManager()
      override func viewDidLoad() {
        super.viewDidLoad()
        dataManager.delegate = self
        dataManager.getData()
    }
}
extension ViewContorller: DataManagerDelegate{
    func didGetData(){ ... }
}
```

### 情境三：被擁有的Closure，定義時請使用weak self即可!!!

```swift
class MemberManager{
    var didChangeMember: ((String) -> Void)?
    func changeMember(){
        didChangeMember?("更換會員")
    }
}

class TestClosure2ViewController: UIViewController {

    var label = UILabel()
    let memberManager = MemberManager()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        memberManager.didChangeMember = { 
            [weak self] value in // <----- Closure被Hold著，必須加上weak才可以被釋放
            guard let self = self else { return }
            self.label.text = value
        }
        
        memberManager.changeMember()
    }

    deinit {
        print("TestClosure2 deinit.")
    }
    
}
```
