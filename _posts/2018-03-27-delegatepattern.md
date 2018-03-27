---
title: Delegate 패턴으로 값 전달하기
layout: post
---

### 소개

* 뷰 컨트롤러간 값을 전달하는 방법은 다양한데, 그 중에서 오늘은 Delegate 패턴을 이용할 것 이다.
* VC1 에서 VC2로 이동 후, VC2에서 입력한 값을 VC1에서 가져오는 형식으로 진행된다.
* 스토리보드는 다음과 같이 만든다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-03-27/01.png">

### VC1

```swift
import UIKit

class VC1: UIViewController, VC2Delegate {
    // 결과를 보여줄 Label
    @IBOutlet weak var resultLabel: UILabel!
    // VC2에서의 결과값을 저장할 변수
    var result: String = ""
    
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
    
    // VC2로 이동
    @IBAction func moveButtonDidTap(_ sender: UIButton) {
        // VC2 인스턴스 생성
        let vc2 = storyboard?.instantiateViewController(withIdentifier: "VC2") as! VC2
        
        // Delegate 지정
        vc2.delegate = self
        
        // VC2 띄우기
        present(vc2, animated: true, completion: nil)
    }
    
    // 결과 확인
    @IBAction func showResultButtonDidTap(_ sender: UIButton) {
        resultLabel.text = result
    }
    
    //MARK: Delegate Method
    
    func saveResult(textFieldContents: String) {
        // VC2로 부터 받은 값을 result 변수에 대입
        result = textFieldContents
    }
}

```

### VC2

```swift
import UIKit

// 프로토콜 정의
protocol VC2Delegate {
    func saveResult(textFieldContents: String) -> Void
}

class VC2: UIViewController {
    // 입력창
    @IBOutlet weak var inputTextField: UITextField!
    
    // Delegate 선언
    var delegate: VC2Delegate?
    
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
    
    // 확인 버튼
    @IBAction func doneButtonDidTap(_ sender: UIButton) {
        if let contents = self.inputTextField.text {
            
            // delegate method 실행
            delegate?.saveResult(textFieldContents: contents)
            dismiss(animated: true, completion: nil)
        }
    }
}
```

### 결과 확인

* [클릭하시면 구동 영상을 감상할 수 있습니다](https://vimeo.com/261947959)




