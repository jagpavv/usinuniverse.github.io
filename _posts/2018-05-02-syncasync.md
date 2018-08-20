---
title: 동기와 비동기 차이점
layout: post
---

### 동기와 비동기

* 면접을 보러다닐 때 동기와 비동기에 대한 질문을 받았던 적이 있다.
* 동기(sync)는 말 그대로 동시에 일어나는 것이며, 비동기(async)는 동시에 일어나지 않는 것이다.
* 더 쉽게 말하자면 동기(sync)는 A 코드가 모두 완료되기 전까지 B 코드는 실행되지 않는다.
* 비동기(asnyc)의 경우 A 코드의 완료 시점과 상관없이 B 코드가 실행된다는 차이점이 있다.
* 즉, 동기(sync)를 집안일로 비유하자면 청소를 다 하고 나서 빨래를 하고 빨래를 다 한 후 잠을 자는 것이다.
* 반면 비동기(async)는 로봇청소기한테 청소를 맡기고(완료시점 알 수 없음) 세탁기에 빨래를 맡기고(완료시점 알 수 없음) 잠을 자는 것이다. 잠을 가장 먼저 깰 수도 있고 세탁이 가장 먼저 끝날 수도 있다.

### iOS의 Queue 종류

* Serial: 직렬 처리로 한 번에 하나의 작업만 한다. 
* Concurrent: 병렬 처리로 한 번에 여러 작업을 한다.
* 두 가지 모두 FIFO(First In First out) 방식으로 처리된다.
* iOS에서는 앱을 실행하면 기본적으로 두 가지 큐를 만들어주는데, Main Queue는 Serial Queue이며 Global Queue는 Concurrent Queue다.
* Global Queue의 경우 qos로 우선순위를 정해 일의 시작 순서를 정할 수 있다. (userInteractive > userInitiated > default > utility > background > unspecified)

### 동기(sync)와 비동기(asnyc) 그리고 직렬과 병렬

* 동기(sync)와 비동기(async)는 처리가 끝날때까지 기다리느냐(동기) 지시만 하고 다른 일을 하느냐의 차이이다(비동기).
* 직렬과 병렬은 한 번에 하나의 작업만 하느냐(직렬) 여러 일을 동시에 하느냐(병렬)의 차이다.

### 동기(sync) 예시

```swift
// 조건
func a() {
    for _ in 0...10 {
        print("a 함수 입니다.")
    }
}

func b() {
    print("b 함수 입니다.")
}

// 실행
DispatchQueue.global().sync {
    self.a()
}
self.b()

// 결과
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
b 함수 입니다.
```

* 동기 방식이기 때문에 a 함수가 끝날 때까지 기다렸다가 다음 블록인 self.b()가 실행된다.

### 비동기(async) 예시

```swift
// 실행
DispatchQueue.global().async {
    self.a()
}
self.b()

// 결과
b 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
```

* 비동기 방식이기 때문에 a 함수를 실행시켜놓고 바로 다음 블록인 self.b()를 실행한다.

### 비동기(async) 방식에 qos 지정

```swift
// 가장 높은 우선 순위
DispatchQueue.global(qos: .userInteractive).async {
    self.a()
}

// 가장 낮은 우선 순위
DispatchQueue.global(qos: .unspecified).async {
    self.b()
}

// 결과
a 함수 입니다.
b 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.

/////

// 가장 낮은 우선 순위
DispatchQueue.global(qos: .unspecified).async {
    self.a()
}

// 가장 높은 우선 순위
DispatchQueue.global(qos: .userInteractive).async {
    self.b()
}

// 결과
b 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
```

### async 응용

* 비동기 방식으로 실행하되, 10초 뒤에 해당함수를 실행하는 방법

```swift
// 지금(.now()) 더하기 10초. 즉 지금부터 10초 후 self.a 실행
DispatchQueue.global().asyncAfter(deadline: .now() + 10) {
    self.a()
}
self.b()

// 결과
b 함수 입니다.
...10초 후...
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
a 함수 입니다.
```
