---
title: 앱 생명주기(3)
layout: post
---

### 앱 실행 상태

* 어떤 순간에도 앱은 아래의 그림의 상태 중 하나의 상태일 것이다.
* 시스템은 시스템 전체에서 일어나는 일에 응답하며 앱의 상태를 변화시킨다.
* 가령 유저가 홈 버튼을 누르거나, 전화가 오거나, 혹은 다른 몇 가지 중단이 발생하면 앱의 상태가 변경된다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/17-10-31/01.png?raw=true">

* Not running -> 앱이 실행되지 않았거나 혹은 시스템에 의해 종료된 상태
* Inactive -> 앱이 foreground에 실행되어 있지만 이벤트를 받지 않고 있는 상태. 보통 다른 상태로 전환될 때 잠깐 동안 이 상태에 머물러 있는다. (다른 코드를 실행 중일 수도 있다.)
* Active -> 앱이 foreground에 실행중이고 이벤트를 받는 상태. 이 상태가 foreground에서의 가장 기본 상태이다.
* Background -> background에 있으며, 코드를 실행하고 있는 상태. 대부분의 앱은 일시 중지 상태로 이 상태가 된다. 그러나 background에서 추가적인 실행이 필요한 경우 일정 기간 이 상태에서 추가적인 실행 시간이 주어지기도 한다. 또 background에서 직접 실행되는 앱은 background 대신 inactive 상태로 전환된다.
* Suspended -> 이 상태가 되면 앱은 메모리에 남아 있지만, 코드는 실행하지 않는다. 만약 메모리가 부족하면 예고없이 이 상태의 앱을 제거하여 더 많은 메모리를 확보하기도 한다.

### 앱 상태의 변화

* 대부분의 앱 상태 변화에는 그에 따른 적절한 메소드가 수반되기 때문에 상태변화에 따른 적절한 대응을 할 수 있도록 도와준다.
* application:willFinishLaunchingWithOptions -> 이 메소드는 가장 처음 코드를 실행할 수 있는 구간이다.
* application:didFinishLaunchingWithOptions -> 이 메소드는 앱이 유저에게 보여지기 전 최종으로 초기화 할 수 있는 구간이다.
* applicationDidBecomeActive -> 앱이 foreground가 되는 순간.
* applicationWillResignActive -> 앱이 active 상태에서 inactive가 되는 상태.
* applicationDidEnterBackground -> 앱이 background에서 실행중이며 언제든지 suspended가 될 수 있는 상태.
* applicationWillEnterForeground -> 앱이 background에서 foreground로 돌아오는 순간. 그러나 아직 active 상태는 아니다.
* applicationWillTerminate -> 앱이 종료된다. 앱이 suspended 상태에서는 호출되지 않는다.

### 앱 종료

* 앱은 언제든지 종료될 수 있도록 준비되어야하며 다른 업무나 유저의 데이터를 저장하기 위해 시간을 쓰거나 동작하면 안된다.
* 시스템의 초기화와 종료는 앱 수명 주기의 정상적인 부분이다.
* 시스템은 다른 앱의 실행을 위해 앱을 종료하기도 하지만, 오작동하거나 이벤트에 응답하지 않는 앱도 종료하곤 한다.
* suspended 상태의 앱은 종료될 때 알림을 받지 않는다. 앱이 현재 background에서 실행중이며 suspended 되지 않은 경우에는 종료하기 전 applicationWillTerminate 를 호출한다.
* 사용자가 직접 앱을 종료할 때도 마찬가지이다.
