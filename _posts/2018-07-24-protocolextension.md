---
title: 프로토콜 초기구현
layout: post
---

# 프로토콜 초기구현

### 기본적인 프로토콜 사용법

* delegate 패턴 구현에 자주 쓰이는 프로토콜의 기본적인 사용법은 다음과 같다. [(delegate 패턴이란?)](https://github.com/usinuniverse/usinuniverse.github.io/blob/master/_posts/2017-11-07-delegatevsnotification.md)
* 실제 구현이 프로토콜을 준수하는 곳에서 이루어진다.

```swift
// 정의
protocol Sendable {
    func send(to: String) -> Void
}

class Sender: Sendable {
    // 구현
    func send(to: String) {
        print("\(to)에게 메시지를 전달합니다.")
    }
    
}
```

### 프로토콜 초기구현

* 보통의 경우 프로토콜을 채택하는 곳에서 구현이 되는데, 한 두개 정도가 아닌 여러 곳에서 프로토콜을 준수하게 되면 상당히 많은 같은 코드가 반복된다.
* 따라서 초기구현을 통해 이것을 방지할 수 있다.

```swift
// 정의
protocol Sendable {
    func send(to: String) -> Void
}
// 구현
extension Sendable {
    func send(to: String) {
        print("\(to)에게 메시지를 전달합니다.")
    }
}

class Sender: Sendable {
    // 구현하지 않아도 이미 구현되어 있음
}

let sender = Sender()
sender.send(to: "옆사람") // 옆사람에게 메시지를 전달합니다.
```
