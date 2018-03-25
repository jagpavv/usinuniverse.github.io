---
title: 조건문
layout: post
---

### 조건문

* 조건문이란? 특정 조건에 따라 선택적으로 코드의 실행을 분기하는 역할을 한다.
* 대표적인 조건문은 if-else, switch-case, guard-else 가 있다.

### if-else

```swift
if 조건 {
    //조건이 만족되면 실행
} else {
    //조건이 만족되지 않을 때 실행.
}
```

```swift
if 조건 {
    //조건이 만족되면 실행
} else if 조건 {
    //조건이 만족되면 실행
} else if 조건 {
    //조건이 만족되면 실행
} else {
    모두 불만족시 실행
}
```

* 위와 같이 else if를 추가하여 조건을 추가할 수 있다.
* 이때 조건값은 참, 거짓으로 나타나는 Bool값으로 표현해야 한다. 즉, 비교 연산자, 논리 연산자 등을 통해 조건의 결과가 Bool 값으로 나와야 한다.
* 연산자를 이용하여 다양한 조건의 조합이 가능하다.

### switch-case

```swift
switch value {
    case 1:
    이 값에 따른 실행구문
    case 2, 3:
    이 값에 따른 실행구문
    default:
    //break 혹은 모든 조건 불만족시 실행구문
}
```

* switch-case 는 패턴 비교문으로, 가장 처음으로 매칭되는 패턴의 구문이 실행된다. 즉 일치하는 조건이 여러개이더라도 처음 만족하는 조건만 실행된다.
* 각 case 끝에는 : 을 통해 패턴 매칭을 종료한다.
* 여러개의 case는 , 를 통해 추가한다.
* 각 case 별로 실행되며, 매칭되어 해당 case 문이 종료되면 switch 문을 종료 시킨다.
* 만약 매칭되는 값이 없다면 default를 통해 기본 매칭값을 설정해야만 한다. 그러나 완전하게 작성된 switch구문에서는 default가 생략될 수 있다.
* where 절을 사용하여 조건부를 작성할 수 있다. (switch 외에도 다양한 곳에서 사용이 가능하다.)

### guard-else

* if와는 정반대의 개념으로 실행된다.
* guard 구문은 else 블록이 필수이지만 표현식의 결과가 참일 경우에 실행할 블록은 없다. (물론 넣을 수는 있다.)
* 조건이 참이 아닐 경우에 실행될 코드만 지정하여 전체적인 코드의 가독성을 높일 수 있다.
* 후속 구문들이 실행되기 전 특정 조건을 확인하여 조기 종료하기 위한 목적으로 주로 사용된다.

```swift
var randomNum: Int? = 0

func nothing(inputNum: Int?) {
    guard inputNum != nil else {
        print("값이 없네요")
        return
    }
    print("값이 있네요")
}

print(nothing(inputNum: randomNum)) // 값이 있네요
```

* 위의 경우 randomNum의 값이 있기에 가드문을 통과한다.
* 만약 randomNum의 값이 nil일 경우 return을 통해 함수가 종료된다.

### if vs guard

```swift
func login() {
    var networkSuccess: Bool = true
    var idMatch: Bool = true
    var passwordMatch: Bool = true
    
    if networkSuccess == true {
        if idMatch == true {
            if passwordMatch == true {
                //로그인 시도
            }
        }
    }
}

```

* 가령 이처럼 간단한 코드도 if문이 중첩될 수록 가독성이 떨어지게 된다. 실전에서는 이보다 더욱 복잡해지기 때문에 코드의 가독성을 높이는 것이 향후 유지보수에도 더욱 수월할 수 밖에 없다.

```swift
func login() {
    var networkSuccess: Bool = true
    var idMatch: Bool = true
    var passwordMatch: Bool = true

    guard networkSuccess == true else { return }
    guard idMatch == true else { return }
    guard passwordMatch == true else { return }
    //로그인 시도
}
```
