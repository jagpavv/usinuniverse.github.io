---
title: 클래스와 구조체
layout: post
---

### 클래스와 구조체

* 클래스와 구조체는 기본적으로 하나의 큰 코드 블록이다.
* 클래스와 구조체에서는 변수나 상수는 프로퍼티로, 함수는 메소드로 불린다.
* 프로퍼티나 메소드는 클래스 혹은 구조체의 멤버다.
* Swift는 객체지향언어로, 필요한 기능을 객체로 구현하여 사용한다. 이때 객체를 만들어내는 주요 대상이 바로 클래스와 구조체이다.

### 클래스와 구조체의 공통점

* 프로퍼티나 메소드를 정의할 수 있다.
* 객체를 원하는 상태로 초기화하는 초기화 블록을 제공한다.
* 객체에 함수적 기능을 추가하는 확장(extends)구문을 사용할 수 있다.
* 프로토콜을 구현할 수 있다.

### 클래스만이 갖는 특징

* 상속이 가능하다.
* 인스턴스가 소멸되기 직전 실행할 구문을 미리 등록할 수 있다.
* 클래스는 참조에 의한 전달, 구조체는 값에 의한 전달이다.
* 참조에 의한 전달은 주소값을 전달하기 때문에 각각 다른 인스턴스도 같은 주소를 가리키게 되어 한 인스턴스에서의 변화는 다른 인스턴스에 영향을 준다. 
* 또한, 클래스는 참조에 의한 전달로 값 비교는 일반 ==이 아닌 === 즉 같은 메모리를 참조하는지를 비교해야 한다.

```swift
class StudentA {
    var name: String = "안재형"
    var score: Double = 99.9

    func sleep(time: Double) {
        print("중간고사 시험에서 \(score)점 맞은 \(name)은 \(time)시간 잔다.")
    }
}

var studentA: StudentA = StudentA()
var ajh = studentA
ajh.name = "안 재형"
studentA.name //안 재형
studentA === ajh //true
```

* 반면 값에 의한 전달은 값을 복사한 형태로 가지고 오기 때문에 인스턴스는 서로 완전히 다른 인스턴스이다. 그러므로 서로 영향을 줄 수 없다.

```swift
struct StudentB {
    var name: String = "불량학생"
    var score: Double = 15.1

    func sleep(time: Double) {
        print("중간고사 시험에서 \(score)점 맞은 \(name)은 \(time)시간 잔다.")
    }
}

var studentB: StudentB = StudentB()
var random = studentB
random.name = "랜 덤 학 생"
studentB.name //불량학생
```

### 클래스와 구조체의 정의

```swift
class StudentA {
    var name: String = "안재형"
    var score: Double = 99.9
    
    func sleep(time: Double) {
        print("중간고사 시험에서 \(score)점 맞은 \(name)은 \(time)시간 잔다.")
    }
}

struct StudentB {
    var name: String = "불량학생"
    var score: Double = 15.1
    
    func sleep(time: Double) {
        print("중간고사 시험에서 \(score)점 맞은 \(name)은 \(time)시간 잔다.")
    }
}
```

### 클래스와 구조체의 사용

* 클래스와 구조체는 정의만으로는 아무런 의미가 없다.
* 클래스 혹은 구조체를 통해 인스턴스를 만들어야만 비로소 사용이 가능하다.
* 클래스 혹은 구조체는 인스턴스를 만들기 위한 일종의 ‘틀’이다.

```swift
var studentA: StudentA = StudentA() //변수 studentA StudentA를 할당하여 프로퍼티와 메소드 사용이 가능해진다.

studentA.name //안재형
studentA.score //99.9
studentA.sleep(time: 9.1) //중간고사 시험에서 99.9점 맞은 안재형은 9.1시간 잔다.

var studentB: StudentB = StudentB() //변수 studentB에 StudentB를 할당하여 프로퍼티와 메소드 사용이 가능해진다.

studentB.name //불량학생
studentB.score //15.1
studentB.sleep(time: 23.9) //중간고사 시험에서 15.1점 맞은 불량학생은 23.9시간 잔다.
```

* 프로퍼티 혹은 메소드로의 접근은 인스턴스를 통해서만 가능하다.
* 인스턴스를 만든 후 Dot Syntax를 통해 접근한다.

### 클래스와 구조체의 초기화

* 클래스와 구조체는 인스턴스를 만드는 과정에서 반드시 프로퍼티 초기화가 이루어져야만 한다.
* 초기화의 방법으로는 선언과 동시에 초기화, 초기화 메소드 사용, 옵셔널 타입으로 선언의 방법이 있다.

```swift
class StudentA {
    var name: String
    var score: Double

    //초기화 메소드 사용
    init() {
        self.name = "안재형"
        self.score = 99.9
    }
    
    func sleep(time: Double) {
        print("중간고사 시험에서 \(score)점 맞은 \(name)은 \(time)시간 잔다.")
    }
}

var studentA: StudentA = StudentA()
studentA.name //안재형
studentA.score //99.9
```

```swift
class StudentA {
    var name: String? //옵셔널 타입으로 선언
    var score: Double? //옵셔널 타입으로 선언

    func sleep(time: Double) {
        print("중간고사 시험에서 \(score)점 맞은 \(name)은 \(time)시간 잔다.")
    }
}

var studentA: StudentA = StudentA()
studentA.name //nil
studentA.score //nil
```

* 구조체의 경우 특별하게 멤버와이즈 초기화 구문을 제공한다.

```swift
struct StudentB {
    var name: String = "불량학생"
    var score: Double = 15.1
    
    func sleep(time: Double) {
        print("중간고사 시험에서 \(score)점 맞은 \(name)은 \(time)시간 잔다.")
    }
}

var studentB: StudentB = StudentB() //name = "불량학생", score = 15.1

//멤버와이즈 초기화
var studentBv2: StudentB = StudentB(name: "성실학생", score: 75.5) //name = 성실학생, score = 75.5
```

* 초기화시 원하는 프로퍼티만을 선택적으로 초기화하는 구문을 추가할 수 있다.
* 이 경우 어떤 인자값도 받지 않는 기본 초기화 구문은 사용이 불가능하다.

```swift
class A {
    var name: String = "이름"
    var age: Int = 19
}

var test: A = A() //기본 초기화 구문

class B {
    var name: String = "이름"
    var age: Int?
    
    init(age: Int) { //초기화 구문 설정
        self.age = age
    }
}

var test2: B = B() //에러 -> init(age:) 라는 초기화 구문을 설정했기 때문
var test2: B = B(age: 20)
```

* 이처럼 초기화 구문을 별도로 지정한 경우 해당 구문을 이용한 초기화만이 사용가능하다.
* 만약 기본 초기화 구문을 계속 사용하고 싶다면, 클래스 내부에 기본 초기화 구문을 정의해야만 하며, 이때 모든 프로퍼티는 옵셔널로 선언이 되거나 초기화 전까지 반드시 값이 정의되어야만 한다.

### 클래스와 구조체의 선택

* 구조체를 선택하는 경우는 다음과 같다.
* 서로 연관된 몇 개의 데이터를 묶는 것이 목적일 때
* 캡슐화된 데이터에 상속이 필요하지 않을 때
* 캡슐화된 데이터의 전달이 값에 의한 전달이 합리적일 때
* 캡슐화된 원본 데이터를 보존해야 할 때
* 이 외에 경우에는 클래스를 사용하는 것이 좋다.
