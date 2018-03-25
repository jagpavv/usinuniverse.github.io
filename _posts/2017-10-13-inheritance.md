---
title: 상속
layout: post
---

### 상속

* 상속이란? 한 클래스가 다른 클래스의 프로퍼티나 메소드를 물려받아 사용하는 것이다.
* 상속을 통해 다른 클래스의 프로퍼티나 메소드를 구현하지 않아도 사용이 가능하다.
* Swift에서는 다중상속을 지원하지 않는다.

```swift
class Human {
    var gender: String?
    
    func eat(food: String) {
        print("\(food)를 먹는다.")
    }
}

class Male: Human { //Human 클래스를 상속
    var age: Int = 19
}

var male: Male = Male() //인스턴스 생성
male.age //19
male.gender //nil
male.gender = "남자" //상위 클래스의 프로퍼티 수정
male.gender //남자
male.eat(food: "사과") //사과를 먹는다.
```

* 이처럼 클래스를 상속받을 경우 상우 클래스의 프로퍼티나 메소드를 사용할 수 있고, 값도 수정이 가능하다.
* Swift에서 다중상속은 지원하지 않지만, Male 클래스를 또 다른 클래스가 상속하여 무한으로 상속이 가능하다.

### 오버라이딩

* 오버라이딩이란? 상속받은 클래스가 상위 클래스의 프로퍼티나 메소드를 필요에 따라 재정의하여 사용하는 것이다.
* 오버라이딩은 프로퍼티나 메소드를 선언하기 전 override 키워드를 추가한다.
* 프로퍼티의 경우 연산 프로퍼티로만 재정의 가능하며 저장 프로퍼티의 경우 기본적으로 값의 제공(get)과 수정(set)이 가능하므로 get과 set이 제공되는 연산 프로퍼티로만 재정의 되어야 한다. 
* get과 set이 제공되던 연산 프로퍼티를 get만 제공하는 프로퍼티로 재정의 하는 것은 불가능하다.
* 메소드를 재정의하는 경우에는 기존 메소드의 매개변수의 이름, 개수, 타입, 반환값의 타입은 변경할 수 없다.
* 프로퍼티나 메소드를 재정의하였을때 본래의 기능을 제한하는 방향으로는 불가능하다.
* 하위 클래스에서 재정의하였더라도 super 키워드를 통해 상위 클래스의 기능을 사용할 수 있다.

```swift
class A {
    var name: String = "Class A"
    
    func hello(someString: String) {
        print(someString)
    }
}

class B: A { //A클래스 상속
    override var name: String { //name 재정의
        get {
            return "override Test"
        }
        set {
        }
    }
    
    override func hello(someString: String) { //hello 재정의
        print(someString)
    }
    
    func originName() {
        print(super.name)
}

var classB: B = B()
classB.name //override Test
classB.originName //Class A
classB.hello(someString: "안녕") //안녕
```

* 하위 클래스에서의 재정의를 막고자 할 때는 프로퍼티, 메소드 명 혹은 클래스 전체에 final 키워드를 추가하면 된다.

```swift
class A {
    final var name: String = "Class A"
    
    func hello(someString: String) {
        print(someString)
    }
}

class B: A {
    override var name: String { //final로 선언된 name 프로퍼티 사용에 따른 오류
        get {
            return "override Test"
        }
        set {
        }
    }
    
    override func hello(someString: String) {
        print(someString)
    }
    
    func origin() {
        print(super.name)
    }
}

final class A {
    var name: String = "Class A"
    
    func hello(someString: String) {
        print(someString)
    }
}

class B: A { //final로 선언된 클래스 A 상속에 따른 오류
    override var name: String {
        get {
            return "override Test"
        }
        set {
        }
    }
    
    override func hello(someString: String) {
        print(someString)
    }
    
    func origin() {
        print(super.name)
    }
}
```
