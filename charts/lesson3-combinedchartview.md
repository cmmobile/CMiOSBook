---
description: 多資料的圖表實作
---

# Lesson3 CombinedChartView

一般用到K線圖得地方 都會需要參考線

使用CombinedChartView實作

```swift
@IBOutlet weak var chart: CombinedChartView!
```

只要照之前步驟多生產一個LineChartData即可實作出來

```swift
private func getLData() -> LineChartData {
    var enties: [ChartDataEntry] = []
    for (i, data) in lineDatas.enumerated() {
        let e = ChartDataEntry(x: Double(i), y: data.price)
        enties.append(e)
    }
    let set1 = setLDataSet(enties: enties)
    let cData = LineChartData(dataSet: set1)
    return cData
}

private func setLDataSet(enties: [ChartDataEntry]) -> LineChartDataSet {
    let dataset = LineChartDataSet(entries: enties, label: nil)
    dataset.colors = [.yellow]
    dataset.drawCirclesEnabled = false
    dataset.drawValuesEnabled = false
    return dataset
}
```

```swift
private func setData() {
    let data = CombinedChartData()
    data.candleData = getCData()
    data.lineData = getLData()
    chart.data = data
}
```

![&#x5716;1](../.gitbook/assets/jie-tu-20200703-xia-wu-5.28.19.png)

### 圖表優化

由圖1可以看出CombinedChartView 的x軸自動設定把 第一及最後的K線吃掉了，解決方法是自訂x的上下邊界

最小值

```swift
chart.xAxis.axisMinimum = -0.5
```

最大值

```swift
chart.xAxis.axisMaximum = Double(lineDatas.count) - 0.5
```

最大值會由資料的根數去決定

待編輯...

