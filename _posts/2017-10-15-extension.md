---
title: 익스텐션
layout: post
---

### 익스텐션

* 익스텐션이란? 이미 존재하는 클래스, 구조체, 열거형 등의 객체를 직접 수정하지 않고 새로운 기능을 추가할 수 있게 한다.
* 익스텐션으로 프로퍼티를 추가할 때에는 연산 프로퍼티로만 가능하다.
* 익스텐션으로 메소드를 추가할 때 기존에 정의되어있던 메소드를 재정의 하는 것은 불가능하다.
* 익스텐션을 남발하게 되면 객체의 의미가 파편화되기 때문에 꼭 필요한 위치에서만 사용하며 분산하여 작성하지 않는 것이 좋다.

```swift
class A {
}

extension A { //클래스 A를 확장
    var name: String {
        return "이름"
    }
}

extension A { //클래스 A를 확장
    func sleep() {
        print("zZZ")
    }
}

var a: A = A()
a.name //이름
a.sleep() //zZZ
```

* 이처럼 익스텐션을 이용하면 기존의 객체의 기능을 확장할 수 있다.
* 구조체나 열거형에서 메소드가 자기 자신의 인스턴스를 수정하거나 프로퍼티를 변경하려 할 때에는 mutating 키워드를 써야한다.

```swift
extension Int {
    mutating func square() {
        self = self * self
    }
}

var squareTest: Int = 4
squareTest.square() //16
```

* 이처럼 익스텐션을 통해 직접 수정이 불가능한 객체에도 기능을 추가할 수 있다.
