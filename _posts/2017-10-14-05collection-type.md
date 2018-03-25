---
title: 컬렉션 타입
layout: post
---

# 컬렉션 타입

### 컬렉션 타입

* Swift는 값의 모음을 저장하기 위해 배열(Array), 집합(Set), 사전(Dictionary), 튜플(Tuple)을 제공한다.
* 튜플을 제외하고는 저장되는 값이 모두 같은 타입이어야만 한다.
* 당연하게도 컬렉션 타입을 var(변수)에 할당하면 변경이 가능, let(상수)에 할당하면 변경이 불가능하다.

### 배열(Array)

* 배열은 정렬된 값의 모음이다.
* 배열은 Index(0부터 시작하는 순서)를 가지고 있다.
* Index를 통해 배열에 저장된 값을 불러올 수 있다.
* append, insert, ,contentsOf, remove 등을 사용하여 수정이 가능하다.

```swift
var someArray: [Int] = [1, 2, 3]
someArray[0]
//1
someArray[1]
//2
someArray.count
//3

var someNum: Int = 5
someArray.append(someNum)
//[1, 2, 3, 5]
someArray.insert(someNum, at: 1)
//[1, 5, 2, 3, 5]
someArray.append(contentsOf: [9, 10, 11])
//[1, 5, 2, 3, 5, 9, 10, 11]

var cities: [String] = ["Seoul", "Tokyo", "New York", "Santiago"]
var length = cities.count

//조건이 변수에 저장되어 효율적
for i in 0..<length {
    print("\(i)번째 도시는 \(cities[i])입니다")
}

for i in cities {
    let index = cities.index(of: i) //.index(of: ) 메소드는 String을 인자값으로 받고 Int?를 리턴한다.
    print("\(index!)번째 도시는 \(i) 입니다.")
}
```

* 배열의 순회탐색을 위해서 for-in 구문내에서 배열의 .count를 사용하는 경우, 배열의 크기가 작다면 상관없지만 크기가 크다면 변수나 상수에 할당하여 설정하는 것이 더욱 좋다. 그 이유는 for-in 구문은 조건식을 평가할때마다 배열의 크기를 매번 다시 계산하기 때문이다.
* 배열이 비어있는지 확인하기 위해서는 .isEmpty 를 사용할 수 있다.
* 특정값이 있는 확인 하기 위해서는 .contains() 을 사용할 수 있다.
* index(of: )는 값의 index를 알 수 있다.

### 집합(Set)

* 집합은 정렬되지 않은 고유한 값이다.
* 순서가 중요하지 않거나 한번만 표시해야 하는 경우 배열 대신 사용된다. 즉 집합은 중복 없이 저장하고자 할때 사용된다.
* 그렇기 때문에 이미 있는 값을 저장하면 아무일도 발생하지 않는다.
* 집합은 intersection, symmetricDifference,union, subtract 등을 사용하여 연산이 가능하다.

```swift
var randomSet: Set<String> = [] //초기값이 없을 경우 제네릭을 통해 타입을 명시해야 한다.
var someSet: Set = [3, 2, 1]
var someSet2: Set = [1, 2, 5]

var someSet.insert(3) //[3, 2, 1]
someSet.intersacton(someSet2) //[1, 2]
someSet.symmetricDifference(someSet2) //[3, 5]
someSet.union(someSet2) //[1,2,3,5]
someSet.subtrat(someSet2) //[3]

var someArray: [Int] = [1, 1, 3, 3, 5, 5, 7, 7, 9, 9]
var someSet = Set(someArray) //집합의 특성상 중복값이 제거된다. 그러나 순서는 정렬되지 않는다.
//[9, 1, 3, 7, 5]
```

```swift
var someSet: Set = [1, 3]
someSet.insert(3) // [1, 3]
someSet.update(with: 3) // [1, 3]
```

* 위 경우 결과값의 차이는 없지만 insert의 경우 중복된 값을 더하려는 이유로 false를 반환하고 값을 더하지 않는다. (중복이 없기 때문)
* 반면 update의 경우 값이 있든 없든 무조건 새 값으로 변경이 된다.

### 사전(Dictionary)

```swift
var someDic: [String : Int] = ["경기도" : 031, "서울" : 02]

someDic["경기도"] //031
```

* 키 - 값 연관의 정렬되지 않은 모음이다.
* 키 값을 통해 데이터에 접근한다. 이 때문에 키 값은 중복될 수 없다.
* 아이템에 index 는 없지만 키에는 내부적으로는 순서가 있기에 for-in 구문을 통해 순회 탐색이 가능하다.
* updateValue 메소드는 기존의 아이템이 없다면 추가를 하고 nil을 반환, 이미 존재한다면 기존 값을 반환한다.
* 저장된 값을 제거할 때에는 nil값을 주는 방법과 removeValue 메소드 두 가지를 사용할 수 있다. reomoveValue를 사용하면 삭제된 값은 removedValue로 사용할 수 있다.

### 튜플(Tuple)

* 튜플은 한 가지 타입의 아이템만 저장할 수 있는 다른 컬렉션 타입과 다르게 여러 가지의 아이템을 동시에 저장할 수 있다.
* 그러나 상수적 성격을 띠므로 값을 추가하거나 삭제하는 것은 불가능하다.
* 튜플은 배열의 index와는 다르게 속성을 통해 이용한다.
* 튜플은 바인딩을 통해서 각각을 변수나 상수로 할당하는 것이 가능하다.

```swift
var someTuple: (Int, String, Bool) = (3, "안녕", true)
//속성을 통한 접근
someTuple.0 //3
someTuple.1 //안녕
someTuple.2 //true

//바인딩을 통한 할당
let (a, b, c) = someTuple
a //3
b //안녕
c //true
```
