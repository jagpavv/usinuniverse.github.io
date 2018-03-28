---
title: 뷰 컨트롤러 크기 조절
layout: post
---

### 뷰 컨트롤러 크기 조절

* 뷰 컨트롤러의 크기를 바꾸는 방법은 여러가지가 있는데 그 중에서도 오늘은 간단한 트릭을 사용할 예정이다.
* VC1 에서 Show 버튼을 누르면 VC2가 Present 되는데, 이때 VC2의 크기가 빨간색 네모 만큼만 올라오게 할 것이다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/raw/master/assets/images/posting%20images/18-03-28/01.png?raw=true">

* 그러기 위해서는 
* (1) VC1 외 VC2를 만들고
* (2) VC2의 view의 배경의 알파값을 0으로 만들어 투명한 상태로 만들어야 한다.
* (3) 그 후 원하는 뷰를 새로 그린다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/raw/master/assets/images/posting%20images/18-03-28/02.png?raw=true">
<img src="https://github.com/usinuniverse/usinuniverse.github.io/raw/master/assets/images/posting%20images/18-03-28/03.png?raw=true">
<img src="https://github.com/usinuniverse/usinuniverse.github.io/raw/master/assets/images/posting%20images/18-03-28/04.png?raw=true">

### VC1.swift

```swift
import UIKit

class VC1: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        // Do any additional setup after loading the view.
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
   
    @IBAction func showButtonDidTap(_ sender: UIButton) {
        let vc2 = storyboard?.instantiateViewController(withIdentifier: "vc2") as! VC2
        
        // 이 부분이 중요! -> presentation style을 바꾸지 않으면 배경색을 투명하게 적용한 것 때문에 뒷 배경이 아예 검은 화면이 나타날 수 있다.    
        vc2.modalPresentationStyle = .overCurrentContext
        
        present(vc2, animated: true, completion: nil)
    }
}
```

* 실행 결과는 다음과 같다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/raw/master/assets/images/posting%20images/18-03-28/05.png?raw=true">
<img src="https://github.com/usinuniverse/usinuniverse.github.io/raw/master/assets/images/posting%20images/18-03-28/06.png?raw=true">

* VC2가 present 됐음에도 VC1이 보이지만 VC1과 유저간의 상호작용은 불가한 상태가 된다.
* VC2에 블러효과나 그림자 효과등 다양한 효과를 통해 확실한 분리 느낌을 줄 수 있다.
* 이를 응용하면 다양한 크기의 커스텀 뷰 컨트롤러를 만들 수 있다.
