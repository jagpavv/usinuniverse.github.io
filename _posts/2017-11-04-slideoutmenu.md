---
title: Slide-out Menu 만들기
layout: post
---

### Storyboard 세팅

* View 위에 Slide out menu view를 미리 만들어 둔다.
* Slide out menu view 의 constraints를 적절히 구현한다. (이번 예제에서는 Leading constraints를 이용할 예정)
* 이때 View 에는 열림, Slide out menu view 에는 닫기 버튼을 만들었다.
* 이 버튼들로 constraints를 조정해 Slide out 효과를 구현한다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/17-11-04/01.png?raw=true">

### Source code 세팅
* ViewController.swift

```swift
import UIKit

class ViewController: UIViewController {
    // slide out menu
    @IBOutlet weak var slideOutView: UIView!
    // leading constraints
    @IBOutlet weak var leadingConstraints: NSLayoutConstraint!
    
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // slide out view의 가로길이만큼 constraints를 주어야 하기 때문에 가로 길이를 계산
        let constant = slideOutView.frame.size.width
        // slide out view의 constraints 변경 (기본값)
        leadingConstraints.constant = -constant
    }

    // 열기 버튼
    @IBAction func presentView(_ sender: UIButton) {
        // slide out view의 constraints 초기화
        leadingConstraints.constant = 0
        
        // 애니메이션 효과 부여
        UIView.animate(withDuration: 0.3) {
            self.view.layoutIfNeeded()
        }
        
    }
    
    // 닫기 버튼
    @IBAction func dismiss(_ sender: UIButton) {
        // 다시 constraints 변경 (초기값으로)
        let constant = presentedView.frame.size.width
        sideConstraints.constant = -constant
        
        // 애니메이션 효과 부여
        UIView.animate(withDuration: 0.3) {
            self.view.layoutIfNeeded()
        }
    }
}
```

* 다음과 같이 작동하는 것을 볼 수 있다. (다양하게 응용가능)
* GIF 품질 때문에 Slide out menu view가 view 전체를 덮는 것 같지만, 실제론 미리 정해둔 가로 길이만큼만 나온다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/17-11-04/02.gif?raw=true" width="50%">

