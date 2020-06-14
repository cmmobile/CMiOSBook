# Commit Message

## 什麼是Commit Message?

Commit Message為推送紀錄的訊息，請務必確實填寫每次程式的更動項目。

## Commit Message的風格

目前 Android Team 暫定是 Angular 風格、iOS Team 尚未訂定。

## Angular 風格 \([Link](https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#-commit-message-guidelines)\)

```text
type(scope): subject
```

* type:
  * build\(改變建置系統或依賴\)
  * ci\(改變CI的設定\)
  * docs\(文件修改\)
  * feat\(新功能\)
  * fix\(修復Bug\)
  * perf\(優化性能\)
  * refactor\(重構\)
  * style\(CodingStyle修正\)
  * test\(新增測試\)
* scope: \(可以為空\)
  * 影響的範圍
* subject
  * 提交敘述

```text
feat: 新建立專案
feat: 新增登入頁UI
feat: 新增登入按鈕功能
fix: 修復登入邏輯參數錯誤

feat : 新增登入頁UI  <----錯誤
feat:新增登入頁UI  <----錯誤
```

