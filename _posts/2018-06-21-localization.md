---
title: iOS 다국어 지원(localization)하는 방법
layout: post
---

# 다국어 지원

* 다국어 지원에 대한 필요성 등은 각설하고, 바로 어떻게 하면 다국어 지원 혹은 지역화를 할 수 있는지 알아보자!
* 우선 늘 만들던대로 프로젝트를 생성한다.
* base가 될 언어로 영어 혹은 한국어 아무거나 선택해 해당 언어로 일단 개발을 해둔다. (여기서는 일단 영어로 했음)

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-21/01.png?raw=true">

```swift
import UIKit



class ViewController: UIViewController {
    // MARK: - Properties
    // MARK: IBOutlet
    @IBOutlet weak var greetingLabel: UILabel!
    
    
    
    // MARK: - Methods
    // MARK: View Life Cycle
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }
    
    // MARK: Memory Management
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    // MARK: IBAction
    
    @IBAction func didTapChangeButton(_ sender: UIButton) {
        self.greetingLabel.text = "Hello~!"
    }
    
}



```

---

* 일단은 아주 간단하게 만들었는데, 버튼을 누르면 Label에 해당 국가의 인사말로 바꾸게 하고 싶다.
* 어떻게 하면 될까?

1. 

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-21/02.png?raw=true">

2.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-21/03.png?raw=true">

3.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-21/04.png?raw=true">

4.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-21/05.png?raw=true">

* 여기에 다국어 지원을 위한 파일이 새로 생긴다.
* 딱 보면 알겠지만 스토리 보드에서 만든 Label과 Button의 원본(영어) 문자열이 보인다.
* 이걸 이렇게 수정해보자.

5.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-21/06.png?raw=true">

6.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-21/07.png?raw=true">

* 짜잔~!
* 영어로 만들었는데 알아서 한글로 나온다!
* 버튼도 눌러볼까?

7.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-21/08.png?raw=true">

* 안타깝게도 이 방법은 스토리 보드에서 만든 것만 지원한다.
* 그럼 코드로 쓴 것은 어떻게 할까?

8.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-21/09.png?raw=true">

* 우선 Strings File을 새로 만든다.

9.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-21/10.png?raw=true">

* 그 후 Localization을 눌러 지원하고 싶은 언어(한국어)를 선택한다.

10.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-21/11.png?raw=true">

* 그리고 바꾸고 싶은 영어 원문(key)를 쓰고 한국어(value)를 입력하고, 다음과 같이 코드를 수정하면 된다.

```swift
@IBAction func didTapChangeButton(_ sender: UIButton) {
        self.greetingLabel.text = NSLocalizedString("Hello~!", comment: "인사말")
}
```
* Localizable.strings에서 key-value 형식으로 지정되기 때문에 key에 해당하는 "Hello~!"를 써주고, comment는 작동과는 상관없고 그저 개발자들이 보기 편하게 주석처럼 남길 수 있도록 한다.

### 결과

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-21/12.png?raw=true">
