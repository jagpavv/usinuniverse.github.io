---
title: WKWebView 사용법
layout: post
---

# WKWebView 사용법

### WKWebView vs UIWebView

* UIWebView는 WKWebView가 나오기 전에 쓰이던 거라 좀 느리고 성능이 떨어진다.
* WKWebView가 UIWebView에 비해 성능적인 면에서는 뛰어나지만 몇 가지 꼭 알아야 할 차이점이 있다.
* UIWebView는 프로세스 내에서 실행되지만 WKWebView는 프로세스 밖에서 실행되어 앱과 별도의 메모리에 스레드된다.
* WKWebView와 JavaScript 간의 통신은 비동기적으로 처리된다.
* WKWebView는 데이터를 쿠키에 저장하지 않으므로 항상 로딩 시간이 동일하다.
* 등등 찾아보면 많은 정보가 있지만, 이번에는 구체적으로 iOS Native와 JavaScript 간의 통신에 대해 다뤄보려 한다.

### 사전 준비

### 스토리보드

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-05-25/01.png?raw=true">

* 스토리보드에서 WKWebView가 담길 Container View를 만든다.
* Container View를 굳이 만드는 이유는 코드로 WKWebView를 만들어야만 Configuration을 적용할 수 있는데, 코드로 만들 경우 오토레이아웃 좀 더 수월하게 하기 위해서다. 그러니 필수는 아니다.
* 버튼을 만들어 Native에서 JavaScript로의 통신이 가능한지 확인할 에정이다.

### 코드

* iOS

```swift
import UIKit
import WebKit



class ViewController: UIViewController, WKUIDelegate, WKNavigationDelegate, WKScriptMessageHandler {
    // MARK: - Properties
    // MARK: Custom
    
    var webView: WKWebView?
    
    // MARK: IBOutlet
    
    @IBOutlet weak var containerView: UIView!
    
    
    
    // MARK: - Methods
    // MARK: View Life Cycle
    
    override func loadView() {
        super.loadView()
        
        // WKUserContentController는 Native에서 JavaScript로 메시지를 보내고 스크립트 삽입을 가능하게 하는 객체다.
        let contentController = WKUserContentController()
        contentController.add(self, name: "callbackHandler") // Message Handler 추가
        
        // WKWebViewConfiguration은 웹 페이지의 렌더링 속도, 미디어 재생 처리 방법 등 다양한 옵션을 결정할 수 있게 하는 객체다.
        // 또한 처음 생성될 때에만 지정할 수 있으며, 이미 만들어진 WKWebView의 Configuration은 get만 가능하다.
        let configuration = WKWebViewConfiguration()
        configuration.userContentController = contentController // User Content Controller 추가
        
        // Native에서 JavaScript로 통신하는 방법은 두 가지가 있는데 그 중에서 첫 번째 방법인 이 방법은 HTML 문서가 시작될 때만 가능하다.
        let userScript = WKUserScript(source: "redHeader()", injectionTime: .atDocumentEnd, forMainFrameOnly: true) // atDocumentEnd는 문서가 로드된 후 하위 리소스가 로드되기전 스크립트를 삽입한다.
        contentController.addUserScript(userScript)
        
        // WKWebView를 미리 지정해둔 Container View의 Bounds에 맞춰 만들고 Configuration을 지정한다.
        self.webView = WKWebView(frame: self.containerView.bounds, configuration: configuration)
        if let webView = self.webView {
            self.containerView.insertSubview(webView, at: 0)
            
            webView.uiDelegate = self // uiDelegate는 웹 페이지 대신 Native User Interface 요소를 나타내는 메서드를 제공한다.
            webView.navigationDelegate = self // navigationDelegate는 WKWebView가 naviation을 하면서 발생하는 이벤트(탐색 요청 수락, 로드, 완료 등)들을 관리할 수 있게 한다.
        }
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let url = URL(fileURLWithPath: "/Users/jaehyeongahn/Desktop/practice.html")
        let myRequest = URLRequest(url: url)
        if let webView = self.webView {
            webView.load(myRequest)
        }
    }
    
    // MARK: Memory Management
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    // MARK: WK Script Message Handler
    // JavaScript -> Native 호출
    func userContentController(_ userContentController: WKUserContentController, didReceive message: WKScriptMessage) {
        if message.name == "callbackHandler" {
            print("JS -> Native 호출, \(message.body)")
        }
    }
    
    // MARK: IBACtion
    // Native -> JavaScript 호출 (두번째 방법)
    @IBAction func didTapchangeButton(_ sender: UIButton) {
        if let webView = self.webView {
            webView.evaluateJavaScript("blueHeader()") { (result, error) in
                if let error = error {
                    print(error)
                } else if let result = result {
                    print(result)
                }
            }
        }
    }
    
}



```

* HTML

```html
<!doctype html>
<html>
<head>

<meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
<script src="/public/js/jquery-3.1.1js"></script>

<script type="text/javascript" charset="UTF-8">

function callNative() {
	try {
		webkit.messageHandlers.callbackHandler.postMessage("MessageBody");
	} catch(err) {
		alert(err);
	}
}

function redHeader() {
	alert('redHeader() CALL');
	document.querySelector('h1').style.color = "red";
}

function blueHeader() {
	alert('blueHeader() CALL');
	document.querySelector('h1').style.color = "blue";
}

</script>

</head>
<body>

<h1>iOS 용 페이지</h1>
<br><input type="button" onclick="callNative()" value="네이티브 함수 호출" />
</body>
</html>
```

---

### 실행 결과

* 처음 실행시 미리 정의해둔 userScript로 인해 타이틀이 빨간색이 된다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-05-25/02.png?raw=true">

* 그리고 Native 버튼을 누르면 색이 바뀐다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-05-25/03.png?raw=true">

* 마지막으로 JavaScript 버튼을 누르면 Native에서 반응한다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-05-25/04.png?raw=true">
