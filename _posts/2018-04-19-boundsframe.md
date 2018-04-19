---
title: iOS에서 bounds와 frame의 차이
layout: post
---

### iOS에서 bounds와 frame의 차이

* UIView 등 눈에 보이는 개체들을 만들다보면 bounds와 frame이 나온다.
* 어떤 경우에는 bounds을 선택하던 frame을 선택하던 결과가 같다. 하지만 반대로 동일한 값임에도 전혀 다른 결과가 나오기도 한다.
* 과연 둘의 차이는 무엇일까? 간략히 요약하면 다음과 같다.
* bounds는 뷰 자체의 좌표계를 이용한 좌표와 크기이다.
* frame은 슈퍼 뷰의 좌표계를 이용한 좌표와 크기이다.

### 자세히 알아보기

* bounds와 frame의 차이를 자세히 알아보려면 아래의 그림을 참고하면 쉽다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-04-19/01.png?raw=true">

* 먼저 위 그림의 상황에서 View B가 기울어지지 않았다고 생각해보자.
* 이런 경우에 View B의 frame과 bounds를 이용한 값을 비교해보면 다음과 같다.

```swift
let bounds = (viewB.bounds.size.width / 2) // 100
let frame = (viewB.frame.size.width / 2) // 100
```

* 이처럼 특정 경우에는 같은 값이기 때문에 bounds와 frame의 차이를 알기 어렵다.
* 이번엔 그림처럼 view B가 기울어졌다고 가정해보자.

```swift
let bounds = (viewB.bounds.size.width / 2) // 100
let frame = (viewB.frame.size.width / 2) // 160
```

* 이와 같은 차이는 아까 말한 좌표계의 기준의 차이에서 비롯된다.
* bounds의 경우는 늘 자기 자신의 좌표계를 기준으로 삼기 때문에 자기 자신이 차지하는 width는 언제나 200으로 고정이다.
* 하지만 frame의 경우는 늘 슈퍼 뷰의 좌표계를 기준으로 삼기 때문에 그림같이 기울기가 변경된다면 슈퍼 뷰의 기준에서 차지하는 width가 변경되기 때문에 고정되지 않는다.
* 기울기가 있는 경우는 흔한 경우는 아니긴 하지만, 슈퍼 뷰의 좌표계를 기준으로 측정해야 하는지 혹은 자기 자신의 좌표계를 기준으로 측정해야 하는지 상황에 따라 잘 골라야 한다.
