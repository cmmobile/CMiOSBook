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
* 抽離檔案系統 \( UserDefault or File \)、資料庫 \( DataBase \)、遠端資料 \( Api \)

## 常見的測試種類

#### 對 Model 測試：可測試 Model 的建構式或是方法

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
    
    guard let data = rawResponse.data(using: .utf8) else{
        XCTAssert(false)
        return
    }
    let nasaData = try? JSONDecoder().decode(NasaData.self, from: data)
    XCTAssertEqual(nasaData?.date, "2006-12-31")
    XCTAssertEqual(nasaData?.mediaType, "image")
}
```

#### 對 API 做異步測試：這算是整合測試，不是單元測試

```swift
func testDataManagerGetNasaData() throws {
    
    let expect = expectation(description: "GetData")
    
    let dataManager = DataManager()
    dataManager.getNasaData {
        result in
        switch result{
        case .success(_):
            XCTAssert(true)
        case .failure(_):
            XCTAssert(false)
        }
        expect.fulfill()
    }
    
    wait(for: [expect], timeout: 10.0)
}
```

#### 對 ViewModel 或是 Manager 測試：當遇到 API 或資料庫，可用 Protocol 抽離實作並依賴注入\(DI\)

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

