---
title: 접근 제어
layout: post
---

### 접근 제어

* 접근 제어의 종류는 Open, Public, Internal, Fileprivate, Private 총 5개 이다.
* 이 중 Open과 Public은 모듈 단위에서 사용되는 접근제어이다.
* Internal은 가장 기본이 되는 형태이며 키워드를 생략한 상태로 보통 쓰는 방식이다.
* Fileprivate와 Private는 소스 파일 단위에서 사용되는 접근 제어이다.
* 즉, 기본은 모두 Internal이며 모듈 단위에서는 Open, Public을 쓰며, 소스 파일 단위에서는 Fileprivate과 private을 사용한다.

### 모듈

* 모듈이란 하나의 앱, framework 등 덩어리라고 볼 수 있다.
* UI를 사용하기 위해서는 UIkit 이라는 덩어리를 import 시켜야 하며, 문자열이나 시간, 데이터 등 다양한 변수나 함수를 사용하기 위해 Foundation 이라는 덩어리를 import 시키듯 이 한 덩어리가 모듈이다.
* 이 덩어리들이 모여 완성된 하나의 앱도 모듈이다. 즉 제일 작은 덩어리를 포함하는 모든 덩어리는 모듈이다. (최하 단위가 framework라 볼 수 있다.)

### 소스 파일

* 소스 파일이란 Xcode 사용시 흔히 볼 수 있는 블라블라.swift에 해당하는 것이다.
* 새 프로젝트를 만들면 기본적으로 생성되는 ViewController.swift도 소스 파일이다.
* 이런 소스 파일들이 모여 하나의 모듈이 완성된다.

### Open과 Public의 차이

* Open 접근 수준은 모듈 외부 즉, import한 주체 프로젝트에서 상속과 재정의가 가능하다.
* Public 접근 수준은 모듈 외부 즉, import한 주체 프로젝트에서 사용은 가능하나 상속이나 재정의는 불가능하다.

### Internal

* 가장 기본이 되는 Internal은 정의된 모듈에서만 사용이 가능하다.
* 즉, 모듈 외부에서는 사용이 불가능하다.

### Fileprivate과 Private의 차이

* Fileprivate 접근 수준은 소스 파일에서만 사용된다.
* 즉, Fileprivate 접근 수준은 정의된 모듈이 아니라 블라블라.swift에서만 사용이 가능하다.
* Fileprivate는 상속과 재정의가 가능한데, 상속할 인스턴스도 Fileprivate 접근 수준이어야만 한다. (Private도 가능)
* Private 접근 수준은 소스 파일 내부에서도 접근을 원치 않는 요소에서 사용한다.

### 한 줄 요약

* Open 접근 수준은 import 하면 상속, 재정의든 마음대로 할 수 있다.
* Public 접근 수준은 import 하면 사용만 할 수 있다.
* Internal 접근 수준은 import 해도 사용할 수 없다.
* Fileprivate 접근 수준은 소스 파일 내부에서만 사용할 수 있다. (같은 접근 수준이라는 전제하에)
* Private 접근 수준은 소스 파일 내부에서도 접근을 막을 수 있다.
