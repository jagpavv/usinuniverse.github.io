---
title: 컬렉션뷰 셀 나누기
layout: post
---

# 컬렉션뷰 셀 나누기

### 정사각형

* 컬렉션뷰는 테이블뷰에 비해서 훨씬 자유도가 높다.
* 때문에 몇가지 세팅이 더 필요한 점도 있다.
* 그 중 가장 간편하게 정사각형으로만 구성된 컬렉션뷰 셀을 구성하는 방법을 소개한다.

```swift
override func viewDidLoad() {
  super.viewDidLoad()
  
  let width = (self.view.frame.size.width - 20) / 3
  let layout = self.mainCollectionView.collectionViewLayout as! UICollectionViewFlowLayout
  layout.itemSize = CGSize(width: width, height: width)
}
```

* 이렇게 하면 정확히 3등분된 정사각형 셀이 생성된다.
* 코드에 대해 상세 설명하자면, view width 에서 -20을 하는 이유는 컬렉션뷰의 Size Inspector에 가보면 Min Spacing을 찾을 수 있다. 이 Min Spacing이 3등분된 셀중 옆 셀과 겹치는 부분의 값이기 때문에 3등분된 셀은 2개의 셀이 겹치는 부분이 2개라 10 * 2 해서 -20을 해야한다.
* 설명이 복잡하지만 그냥 외우고자 한다면 셀이 2등분인 경우 Min Spacing * 2(등분), 4등분인 경우 Min Spacing * 4(등분) 을 width에서 빼고 다시 등분으로 나누면 된다.
* 또 collectionViewLayout을 UICollectionViewFlowLayout으로 캐스팅 하는 이유는 collectionViewLayout에는 itemSize가 없기 때문이다.

### 결과

* `let width = (self.view.frame.size.width - 10) / 2`

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-08-31/01.png?raw=true">

* `let width = (self.view.frame.size.width - 20) / 3`

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-08-31/02.png?raw=true">

* `let width = (self.view.frame.size.width - 30) / 4`

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-08-31/03.png?raw=true">
