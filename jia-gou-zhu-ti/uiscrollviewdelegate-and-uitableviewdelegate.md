---
description: 針對 UIScrollView 和 UITableView 主要的各種 delegate 發動時機，做說明
---

# UIScrollViewDelegate

UIScrollViewDelegate protocol聲明的方法允許採用的委託 UIScrollView，從而響應並在一定程度上影響諸如滾動，縮放，滾動內容的減速和滾動動畫之類的操作，任何繼承 UIScrollView 都可使用。

## Responding to Scrolling and Dragging

### `func scrollViewDidScroll(UIScrollView)`

Tells the delegate when the user scrolls the content view within the receiver.

當有發生任何滑動時都會觸發

### `func scrollViewWillBeginDragging(UIScrollView)`

Tells the delegate when the scroll view is about to start scrolling the content.

當手指碰觸到螢幕，並且開始滑動前時觸發

### `func scrollViewWillEndDragging(UIScrollView, withVelocity: CGPoint, targetContentOffset: UnsafeMutablePointer<CGPoint>)`

Tells the delegate when the user finishes scrolling the content.

當手指離開螢幕前觸發

### `func scrollViewDidEndDragging(UIScrollView, willDecelerate: Bool)`

Tells the delegate when dragging ended in the scroll view.

當手指離開螢幕後觸發

### `func scrollViewShouldScrollToTop(UIScrollView) -> Bool`

Asks the delegate if the scroll view should scroll to the top of the content.

當點擊 status bar 的時間滑動前觸發

回傳 true 開啟點擊 status bar 的時間可以 scroll 到頂部

### `func scrollViewDidScrollToTop(UIScrollView)`

Tells the delegate that the scroll view scrolled to the top of the content.

當點擊完 status bar 的時間並且滑動到頂部後觸發

### `func scrollViewWillBeginDecelerating(UIScrollView)`

Tells the delegate that the scroll view is starting to decelerate the scrolling movement.

當快速滑動準備停止時觸發

### `func scrollViewDidEndDecelerating(UIScrollView)`

Tells the delegate that the scroll view has ended decelerating the scrolling movement.

當快速滑動停下來時觸發


UIScrollViewDelegate protocol聲明的方法允許採用的委託 UIScrollView，從而響應並在一定程度上影響諸如滾動，縮放，滾動內容的減速和滾動動畫之類的操作，任何繼承 UIScrollView 都可使用。

## Responding to Scrolling and Dragging

### `func scrollViewDidScroll(UIScrollView)`

Tells the delegate when the user scrolls the content view within the receiver.

當有發生任何滑動時都會觸發

### `func scrollViewWillBeginDragging(UIScrollView)`

Tells the delegate when the scroll view is about to start scrolling the content.

當手指碰觸到螢幕，並且開始滑動前時觸發

### `func scrollViewWillEndDragging(UIScrollView, withVelocity: CGPoint, targetContentOffset: UnsafeMutablePointer<CGPoint>)`

Tells the delegate when the user finishes scrolling the content.

當手指離開螢幕前觸發

### `func scrollViewDidEndDragging(UIScrollView, willDecelerate: Bool)`

Tells the delegate when dragging ended in the scroll view.

當手指離開螢幕後觸發

### `func scrollViewShouldScrollToTop(UIScrollView) -> Bool`

Asks the delegate if the scroll view should scroll to the top of the content.

當點擊 status bar 的時間滑動前觸發

回傳 true 開啟點擊 status bar 的時間可以 scroll 到頂部

### `func scrollViewDidScrollToTop(UIScrollView)`

Tells the delegate that the scroll view scrolled to the top of the content.

當點擊完 status bar 的時間並且滑動到頂部後觸發

### `func scrollViewWillBeginDecelerating(UIScrollView)`

Tells the delegate that the scroll view is starting to decelerate the scrolling movement.

當開始加速滑動時觸發

### `func scrollViewDidEndDecelerating(UIScrollView)`

Tells the delegate that the scroll view has ended decelerating the scrolling movement.

當快速滑動停下來時觸發

