---
title: 로테이트 애니메이션 만들기
layout: post
---

# 로테이트 애니메이션 만들기

### 360도 회전하는 애니메이션

```swift
func rotateAnimation() {
        if self.isUpdating == true { // 특정 조건이 만족하는 동안 계속해서 동작
        
            UIView.animate(withDuration: 0.5, delay: 0.0, options: .curveEaseIn, animations: { // curveEaseIn 옵션은 시작은 느리게, 점점 가속도가 붙는다.
                self.refreshButton.transform = CGAffineTransform(rotationAngle: CGFloat.pi)
            }, completion: nil)
            
            UIView.animate(withDuration: 0.5, delay: 0.45, options: .curveEaseOut, animations: { // curveEaseOut 옵션은 시작은 빠르게, 점점 느려진다.
                self.refreshButton.transform = CGAffineTransform(rotationAngle: CGFloat.pi * 2.0)
            }) { (bool) in
                if bool == true {
                    self.rotateAnimation()
                }
            }
        }
    }
```

### 결과

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-19/01.gif?raw=true">

### 응용

* 이걸 응용하면 + 버튼을 누르면 x버튼이 되도록 로테이트하는 등의 다양한 방식의 UI&UX적인 표현을 할 수 있다.
