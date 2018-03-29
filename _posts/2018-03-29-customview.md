---
title: 커스텀뷰 만들기
layout: post
---

### 커스텀뷰를 만드는 이유

* 간단한 커스텀뷰의 경우는 코드로 바로 만들면 되지만, 뷰 안에 버튼, 이미지, 레이블 등의 복잡한 객체들이 있을 경우에는 미리 정의해둔 커스텀뷰를 쓰는 것이 편하기 때문이다.
* 아래처럼 Show 버튼을 누르면 미리 정의해둔 커스텀뷰가 생성되도록 할 예정이다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/raw/master/assets/images/posting%20images/18-03-29/01.png?raw=true">

* 우선 View를 하나 만들어야 한다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/raw/master/assets/images/posting%20images/18-03-29/02.png?raw=true">

* 그리고 UIView를 상속받은 커스텀뷰 클래스를 만든다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/raw/master/assets/images/posting%20images/18-03-29/03.png?raw=true">

* 그리고 나서 커스텀뷰 클래스와 커스텀뷰를 연결시켜줘야하는데 주의할 것이 File's Owner와 커스텀뷰 클래스를 연결시켜줘야만 한다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/raw/master/assets/images/posting%20images/18-03-29/04.png?raw=true">

* 그 후 원하는 크기로 바꾸기 위해 Size Inspector를 수정하고 원하는 뷰를 생성한다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/raw/master/assets/images/posting%20images/18-03-29/05.png?raw=true">

* 여기서 중요한 것은 커스텀뷰 초기화 메서드를 정의해야만 한다. (메서드의 이름은 중요하지 않고 내용이 중요)
* 코드는 다음과 같다.

### CustomView.swift

```swift
import UIKit

class CustomView: UIView {
    
    //MARK: Variable: IBOutlet
    
    @IBOutlet var mainView: UIView!
    @IBOutlet weak var topView: UIView!
    @IBOutlet weak var centerView: UIView!
    
    
    
    // 스토리보드로 초기화 시킬 때 필요
    required init?(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)
        customInit()
    }
    
    // 코드로 초기화 시킬 때 필요
    override init(frame: CGRect) {
        super.init(frame: frame)
        customInit()
    }
    
    //MARK: Custom Init
    
    private func customInit() {
        Bundle.main.loadNibNamed("CustomView", owner: self, options: nil)
        
        mainView.frame = self.bounds
        mainView.autoresizingMask = [.flexibleWidth, .flexibleHeight]
        
        addSubview(mainView)
    }
    
    //MARK: UI Button Event
    
    @IBAction func buttonDidTap(_ sender: UIButton) {
        mainView.backgroundColor = UIColor.lightGray
        topView.layer.cornerRadius = 30
        centerView.layer.cornerRadius = 20
    }
    
}
```

### ViewController.swift

```swift
import UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }

    @IBAction func showButtonDidTap(_ sender: UIButton) {
        let customView = CustomView(frame: CGRect(x: 85, y: 120, width: 200, height: 350))
        
        view.addSubview(customView)
    }
    
}
```

-----

* 실행 결과는 다음과 같다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-03-29/07.png?raw=true">

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-03-29/08.png?raw=true">
