---
title: iOS touch event 종류
layout: post
---

### Control Event로 정의되어 있는 Touch event 종류

* touchDown
> 컨트롤을 터치했을 때 발생하는 이벤트

* touchDownRepeat
> 컨트롤을 연속 터치 할 때 발생하는 이벤트

* touchDragInside
> 컨트롤 범위 내에서 터치한 영역을 드래그 할 때 발생하는 이벤트

* touchDragOutside
> 터치 영역이 컨트롤의 바깥쪽에서 드래그 할 때 발생하는 이벤트

* touchDragEnter
> 터치 영역이 컨트롤의 일정 영역 바깥쪽으로 나갔다가 다시 들어왔을 때 발생하는 이벤트

* touchDragExit
> 터치 영역이 컨트롤의 일정 영역 바깥쪽으로 나갔을 때 발생하는 이벤트

* touchUpInside
> 컨트롤 영역 안쪽에서 터치 후 뗐을때 발생하는 이벤트

* touchUpOutside
> 컨트롤 영역 안쪽에서 터치 후 컨트롤 밖에서 뗐을때 이벤트

* touchCancel
> 터치를 취소하는 이벤트 (touchUp 이벤트가 발생되지 않음)
 
* valueChanged
> 터치를 드래그 및 다른 방법으로 조작하여 값이 변경되었을때 발생하는 이벤트

* primaryActionTriggered
> 버튼이 눌릴때 발생하는 이벤트 (iOS보다는 tvOS에서 사용)

* editingDidBegin
> UITextField에서 편집이 시작될 때 호출되는 이벤트

* editingChanged
> UITextField에서 값이 바뀔 때마다 호출되는 이벤트

* editingDidEnd
> UITextField에서 외부객체와의 상호작용으로 인해 편집이 종료되었을 때 발생하는 이벤트

* editingDidEndOnExit
> UITextField의 편집상태에서 키보드의 return 키를 터치했을 때 발생하는 이벤트

* allTouchEvents
> 모든 터치 이벤트

* allEditingEvents
> UITextField에서 편집작업의 이벤트

* applicationReserved
> 각각의 애플리케이션에서 프로그래머가 임의로 지정할 수 있는 이벤트 값의 범위

* systemReserved
> 프레임워크 내에서 사용하는 예약된 이벤트 값의 범위

* allEvents
> 시스템 이벤트를 포함한 모든 이벤트
