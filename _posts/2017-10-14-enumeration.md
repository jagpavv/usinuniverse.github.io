---
title: 열거형
layout: post
---

### 열거형

* 열거형이란? 그룹에 대한 연관된 값을 정의하고 사용가능한 타입이다.
* 원치 않는 값의 입력을 방지할 때, 입력받을 값을 특정할 수 있을 때, 제한된 값 중에서만 선택하도록 강요할 때 사용할 수 있다.
* 원시값(rawValue)이라는 형태로 실제 값(정수, 실수, 문자열 등)을 부여할 수 있다.

```swift
enum Color {
    case red
    case blue
    case yellow
}

or

enum Color {
    case red, blue, yellow
}
```

### 열거형의 원시값

```swift
enum CompassPoint: Int {
    case north = 1, south, east, west
}

CompassPoint.south.rawValue //2
CompassPoint.east.rawValue //3
CompassPoint.west.rawValue //4
```

* 원시값은 원하는 값을 지정할 수 있으며, 시작값을 지정할 수 있다. (기본값은 0)

### 열거형과 switch 구문

* 열거형으로 선언된 변수를 switch 구문과 활용할 경우 모든 값에 대한 비교로 인해 default 구문을 생략할 수 있다.

```swift
enum CompassPoint {
    case north, south, east, west
}

var directionToHead = CompassPoint.west

switch directionToHead {
case .north:
    print("Lots of planets have a north")
case .south:
    print("Watch out for penguins")
case .east:
    print("Where the sun rises")
case .west:
    print("Where the skies are blue") //실행
}
```

* 열거형의 모든 case가 제공될때 default 값은 제공될 필요가 없다. 하지만 만약 모든 case에 대한 비교가 이루어지지 않았을 때에는 반드시 default 구문이 존재해야 한다.
