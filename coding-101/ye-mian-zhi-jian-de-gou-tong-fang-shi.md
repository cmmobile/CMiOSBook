---
description: 學習如何解決多頁面傳遞資料的問題、或是包裝Model資料中心與VC溝通的問題。
---

# 物件之間的溝通方式

#### 當開始拆分架構時會有很多情境：

* 拆分成 ViewController 和 View
* 拆分大 View 和 小View
* 拆分成 ViewController 和 Model
* 拆分成 ViewController 和 ViewModel / DataManager

#### 兩個類別之間傳遞資料，解法有以下方式：

* Property
  * 單向持有還OK，例如：ViewController 擁有 View
  * 如果兩個類別互相擁有，則比較不建議，例如：父VC和子VC互相擁有
  * 缺點：兩個類別互相耦合，不符合物件導向設計原則和MVC設計
  * 缺點：兩物件互相擁有，沒有使用 weak，會發生 RetainCycle
* Static 物件或是 Singleton 模式
  * 此資料必須確定是有唯一性才可以使用，不然請少用此模式
  * 缺點：專案全部物件都能存取，安全性不足
  * 缺點：同時存取相同資料時，可能會有多執行緒的問題
  * 可能適用的時機
    * 會員資料的運用

```text
class UserManager {

    private init() {}

    static let shared = UserManager()

    /// 會員名稱
    var memberName: String = ""

    /// 會員id
    var memberId: Int = 0

}
```

* Delegate
  * 推薦使用!!\(UIKit中大量使用\)
  * 可能適用的時機
    * 到設定頁面，選取設定後，改變前一個頁面樣式為所選設定 

```text
 class ViewController: UIViewController {

    let label = UILabel()

    override func viewDidLoad() {
        super.viewDidLoad()

        label.frame = CGRect(x: 50, y: 50, width: 50, height: 50)
        view.addSubview(label)

        label.text = "fine"
    }

    ......

     func presentOptionVC() {
        let vc = OptionViewController()
        vc.delegate = self
        present(vc, animated: true, completion: nil)
    }

    ......

 }

 extension ViewController: OptionViewControllerDelegate {

    func changeState(_ string: String) {
        label.text = string
    }

}
```

```text
protocol OptionViewControllerDelegate: class {
    func changeState(_ string: String)
}

class OptionViewController: UIViewController {

    weak var delegate: OptionViewControllerDelegate?

    lazy var changeButton: UIButton = {
        let button = UIButton()
        button.addTarget(self, action: #selector(changeState), for: .touchUpInside)
        return button
    }()

    override func viewDidLoad() {
        super.viewDidLoad()

        button.frame = CGRect(x: 50, y: 50, width: 50, height: 50)
        view.addSubview(button)

    }

    @objc func changeState() {
        let stateString = "awesome"
        delegate?.changeState(stateString)
    }

}
```

* Closure
  * 推薦使用!!\(URLSession中使用\)
  * 可能適用的時機
    * 在 vc 利用 model 打 api ，api 回來後，傳回 vc 更改畫面更改為api所獲得資料， 

```text
class ViewController: UIViewController {

    let model = VCModel()
    let label = UILabel()

     override func viewDidLoad() {
        super.viewDidLoad()

        label.frame = CGRect(x: 50, y: 50, width: 50, height: 50)
        view.addSubview(label)

        model.callApi { [weak self] (string) in
            guard let self = self else { return }
            self.label.text = string
        }

    }

}
```

```text
class VCModel {

    func callApi(completion: ((String)->())) {

        let urlString = "某個api url 的 String"
        guard let url = URL(string: urlString) else { return }

        var request = URLRequest(url: url)
        request.httpMethod = "GET"

        let task = URLSession.shared.dataTask(with: request) { [weak self] (data, response, error) in
            guard let self = self else { return }

            if let error = error {
                print(error.localizedDescription)
                return
            }

            guard let data = data else { return }

            do {
                let jsonData = try JSONDecoder().decode(String.self, from: data)
                DispatchQueue.main.async {
                    completion(jsonData)
                }
            }catch {
                print(error.localizedDescription)
            }
        }
        task.resume()
    }
}
```

* KVO\([官方說明文件](https://developer.apple.com/documentation/swift/cocoa_design_patterns/using_key-value_observing_in_swift)\)
  * 監聽某個數值的改變，視情況使用

```text
class OffsetToObserve: NSObject {
    @objc dynamic var offset: Int = .zero
    func update(_ offset: Int) {
        self.offset = offset
    }
}
```

```text
class KVOTest {

    private var observation: NSKeyValueObservation?

    lazy var offsetToObserve: OffsetToObserve = {
        return OffsetToObserve()
    }()

    func setObserveObject(on offsetValue: OffsetToObserve) {

        observation = offsetValue.observe(\.offset, options: [.new]) { (object, change) in

            print(change.newValue)

        }

    }

    ......
    func updateValue(value: Int) {
        offsetToObserve.update(value)
    }
    ......   

}
```

* Notifications Center
  * 缺點：資料流很難控制，發動時機不確定，任何地點和時間都可以呼叫，真的有需求才使用
  * 可能適用的情境
    * 常用案例：觀察鍵盤顯示或消失事件
    * 兩個 vc 之間傳遞資料

```swift
/// 加入觀察者
NotificationCenter.default.addObserver(self, selector: #selector(keyboardWillBeShown(note:)), name: Notification.Name.UIKeyboardWillShow, object: nil)

/// 發動通知
NotificationCenter.default.post(name: Notification.Name.UIKeyboardWillShow, object: nil, userInfo: userInfo)

/// 移除觀察者
NotificationCenter.default.removeObserver(self, name: Notification.Name.UIKeyboardWillShow, object: nil)
```

