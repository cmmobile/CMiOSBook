# XCTest-UnitTest

## **XCTest 和 Unit Testing Bundle**

* **XCTest：是用來建立unit tests, performance tests, UI tests的Framework**
* **Unit Testing Bundle：Xcode內建用來做單元測試的Target**

## **如何安裝**

![&#x65B0;&#x589E;Target &amp;gt; iOS Unit Testing Bundle](../.gitbook/assets/image-1565242075194.26.37.png)

![&#x547D;&#x540D;Target&#x540D;&#x7A31;](../.gitbook/assets/image-1565242110322.27.12.png)

![&#x5B8C;&#x6210;&#x5B89;&#x88DD;](../.gitbook/assets/image-1565242215497.29.49.png)

## **如何撰寫測試**

* setUp\(\) 和 tearDown\(\) 在每個測試方法的前後都會執行，可以做一些統一初始化的動作或是清掉狀態等動作
* 在這個類別中定義的測試方法，名稱需要以「test開頭」，並且「不返回內容」，才可執行

```swift
class SampleAppTests: XCTestCase {

/// 每個測試執行前會呼叫
override func setUp() {
    // Put setup code here. This method is called before the invocation of each test method in the class.
}

/// 每個測試執行後會呼叫
override func tearDown() {
    // Put teardown code here. This method is called after the invocation of each test method in the class.
}

//是否為有效信箱 - 成功
func testToolIsEmailValidSuccess() {
    let tool = Tool()
    let result = tool.isEmailValid(email: "aaaa@aaaa.bbb")
    XCTAssertTrue(result)
}

//是否為有效密碼 - 失敗
func testToolIsEmailValidFail() {
	let tool = Tool()
    let result = tool.isPasswordValid(password: "aaaaaa")
    XCTAssertTrue(result)
}

// 計算效能
func testPerformanceExample() {
    self.measure {
        let tool = Tool()
        _ = tool.isEmailValid(email: "aaaa@aaaa.bbb")
    }
}
```

## 如何把程式寫成可測試

* 乾淨的架構 \( 請詳見 MVC 、 MVVM 等等架構 \)
* 職責單一的物件
* 抽離檔案系統、資料庫、遠端資料

對 Model 測試



對 ViewModel 或是 Manager 測試

## **如何執行**

![&#x53F3;&#x9375;&#x9EDE;&#x64CA;Run Test](../.gitbook/assets/image-1565243084406.42.17.png)

![&#x5DE6;&#x9375;&#x9EDE;&#x64CA;&#x64AD;&#x653E;&#x5C0F;&#x5716;&#x793A;](../.gitbook/assets/image-1565243102802.43.59.png)

## **如何看測試結果：**

![&#x904E;&#x7A0B;&#x4E2D;Console&#x6703;&#x986F;&#x793A;](../.gitbook/assets/image-1565243479162.50.53.png)

![&#x6E2C;&#x8A66;&#x5206;&#x9801;\(&#x5C0D;&#x6E2C;&#x8A66;&#x53F3;&#x9375;&amp;gt;Jump to Report\)](../.gitbook/assets/image-1565243522420.49.53.png)

![](../.gitbook/assets/image-1565243565622.49.44.png)

## **如何使用測試覆蓋率**

![&#x8A2D;&#x5B9A;&#x7D71;&#x8A08;&#x6E2C;&#x8A66;&#x8986;&#x84CB;&#x7387;](../.gitbook/assets/image-1565243957171.58.17.png)

![&#x67E5;&#x770B;&#x6E2C;&#x8A66;&#x8986;&#x84CB;&#x7387;](../.gitbook/assets/image-1565243676834.54.19.png)

