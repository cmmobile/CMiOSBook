# 命名\(Naming\)

{% hint style="success" %}
命名一律駝峰式，不論存取權限、不論常數或變數

* 開頭必大寫：Class\(類別\)、Structure\(結構\)、Enum\(列舉\)、Protocol\(協議\)
* 開頭必小寫：Function\(方法\)、Field\(變數\)、Property\(屬性\)、列舉內部的Case
{% endhint %}

{% hint style="danger" %}
禁止全大寫和底線式
{% endhint %}

```swift
class WidgetContainer { //大寫開頭駝峰式
    var widgetButton: UIButton //小寫開頭駝峰式
    let widgetHeightPercentage = 0.85 //小寫開頭駝峰式
}

enum Shape {
    case rectangle //列舉內部的case小寫開頭駝峰式
    case square
}
```

{% hint style="info" %}
備註：如果後台傳回來的json轉換出的物件，欄位是大寫，可使用CodingKey的方式解析。
{% endhint %}

```swift
public class GetLoginGuidResponse: CMoneyResponseBase, Codable {
    public var guid: String = ""
    public var authToken: String  = ""
    public var memberPk: Int = 0

    /// CodingKeys
    public enum CodingKeys: String, CodingKey {
        case guid = "Guid"
        case authToken = "AuthToken"
        case memberPk = "MemberPk"
    }
}
```

{% hint style="success" %}
建議使用有意義的命名，該命名盡可能符合該變數所具有的功能

a, b, temp1, temp2, nameA, nameB 這一類的命名方式都是不好的
{% endhint %}

