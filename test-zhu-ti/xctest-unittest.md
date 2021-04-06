# XCTest-UnitTest

## **XCTest 和 Unit Testing Bundle**

* **XCTest：是用來建立unit tests, performance tests, UI tests的Framework**
* **Unit Testing Bundle：Xcode內建用來做單元測試的Target**

## **如何建立 Unit Tests Target**
1. 當我們在建立專案時，Xcode 會貼心的提醒要不要自動建立 Unit / UI Tests，只要勾選 Include Tests 則會自動產生 Unit / UI Tests 的 Target(圖1)。
2. 但若在上述步驟忘了勾選，或是後來才決定要加入 Unit Tests，則也可以透過手動新增Unit Tests 的 Target(圖2)。
3. 手動新增的 Target 需要手動 Import 被測試的 Target(圖3)。
```@testable import Demo```

![圖1](https://user-images.githubusercontent.com/36924807/113652165-dae38680-96c5-11eb-8eca-bb0044affad9.png)

![圖2](https://user-images.githubusercontent.com/36924807/113652163-d9b25980-96c5-11eb-921a-a2b5a630d80f.png)

![圖3](https://user-images.githubusercontent.com/36924807/113652157-d61ed280-96c5-11eb-94f0-3e483cd449b4.png)

## 命名
* 必須以前綴**test**開頭、**不帶參數、不返回值**。
* 讓名稱具有**描述性**，透過命名了解測試方法將要測試的內容。
* 好的命名可以快速識別失敗的測試、幫助辨識是否已經測試過某種腳本或一段代碼。
* 當測試失敗時，我們想要知道甚麼？

1. 我們正在測試什麼？
2. 在什麼情況下？
3. 預期的結果是什麼？

```swift
test_username_is_nil()
testEmptyTableRowAndColumnCount()
testValidCallToiTunesGetsHTTPStatusCode200()
```

## Setup and Teardown
* setUp()：在測試執行前做一些初始化的設定。
* tearDown()：測試結束後，在這裡清除資料或設定，確保不會留下任何可能影響後續測試的東西。

> [官方文件](https://developer.apple.com/documentation/xctest/xctestcase/understanding_setup_and_teardown_for_test_methods)

```swift 
class SetUpAndTearDownExampleTestCase: XCTestCase {
    
    override class func setUp() { // 1.
        // This is the setUp() class method.
        // It is called before the first test method begins.
        // Set up any overall initial state here.
    }
    
    override func setUpWithError() throws { // 2.
        // This is the setUpWithError() instance method.
        // It is called before each test method begins.
        // Set up any per-test state here.
    }
    
    override func setUp() { // 3.
        // This is the setUp() instance method.
        // It is called before each test method begins.
        // Use setUpWithError() to set up any per-test state,
        // unless you have legacy tests using setUp().
    }
    
    func testMethod1() throws { // 4.
        // This is the first test method.
        // Your testing code goes here.
        addTeardownBlock { // 5.
            // Called when testMethod1() ends.
        }
    }
    
    func testMethod2() throws { // 6.
        // This is the second test method.
        // Your testing code goes here.
        addTeardownBlock { // 7.
            // Called when testMethod2() ends.
        }
        addTeardownBlock { // 8.
            // Called when testMethod2() ends.
        }
    }
    
    override func tearDown() { // 9.
        // This is the tearDown() instance method.
        // It is called after each test method completes.
        // Use tearDownWithError() for any per-test cleanup,
        // unless you have legacy tests using tearDown().
    }
    
    override func tearDownWithError() throws { // 10.
        // This is the tearDownWithError() instance method.
        // It is called after each test method completes.
        // Perform any per-test cleanup here.
    }
    
    override class func tearDown() { // 11.
        // This is the tearDown() class method.
        // It is called after all test methods complete.
        // Perform any overall cleanup here.
    }
    
}
```

![執行順序](https://user-images.githubusercontent.com/36924807/113652418-56ddce80-96c6-11eb-97e6-d0d7a8cec2e8.png)

* **什麼時候使用addTeardownBlock()？**
通常我們會在tearDown()中清除設定，但是tearDown()會在每項測試完成後執行。如果我們只是需要在某一項測試中清除某項設定或資料，就可以使用addTeardownBlock()，針對特定的測試項目進行處理。

## Throwing methods
* 測試方法也可以也可以拋出錯誤，只要被測試的方法中拋出錯誤，就可以使用合適的XCTAssert來驗證。

> [回顧Error Handling](https://www.appcoda.com.tw/swift-error-handling/)

```swift
func test_username_is_nil() throws {
    let expectedError = ValidationError.invalidValue
    var error: ValidationError?
        
    XCTAssertThrowsError(try validation.validateUsername(nil)) { thrownError in
        error = thrownError as? ValidationError
    }
        
    XCTAssertEqual(expectedError, error)
        
    XCTAssertEqual(expectedError.errorDescription, error?.errorDescription)
}
```
```swift
func test_is_valid_username() throws {
    XCTAssertNoThrow(try validation.validateUsername("nic wu"))
}
```
## XCTAssert
* 選擇正確的assert很重要。
* 不同的assert，提供不同的失敗原因和訊息，可以幫助我們更快的識別失敗的測試。

>[官方文件](https://developer.apple.com/documentation/xctest#2870839)

![XCTAssert](https://user-images.githubusercontent.com/36924807/113652695-dc617e80-96c6-11eb-8adc-645978e80286.png)

![XCTAssertEqual提供錯誤訊息](https://user-images.githubusercontent.com/36924807/113652723-ea170400-96c6-11eb-90d3-7c2542154021.png)


## 如何把程式寫成可測試

* 乾淨的架構 \( 請詳見 MVC 、 MVVM 等等架構 \)
* 職責單一的物件
* 抽離檔案系統 \( UserDefault or File \)、資料庫 \( DataBase \)、遠端資料 \( Api \)
  * [https://cocoapods.org/pods/MockUserDefaults](https://cocoapods.org/pods/MockUserDefaults)

## 常見的測試種類

#### 1. 對 Model 測試：可測試 Model 的建構式或是方法

```swift
func testNasaData() throws {

    let rawResponse = """
    {
    "description": "The past year was extraordinary for the discovery of extraterrestrial fountains and flows -- some offering new potential in the search for liquid water and the origin of life beyond planet Earth.. Increased evidence was uncovered that fountains spurt not only from Saturn's moon Enceladus, but from the dunes of Mars as well. Lakes were found on Saturn's moon Titan, and the residual of a flowing liquid was discovered on the walls of Martian craters. The diverse Solar System fluidity may involve forms of slushy water-ice, methane, or sublimating carbon dioxide. Pictured above, the light-colored path below the image center is hypothesized to have been created sometime in just the past few years by liquid water flowing across the surface of Mars.",
    "copyright": "MGS, MSSS, JPL, NASA",
    "title": "A Year of Extraterrestrial Fountains and Flows",
    "url": "https://apod.nasa.gov/apod/image/0612/flow_mgs.jpg",
    "apod_site": "https://apod.nasa.gov/apod/ap061231.html",
    "date": "2006-12-31",
    "media_type": "image",
    "hdurl": "https://apod.nasa.gov/apod/image/0612/flow_mgs_big.jpg"
    }
    """
    
    // XCTUnwrap嘗試解開可選的內容，如果可選的內容為nil，則會拋出錯誤（並因此導致測試失敗）
    let data = try XCTUnwrap(rawResponse.data(using: .utf8))
    let nasaData = try XCTUnwrap(JSONDecoder().decode(NasaData.self, from: data))
        
    XCTAssertEqual(nasaData.date, "2006-12-31")
    XCTAssertEqual(nasaData.mediaType, "image")
}
```

#### 2. 對 API 做異步測試：使用 XCTestExpectation（這算是整合測試，不是單元測試）

```swift
func testDataManagerGetNasaData() {
        
    // 宣告expectation
    let expect = expectation(description: "Get nasa data")
    let dataManager = DataManager()
    let urlString = "https://raw.githubusercontent.com/cmmobile/NasaDataSet/main/apod.json"
        
    dataManager.getNasaData(urlString: urlString) { result in
            
        switch result {
        case .success(_):
            XCTAssert(true)
        case .failure(_):
            XCTAssert(false)
        }
        // 達成期望，讓test runner知道可以繼續
        expect.fulfill()
    }
    // 等待期望被實現，或者10秒後超時
    wait(for: [expect], timeout: 10.0)
}
```

#### 3. 對 ViewModel 或是 Manager 測試：當遇到 API 或資料庫，可用 Protocol 抽離實作並依賴注入\(DI\)

建立呼叫 DataProvider 的 Protocol，並用依賴注入\(DI\)

```swift
class DataProvider: DataProviderDelegate{
    
    func getData(url: URL, completionHandler: @escaping (Data?, URLResponse?, Error?) -> Void){
        let task = URLSession.shared.dataTask(with: url) {
            (data, response, error) in completionHandler(data, response, error)
        }
        task.resume()
    }
}

protocol DataProviderDelegate: class {
    func getData(url: URL, completionHandler: @escaping (Data?, URLResponse?, Error?) -> Void)
}

class DataManagerWithDI{
    
    enum DataManagerError: Error {
        case urlError
        case noData
    }
    
    weak var dataProvider: DataProviderDelegate?
    
    init(dataProvider: DataProviderDelegate) {
        self.dataProvider = dataProvider
    }
    
    func getNasaData(completionHandler: @escaping (Result<[NasaData], Error>) -> Void) {
        
        guard let url = URL(string: "https://raw.githubusercontent.com/cmmobile/NasaDataSet/main/apod.json") else {
            completionHandler(.failure(DataManagerError.urlError))
            return
        }
        dataProvider?.getData(url: url) {
            (data, response, error) in
            if let error = error {
                completionHandler(.failure(error))
                return
            }
            if let data = data,
               let nasaDataArray = try? JSONDecoder().decode([NasaData].self, from: data) {
                completionHandler(.success(nasaDataArray))
            } else {
                completionHandler(.failure(DataManagerError.noData))
            }
        }
    }
}
```

測試的時候，建立 FakeDataProvider ，注入並且測試

```swift
func testDataManagerWithDIGetNasaData() throws {
    
    let fakeDataProvider = FakeDataProvider()
    let dataManagerWithDI = DataManagerWithDI(dataProvider: fakeDataProvider)
    dataManagerWithDI.dataProvider = fakeDataProvider
    dataManagerWithDI.getNasaData {
        result in
        switch result{
        case .success(_):
            XCTAssert(true)
        case .failure(_):
            XCTAssert(false)
        }
    }
}

class FakeDataProvider: DataProviderDelegate{
    
    func getData(url: URL, completionHandler: @escaping (Data?, URLResponse?, Error?) -> Void){
        
        let dataString = """
        [{
        "description": "The past year was extraordinary for the discovery of extraterrestrial fountains and flows -- some offering new potential in the search for liquid water and the origin of life beyond planet Earth.. Increased evidence was uncovered that fountains spurt not only from Saturn's moon Enceladus, but from the dunes of Mars as well. Lakes were found on Saturn's moon Titan, and the residual of a flowing liquid was discovered on the walls of Martian craters. The diverse Solar System fluidity may involve forms of slushy water-ice, methane, or sublimating carbon dioxide. Pictured above, the light-colored path below the image center is hypothesized to have been created sometime in just the past few years by liquid water flowing across the surface of Mars.",
        "copyright": "MGS, MSSS, JPL, NASA",
        "title": "A Year of Extraterrestrial Fountains and Flows",
        "url": "https://apod.nasa.gov/apod/image/0612/flow_mgs.jpg",
        "apod_site": "https://apod.nasa.gov/apod/ap061231.html",
        "date": "2006-12-31",
        "media_type": "image",
        "hdurl": "https://apod.nasa.gov/apod/image/0612/flow_mgs_big.jpg"
        }]
        """
            
        let data = Data(dataString.utf8)
        completionHandler(data, nil ,nil)
    }
}
```

#### 4. 性能測試：使用Measure Block

```swift
class PerformanceTests: XCTestCase {

    lazy var testData: [Int] = {
        return (0..<100000).map { Int($0) }
    }()

    func test_performance_getEvenNumbers_forEachLoop() {
        //這個區塊會運行10次，收集平均執行的時間和運行的標準偏差
        measure {
            var evenNumbers: [Int] = []
            testData.filter { number in number % 2 == 0}.forEach { number in evenNumbers.append(number) }
        }
    }


    func test_performance_getEvenNumbers_forLoop() {
        //這個區塊會運行10次，收集平均執行的時間和運行的標準偏差
        measure {
            var evenNumbers: [Int] = []
            for number in testData {
                if number % 2 == 0 {
                    evenNumbers.append(number)
                }
            }
        }
    }
}
```
* 性能測試需要設置Baseline來驗證是否通過測試，沒有設置的會提示No baseline average for Time。
* 點擊左側灰色菱形圖示，可以看到更多訊息、設置Baseline。

![性能測試設置Baseline](https://user-images.githubusercontent.com/36924807/113653072-b2f52280-96c7-11eb-950d-b135d23890a6.png)

## **測試覆蓋率 Code Coverage**

![步驟1：選擇Edit Scheme](https://user-images.githubusercontent.com/36924807/113653311-30209780-96c8-11eb-885a-509214745dd9.png)

![步驟2：Test->Options->勾選Code Coverage](https://user-images.githubusercontent.com/36924807/113653338-3ca4f000-96c8-11eb-8d6d-9f58b30ff6e8.png)

* 步驟3：Run Test

![步驟4：Report navigator->Coverage](https://user-images.githubusercontent.com/36924807/113653371-4dedfc80-96c8-11eb-9a51-c2d6327fc512.png)

![步驟5：點擊Coverage可以看到所有檔案的Code Coverage](https://user-images.githubusercontent.com/36924807/113653420-5fcf9f80-96c8-11eb-93ca-18ba4f580e9b.png)

![步驟6：點擊下拉，可以看到每一個方法的Code Coverage](https://user-images.githubusercontent.com/36924807/113653439-69f19e00-96c8-11eb-81bb-b2813e8a0ce7.png)

![步驟7：進入某一檔案，綠色表示有被測試到程式碼，紅色表示沒有被測試到](https://user-images.githubusercontent.com/36924807/113653460-737b0600-96c8-11eb-9a89-93e810ed1574.jpeg)

## 100% Coverage?
* 100％的覆蓋率並不是我們主要的目標，至少確認重要的邏輯已經被測試。
* 達到100％可能會非常耗時、精力，而效益可能不那麼大。

## 是否需要測試private method?
* [單元測試：是否針對非 public method 進行測試？](https://dotblogs.com.tw/hatelove/2012/07/19/why-you-should-not-write-unit-test-with-private-and-protected-method)
* [Testing private methods and variables in Swift](https://www.avanderlee.com/swift/testing-private-methods-variables/)

## 推薦書單

1. [IOS Unit Testing by Example: Xctest Tips and Techniques Using Swift](https://www.tenlong.com.tw/products/9781680506815)
2. [單元測試的藝術](https://www.tenlong.com.tw/products/9789864342471?list\_name=srh)
3. [Test-Driven iOS Development with Swift 4](http://englishonlineclub.com/pdf/Test-Driven%20iOS%20Development%20with%20Swift%204%20-%20Write%20Swift%20code%20that%20is%20maintainable,%20flexible,%20and%20easily%20extensible%20%5BEnglishOnlineClub.com%5D.pdf)

## Reference
1. [WWDC2019: Testing in Xcode](https://www.avanderlee.com/swift/unit-tests-best-practices/)
2. [Unit tests best practices in Xcode and Swift](https://www.avanderlee.com/swift/unit-tests-best-practices/)
3. [Getting Started With Unit Testing](https://www.youtube.com/watch?v=P-Zow2yVx4o)
4. [iOS Unit Testing and UI Testing Tutorial](https://www.raywenderlich.com/960290-ios-unit-testing-and-ui-testing-tutorial)
5. [The Essential Guide to Unit Testing in iOS](https://matteomanferdini.com/ios-unit-testing/)

