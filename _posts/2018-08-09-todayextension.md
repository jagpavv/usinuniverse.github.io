---
title: Today Extension 알아보기
layout: post
---

# Today Extension 알아보기

### Today Extension

* Today Extension, 일명 위젯(Widget)이라 불리는 Today Extension은 메인앱에 종속되어 있는 앱이다.
* Today Extension은 메인앱에 종속되어 있기 때문에 일반적인 App Life Cycle을 따르지 않는다.
* Today Extension의 App Life Cycle은 ```TodayViewController```와 매핑되며 TodayViewController의 ```viewDidLoad```는 메인앱의 ```application(_:didFinishLaunchingWithOptions:)```와 매핑된다. 
* 그렇기 때문에 Today Extension만 단독으로 존재할 순 없다.
* iOS 10 이상부터는 compact mode, expanded mode가 존재하는데 compact가 기본이며 이때 높이는 110pts다. (이보다 작아질 수 없다.)

### Today Extension 추가하기

* File -> New -> Target -> Today Extension 선택

### compact mode, expanded mode 다루기

* ```viewDidLoad```에 ```extensionContext?.widgetLargestAvailableDisplayMode = .expanded```를 추가하면 더보기 버튼이 활성화된다.
* ```widgetActiveDisplayModeDidChange```은 옵셔널 메서드로 더보기 혹은 간략히보기를 누르면 호출된다.

### Today Extension 최신 상태 유지

* ```widgetPerformUpdate```를 이용해 위젯 스크린이 꺼져있는 동안에도 뷰를 업데이트 하도록 지원한다.

```swift
func widgetPerformUpdate(completionHandler: (@escaping (NCUpdateResult) -> Void)) {
    // Perform any setup necessary in order to update the view.
    
    // If an error is encountered, use NCUpdateResult.Failed
    // If there's no update required, use NCUpdateResult.NoData
    // If there's an update, use NCUpdateResult.NewData
    
    //completionHandler(NCUpdateResult.newData) // 업데이트 성공시 호출하는 방법
    //completionHandler(NCUpdateResult.NoData) // 아무런 업데이트가 없을 시
    //completionHandler(NCUpdateResult.Failed) // 업데이트 실패 했을 시
    
    // 이런식으로 사용할 수 있다.
    if self.isSuccessUpdate() == true {
        // do something
        completionHandler(.newData)
    } else {
        // do something
        completionHandler(.failed)
    }
}
```

