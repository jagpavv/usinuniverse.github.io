---
title: 연산 프로퍼티
layout: post
---

### 연산 프로퍼티 이해하기

* 개인적으로 연산 프로퍼티를 쉽게 이해하기 위해 작성한다.
* 연산 프로퍼티란 특정 연산을 통해 값을 제공하는 프로퍼티다.
* get은 값을 반환하며 set은 값을 할당한다.
* 그러나 연산 프로퍼티 자체의 값을 변경하기 위해서가 아니라 클래스나 구조체 등 해당 인스턴스의 다른 프로퍼티와 의존적인 관계인 경우에 사용한다.
* 예시를 통해 이해해보자.

```swift
class Length {
    var length = 0.0
    
    var half: Double {
        get {
            return length / 2.0
        }
        set {
            length = newValue * 2.0
        }
    }
}
```

* set이 없다면 half 변수는 length에 의해서 계산만 되어질뿐 새로운 값을 할당할 수 없으며 half만으로 length 의 값을 구할 수 없다.
* 그러나 실제로는 length와 half(length)는 연관된 값이고, length를 통해 half를 구할 수 있듯, half를 통해서도 length를 구할 수 도 있어야 한다.
* 그렇기 때문에 set을 구현하고, set의 newValue를 통해 연관된 값인 length를 업데이트 할 수 있는 연산을 추가하는 것이다.
* 그래서 연산(관) 프로퍼티로 외우면 이해하기 쉽다.
