---
description: 我們需要一套程式碼的穿搭規則，讓團隊裡的程式碼更具可讀性、易維護、一致性。
---

# CodingStyle 規則

## 核心目標

使程式碼能夠以下三個特性

* 可讀性
* 易維護
* 一致性

## 規則參考

* [The Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)  
* [LanguageGuide](https://docs.swift.org/swift-book/LanguageGuide/TheBasics.html)
* [raywenderlich](https://github.com/raywenderlich/swift-style-guide) \(主要\)
* [linkedin](https://github.com/linkedin/swift-style-guide)

## 專案\(Project\)

專案強制寫Readme ，三大部分

* 建置相關\(環境、方法等等\)
* 架構相關\(MVC設計、關鍵腳本、資料中心等等\)
* 三方依賴\(需包含版本\)

## 正確性\(Correctness\)

* 禁止有紅色Error，上Git請確保可以編譯成功
* 盡量避免有黃色Warning \(ex.語法過時,不安全的用法之類的\)\(三方例外\)

## 命名\(Naming\)

* 命名一律駝峰式，不論存取權限、不論常數或變數\(因為官方推薦\)
* 開頭必大寫：Class\(類別\)、Structure\(結構\)、Enum\(列舉\)、Protocol\(協議\)
* 開頭必小寫：Function\(方法\)、Field\(變數\)、Property\(屬性\)、列舉內部的Case
* 禁止全大寫和底線式

```swift
    class WidgetContainer { //大寫開頭駝峰式
        private var widgetButton: UIButton //小寫開頭駝峰式
        let widgetHeightPercentage = 0.85 //小寫開頭駝峰式
    }

    enum Shape {
        case rectangle //列舉內部的case小寫開頭駝峰式
        case square
    }
```

備註：如果後台傳回來的json轉換出的物件，欄位是大寫，可使用CodingKey的方式

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

{% hint style="danger" %}
目前還在搬移中
{% endhint %}

