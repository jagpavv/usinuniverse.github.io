---
title: MVC 패턴
layout: post
---

### MVC 패턴이란?
* Model - View - Controller 의 약자로 각각의 역할에 따라 나누어 부피가 커져도 유지보수가 용이하도록 만든 디자인 패턴이다.
* MVC 패턴을 사용하면 객체들의 재사용성이 높아지고, 쉽게 확장할 수 있다.
<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/17-11-01/01.png?raw=true">

### Model
* Model은 데이터를 담당한다.
* 가장 많이 드는 예로 계산기의 경우 값을 계산하고, 저장하는 역할이라고 할 수 있다.

### View
* View는 말그대로 View(보여지는 영역)을 담당한다.
* 계산기의 경우 결과를 보여주고, 유저가 입력한 값을 보여주는 역할을 한다.

### Controller
* Controller는 View와 Model을 이어주는 역할이다.
* View의 변화를 Model에 전달하고, 반대로 Model의 변화를 View에 전달한다.

### iOS 에서의 MVC 패턴
* MVC 패턴을 표현한 그림에서 알 수 있듯이 View와 Model은 서로 독립적이다.
* Model과 View는 자신을 Control 하는 Controller에 대해서 아무런 정보도 없다.
* Model과 View는 어떻게 Controller와 상호작용할까?

-----

### Controller -> View
* Controller가 View에게 말을 거는 방법은 @IBOutlet 이다.
* View에서 Controller로 Outlet을 걸기 때문에 View가 Controller에게 말을 거는 것 같지만, 사실은 Outlet을 통해 Controller가 '니가 보여줄 메시지 혹은 사진 등은 이것이다.' 라고 말을 거는 것이다.

### Controller -> Model
* Controller가 Model에게 말을 거는 방법은 Model 클래스의 인스턴스를 생성하는 것이다.

### View -> Controller
* View가 Controller에게 말을 거는 방법은 두 가지 이다. (@IBAction, Delegate)
* IBAction은 Target-Action 이라고도 불리는데 자신(Controller)에게 과녁(Target)을 설정하고 화살(Action)을 건네주는 방식이다.
* 가령 버튼에서 Action이 발생하면 Controller에서 이에 대한 응답을 하는 형식이다.
* Delegate는 View가 자신에 대한 제어권을 Controller에게 위임하여 응답하는 형식이다. (did, will, should 등)

### Model -> Controller
* Model이 Controller에게 말을 거는 방법도 두 가지 이다. (Notification, KVO)
* Model이 바뀐 값을 직접 전달하는 것이 아닌, 값이 바뀌었다는 Notification을 통해 Controller가 접근할 수 있도록 한다.
* KVO는 Key-Value Observer 의 약자로 프로퍼티에 Observer를 통해 값의 변화를 감지하고 반응할 수 있도록 한다.
