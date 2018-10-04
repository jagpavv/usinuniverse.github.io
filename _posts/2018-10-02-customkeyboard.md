---
title: 커스텀 키보드 만들기(iOS)
layout: post
---

# 커스텀 키보드 만들기(iOS)

### 목표

* 카카오톡에서 이모티콘을 둘러보고 선택하는 키보드를 만들 예정이다.
* 전체적인 과정은 UIView를 Custom Keyboard로 만들고(xib), UITextField의 inputView를 Custom Keyboard로 변경하는 방식이다.
* ❗️[커스텀 뷰 만드는 방법](https://github.com/usinuniverse/usinuniverse.github.io/blob/master/_posts/2018-03-29-customview.md)

### 1. Custom Keyboard 만들기

1. 뼈대 만들기

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-10-02/01.png?raw=true">

2. CustomKeyboard.swift

```swift
import UIKit



protocol CustomKeyboardDelegate {
    func didTapTap(index: Int) -> Void
    func didTapEmoticon(index: Int) -> Void
}

class CustomKeyboard: UIView {
    // MARK: - Properties
    // MARK: Custom
    
    var delegate: CustomKeyboardDelegate?
    
    // MARK: IBOutlet
    
    @IBOutlet var mainView: UIView!
    @IBOutlet weak var emoticonScrollView: UIScrollView!
    @IBOutlet weak var tabScrollView: UIScrollView!
    
    
    
    // MARK: - Methods
    // MARK: Life Cycle
    
    required init?(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)
        
        self.customKeyboardInit()
    }
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        
        self.customKeyboardInit()
    }
    
    func customKeyboardInit() {
        Bundle.main.loadNibNamed("CustomKeyboard", owner: self, options: nil)
        self.addSubview(mainView)
        self.mainView.frame = self.bounds
        self.mainView.autoresizingMask = [.flexibleWidth, .flexibleHeight]
    }
    
    // MARK: Custom
    // Private
    private func configureOriginPoint(to count: Int, isXOrigin: Bool) -> Int {
        if isXOrigin {
            return count % 3
        } else {
            return count / 3
        }
    }
    
    private func configureHeight(to count: Int) -> CGFloat {
        let count = CGFloat(count) / 3.0
        let result = count.rounded(.awayFromZero)
        return result
    }
    
    // Objc
    @objc func didTapTab(_ gesture: UITapGestureRecognizer) {
        guard let index = gesture.view?.tag else { return }
        self.delegate?.didTapTap(index: index)
    }
    
    @objc func didTapEmoticon(_ gesture: UITapGestureRecognizer) {
        guard let emoticonView = gesture.view as? UIImageView else { return }
        let index = emoticonView.tag
        self.delegate?.didTapEmoticon(index: index)
    }
    
    // Internal
    func configureTabItem(to amount: Int) {
        self.tabScrollView.contentSize = CGSize(width: 10 + (CGFloat(amount) * 28) + (CGFloat(amount) * 10),
                                                height: self.tabScrollView.frame.size.height)
        
        for i in 0..<amount {
            let imageView = UIImageView()
            imageView.frame.size = CGSize(width: 28.0, height: 28.0)
            imageView.frame.origin = CGPoint(x: 10 + (CGFloat(i) * 28) + (CGFloat(i) * 10), y: 8.0)
            imageView.backgroundColor = UIColor.blue
            imageView.tag = i
            imageView.isUserInteractionEnabled = true
            
            let tapGesture = UITapGestureRecognizer(target: self, action: #selector(self.didTapTab))
            imageView.addGestureRecognizer(tapGesture)
            
            self.tabScrollView.addSubview(imageView)
        }
    }
    
    func configureEmoticon(to amount: Int) {
        for subview in self.emoticonScrollView.subviews {
            subview.removeFromSuperview()
        }
        
        let emoticonSize = CGSize(width: ((self.bounds.size.width - 40) / 3), height: ((self.bounds.size.width - 40) / 3))
        self.emoticonScrollView.contentSize = CGSize(width: self.frame.width,
                                                     height: 10 + (emoticonSize.width * self.configureHeight(to: amount)) + (10 * CGFloat(amount) / 3))
        
        for i in 0..<amount {
            let emoticon = UIImageView()
            emoticon.frame.size = emoticonSize
            emoticon.frame.origin.x = 10 + (CGFloat(self.configureOriginPoint(to: i, isXOrigin: true)) * emoticon.bounds.size.width + (CGFloat(self.configureOriginPoint(to: i, isXOrigin: true)) * 10))
            emoticon.frame.origin.y = 10 + (CGFloat(self.configureOriginPoint(to: i, isXOrigin: false)) * emoticon.bounds.size.width + (CGFloat(self.configureOriginPoint(to: i, isXOrigin: false)) * 10))
            emoticon.backgroundColor = UIColor.black
            emoticon.tag = i
            emoticon.isUserInteractionEnabled = true
            
            let tapGesture = UITapGestureRecognizer(target: self, action: #selector(self.didTapEmoticon(_:)))
            emoticon.addGestureRecognizer(tapGesture)
            
            self.emoticonScrollView.addSubview(emoticon)
        }
        
        self.emoticonScrollView.setContentOffset(.zero, animated: true)
    }
    
}
```

### 2. Custom Keyboard 적용하기

```swift
import UIKit



class ViewController: UIViewController {
    // MARK: - Properties
    // MARK: Custom
    
    var customKeyboard: CustomKeyboard?
    var keyboardHeight: CGFloat?
    
    // MARK: IBOutlet
    
    @IBOutlet weak var textField: UITextField!
    @IBOutlet weak var fakeTextField: UITextField!
    
    
    
    // MARK: - Methods
    // MARK: View Life Cycle
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        NotificationCenter.default.addObserver(self, selector: #selector(keyboardWillShow(notification:)), name: UIResponder.keyboardWillShowNotification, object: nil)
    }
    
    // MARK: Custom
    // Objc
    @objc func keyboardWillShow(notification: NSNotification) {
        if let keyboardHeight = (notification.userInfo?[UIResponder.keyboardFrameEndUserInfoKey] as? NSValue)?.cgRectValue.height {
            self.keyboardHeight = keyboardHeight
        }
    }
    
    // MARK: IBAction
    
    @IBAction func didTapEmoticonButton(_ sender: UIButton) {
        if self.customKeyboard == nil {
            self.customKeyboard = CustomKeyboard(frame: CGRect(x: 0, y: 0, width: self.view.frame.width, height: self.keyboardHeight ?? 200))
            self.customKeyboard?.delegate = self
            
            self.customKeyboard?.configureTabItem(to: 12)
            self.customKeyboard?.configureEmoticon(to: 10)
        }
        
        self.fakeTextField.inputView = self.customKeyboard
        self.fakeTextField.becomeFirstResponder()
    }
    
}

// MARK: - Extension
// MARK: Custom Keyboard Delegate

extension ViewController: CustomKeyboardDelegate {
    
    func didTapTap(index: Int) {
        print(index)
    }
    
    func didTapEmoticon(index: Int) {
        print(index)
    }
    
}
```

### 3. 결과

[![CustomKeyboard](https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-10-02/02.png?raw=true)](https://vimeo.com/292847438)

* 클릭하시면 동영상을 감상하실 수 있습니다.






