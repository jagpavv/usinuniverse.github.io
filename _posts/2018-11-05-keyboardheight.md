---
title: 키보드가 나타나지 않고 키보드 높이만 얻는 방법
layout: post
---

# 키보드가 나타나지 않고 키보드 높이만 얻는 방법

### 필요 이유

* 보통 커스텀 키보드를 만드는 경우에 `UITextField`의 `inputView`를 새롭게 정의해 만든다.
* 이때 `inputView`를 대체할 새로운 `view`를 만들때 사용자마다의 각각 다른 키보드 높이에 대응해야 UX적으로 좋다.
* 그렇기 때문에 커스텀 키보드가 올라오기전 미리 키보드의 높이를 알고 있어야만 한다.
* 카카오톡의 경우 이모티콘 버튼을 누르면 사용자의 키보드와 동일한 높이의 `inputView`가 나타난다.
* 사용자가 한 번 이라도 키보드를 사용했더라면 여기서 키보드 높이를 얻을 수 있지만, 한 번도 사용하지 않고 바로 이모티콘 버튼을 눌렀다 가정해보자.
* 여기서 `inputView`는 어떻게 사용자 기기의 키보드 높이를 얻을 수 있을까?
* 이 경우에 방법은 아래와 같다.

```swift
import UIKit



class ViewController: UIViewController {
    // MARK: - Properties
    // MARK: Custom
    
    var keyboardHeight: CGFloat?
    
    // MARK: IBOutlet
    
    @IBOutlet weak var textField1: UITextField!
    @IBOutlet weak var textField2: UITextField!
    
    
    
    // MARK: - Methods
    // MARK: View Life Cycle
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        NotificationCenter.default.addObserver(self, selector: #selector(keyboardWillShow(notification:)), name: UIResponder.keyboardWillShowNotification, object: nil)
    }
    
    // MARK: Custom
    
    @objc func keyboardWillShow(notification: NSNotification) {
        if let keyboardHeight = (notification.userInfo?[UIResponder.keyboardFrameEndUserInfoKey] as? NSValue)?.cgRectValue.height {
            self.keyboardHeight = keyboardHeight
            print(self.keyboardHeight)
        }
    }
    
    // MARK: IBAction
    
    @IBAction func didTapButtn(_ sender: UIButton) {
        if let view = self.textField2.viewWithTag(99) {
            view.removeFromSuperview()
        }
        
        let textField = UITextField()
        self.view.addSubview(textField)
        textField.becomeFirstResponder() // 이 시점에 self.keyboardHeight에 값이 할당.
        textField.resignFirstResponder()
        textField.removeFromSuperview()
        
        guard let keyboardHeight = self.keyboardHeight else { return }
        
        let view = UIView(frame: CGRect(x: 0, y: 0, width: self.view.frame.width, height: keyboardHeight))
        view.backgroundColor = UIColor.blue
        view.tag = 99
        self.textField2.inputView = view
        self.textField2.becomeFirstResponder()
    }
    
}
```
