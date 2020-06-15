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

建立模擬資料

```swift
var enties: [CandleChartDataEntry] = []
for i in 1...5 {
    let number = Double(i) * 10
    let e = CandleChartDataEntry(x: Double(i), shadowH: number + 2, shadowL: number - 2, open: number, close: number + 2)
    enties.append(e)
}
```

一般大部分在公司的Chart X軸代表的都是日期

