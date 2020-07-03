---
description: 圖表資料的建構與呈現
---

# Lesson2 Chart Data

範例：[https://github.com/cmmobile/WWChartsDemo](https://github.com/cmmobile/WWChartsDemo)

要建立Chart圖上的資料，此範例以建構出K線圖為目標來介紹。

### 1.建立Entry

```text
ChartDataEntry(x: 0, y: 1)
```

一般的線圖、Bar圖在圖面上皆是一個x一個y而k線圖則有開高低收這四個數字

```text
CandleChartDataEntry(x: 0, shadowH: 10, shadowL: 0, open: 0, close: 0)
```

### 2.Entry轉換成DataSet

建立模擬資料

```swift
private func setData() {
    var enties: [CandleChartDataEntry] = []
    for (i, data) in datas.enumerated() {
        let e = CandleChartDataEntry(x: Double(i), shadowH: data.high, shadowL: data.low, open: data.open, close: data.close)
        enties.append(e)
    }
    let set1 = CandleChartDataSet(entries: enties, label: nil)
    let data = CandleChartData(dataSet: set1)
    
    chart.data = data
}
```

一般大部分在公司的Chart X軸代表的都是日期

所以會拿index值直接放入x內再使用限制線去顯示日期

![](../.gitbook/assets/jie-tu-20200702-xia-wu-6.00.59.png)

### 3.DataSet轉換成ChartData

...

