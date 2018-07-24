---
title: 스위프트 고차함수 사용법
layout: post
---

# 고차함수란?

* 매개변수로 함수를 갖는 함수를 고차함수라 부른다.
* 스위프트에서 유용한 고차함수는 맵, 필터, 리듀스가 있다.

### 맵

```swift
// for-in 구문과 map

let numbers = [0, 1, 2, 3, 4, 5]
var doubledNumbers = [Int]()

// for-in 구문
for number in numbers {
    doubledNumbers.append(number * 2)
}
print(doubledNumbers) // [0, 2, 4, 6, 8, 10]

// map
doubledNumbers = numbers.map { (number) -> Int in
    return number * 2
}
print(doubledNumbers) // [0, 2, 4, 6, 8, 10]

// return 생략
doubledNumbers = numbers.map { $0 * 2 }
print(doubledNumbers) // [0, 2, 4, 6, 8, 10]
```

### 필터

```swift
let numbers = [0, 1, 2, 3, 4, 5]
var evenNumbers = [Int]()

// filter
evenNumbers = numbers.filter({ (number) -> Bool in
    return number % 2 == 0
})
print(evenNumbers) // [0, 2, 4]

// return 생략
evenNumbers = numbers.filter({ $0 % 2 == 0 })
print(evenNumbers) // [0, 2, 4]
```

### 리듀스

```swift
let numbers = [0, 1, 2, 3, 4, 5]

// reduce
let sum = numbers.reduce(0) { $0 + $1 }
print(sum) // 15

let subract = numbers.reduce(0) { $0 - $1 }
print(subract) // -15
```

---

### 컴팩트맵

* 컴팩트맵은 옵셔널을 알아서 처리한다.

```swift
let numbers: [Int?]
numbers = [0, 1, 2, 3, nil, 5]

let mapResult = numbers.map { $0 }
print(mapResult) // [Optional(0), Optional(1), Optional(2), Optional(3), nil, Optional(5)]

let compactMapResult = numbers.compactMap { $0 }
print(compactMapResult) // [0, 1, 2, 3, 5]

```
