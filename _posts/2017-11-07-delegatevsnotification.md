---
title: Delegate VS Notification
layout: post
---

### Delegate와 Notification의 공통점

* 둘 모두 값 또는 메시지를 전송하는 것에 사용된다.
* 그러나 세부적인 내용에선 차이가 있다.

### Delegate와 Notification의 차이점

* Delegate는 1 대 1 소통 방식이며 애플이 권장하는 방식이기도 하다.
* Delegate를 채택한 클래스, 즉 Delegate를 통해 특정 속성에 대해 위임을 받은 대리인이 되는 클래스는 필요에 따라 프로토콜을 구현해야 하는 경우도 있다.

* Notification은 1 대 다 소통 방식이다.
* Notification은 앱 전체에 걸쳐 값 또는 메시지를 전송할 수 있다.
* Notification은 전송받을 클래스에 대해 알 필요가 없기 때문에 앱의 구성요소를 분리하는 데 매우 유용하다. (이 뜻은 정확한 이해가 안됨. 추후 수정 예정)
