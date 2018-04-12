---
title: 사이즈 클래스 이용하여 화면 구성하기
layout: post
---

### 사이즈 클래스 이용하여 화면 구성하기

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-04-11/06.png?raw=true">

* 사이즈 클래스를 이해하기 위해서는 위 그림을 참고해야 한다.
* Regular는 평균적인 크기이며 Compact는 말 그대로 작은 크기로 보면 된다.
* Portrait orientation(일반적인 사용 형태로 세로로 세워서 사용하는 형태) 하에서는 아이폰은 기종과 상관없이 Regular 높이와 Compact 너비를 가지고 있다.
* 하지만 Landscape Orientation(가로로 눕혀서 사용하는 형태) 하에서는 Plus 모델의 경우 Compact 높이와 Regular 너비를 가지며, 그 외 기종(X 포함)은 Compact 높이와 Compact 너비를 가진다.
* 정리하자면 아이폰의 경우 기본적으로 세로모드에서는 Regular 높이와 Compact 너비로 모두 동일하며, 가로모드에서는 Plus 모델만 Compact 높이와 Regular 너비를 가진다.
* 따라서 Compact와 Regular 사이에 각각 다른 화면을 만들 수 있다. (아이폰SE ~ 아이폰X의 가로모드에서는 안보이던 버튼이 Plus 모델의 가로모드에서는 보이게끔.)

### 실습

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-04-11/01.png?raw=true">

* 위 상황에서 가로모드는 일반적으로 다음과 같다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-04-11/02.png?raw=true">

* 그러나 Regular 너비에서는 특정 뷰가 더 생겼으면 좋겠다. 그럴 때에는 스토리보드 하단에 Vary for Traits 버튼을 눌러 변경하고 싶은 width와 height를 선택한다.
* 아래는 Regular 너비와 Compact 높이인 Plus 모델에만 적용될 스토리보드라고 이해하면 된다. (스토리보드 하단이 파란색으로 변경)
* Plus 모델에만 적용하고 싶은 화면을 완성한 후 Done Varing 을 누른다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-04-11/03.png?raw=true">

* 그러면 아까와는 다른 가로모드 화면이 보인다. Plus 모델에서만 적용이 되는지 확인해보자.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-04-11/04.png?raw=true">

* 보다시피 Plus 모델이 아닌 경우에는 처음 설정한 화면이 나오는 것을 확인할 수 있다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-04-11/05.png?raw=true">
