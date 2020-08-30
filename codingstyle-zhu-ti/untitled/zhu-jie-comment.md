# 註解\(Comment\)

{% hint style="success" %}
全部類別、屬性、方法、列舉，皆用詳細註解\(option + cmd + /\)
{% endhint %}

```swift
/// 車子
class Car {

    /// 速度
    private var speed = 10
    
    /// 出發
    ///
    /// - Parameters:
    ///   - key: 鑰匙
    ///   - destination: 目的地
    func run(key: String, destination: String){
        // ...
    }
}
```

{% hint style="info" %}
平常用於解釋部份 Code 為 //....
{% endhint %}

```swift
func run(key: String, destination: String){
    // Step1: 使用鑰匙發動車子
    start(key: key)
    // Step2: 往目的地前進
    runLoop(destination: destination)
}
```

{% hint style="info" %}
標記特定目的的寫法

* // MARK: - 說明文字 
* // TODO: - 需要做的功能 
* // FIXME: - 待修正的BUG
{% endhint %}

```swift
// MARK: - TransportationDelegate
extension Car: TransportationDelegate {

}
```

