---
title: contentInset과 contentOffset의 차이
layout: post
---

# contentInset과 contentOffset의 차이

### contentInset

* 애플문서: content view와 safe area 혹은 scroll view edges와의 거리.

### contentOffset

* 애플문서: content view의 원점과 부모뷰간의 원점 거리.

### 차이점

* 분명한 차이점은 contentOffset의 경우 원점간의 거리일뿐 추가공간과는 관련이 없고, 반면 contentInset은 추가공간을 정의한다는 것이다.

```swift
contentInset = UIEdgeInsetsMake(66.0, 0.0, 0.0, 0.0)
// Scrolled all the way to the top
 ________________  
|                |
|       66       |
|  ############  | 
|  #          #  |
|  #          #  |
 --# ---------#--  
   #          #  
   #          #  
   #          #
   #          #
   #          #
   #          #
   ############



contentOffset = CGPoint(x: 0.0, y: 42.0)  
   ############
   #    42    #
 __#__________#__  <------ content view's y = 42
|  #          #  |
|  #          #  |
|  #          #  |
|  #          #  |
|  #          #  |
 --#----------#--
   #          #
   ############  
```
