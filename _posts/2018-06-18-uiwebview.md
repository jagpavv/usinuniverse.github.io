---
title: UIWebView 사용법
layout: post
---

# UIWebView 사용법

* 기존에 WKWebView와 UIWebView를 아주 간단히 비교한 [포스팅](https://github.com/usinuniverse/usinuniverse.github.io/blob/master/_posts/2018-05-25-wkwebview.md) 참고
* 현재 개발하고 있는 프로젝트가 반은 하이브리드고 반은 네이티브인 앱이다.
* 그래서 웹 기반 View를 사용해야 하는데 WKWebView의 경우 로컬 파일을 CORS 정책 때문에 제대로 읽어오지 못한다.
* 성능차이가 있지만, 그렇게 무거운 앱은 아니기에 어쩔 수 없이 UIWebView로 구현해야 했다.
* 기본적으로 UIWebView가 WKWebView로 발전했다고 볼 수 있기 때문에 사용법이 아주 유사하다.
* 바로 코드로 들어가본다.

### 코드

```swift
import UIKit



class ViewController: UIViewController {
    // MARK: - Properties
    var webView: UIWebView?
    
    // MARK: - Methods
    // MARK: View Life Cycle
    
    override func loadView() {
        super.loadView()
        
        self.webView = UIWebView(frame: super.view.frame)
        self.view = self.webView
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let urlPath = Bundle.main.url(forResource: "test", withExtension: "html", subdirectory: "local data")
        guard let url = urlPath else { return }
        let urlRequest = URLRequest(url: url)
        self.webView?.loadRequest(urlRequest)
    }
    
    // MARK: Memory Management
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
}



```

### 실행 결과

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-18/03.png?raw=true">

---

### UIWebViewDelegate

* WKWebView와 마찬가지로 UIWebViewDelegate를 활용해서 html의 로드 시작시점, 완료시점 등 원하는 작업을 할 수 있다.

### 코드

```swift
import UIKit



class ViewController: UIViewController {
    .
    .
    .
    override func loadView() {
        super.loadView()
        
        self.webView = UIWebView(frame: super.view.frame)
        self.webView?.delegate = self // delegate 지정
        self.view = self.webView
    }
    .
    .
    .
}

// MARK: - Extension

extension ViewController: UIWebViewDelegate {
    
    func webViewDidFinishLoad(_ webView: UIWebView) {
        self.webView?.stringByEvaluatingJavaScript(from: "redHeader()")
    }
    
}



```

### 실행 결과

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-18/04.png?raw=true">

---

### 네이티브에서 자바스크립트 호출 방법

* 네이티브에서 자바스크립트를 호출하는 방법은 UIWebVIew나 WKWebView나 아주 비슷하다.
* WKWebView에서는 evaluateJavaScript 메서드를 사용.
* UIWebVIew에서는 stringByEvaluatingJavaScript 메서드를 사용.

### 자바스크립트에서 네이티브 호출 방법

* 자바스크립트에서 WKWebView를 호출 하는 방법은 이전 [포스팅](https://github.com/usinuniverse/usinuniverse.github.io/blob/master/_posts/2018-05-25-wkwebview.md) 참고.
* 자바스크립트에서 UIWebView를 호출하기 위해서는 우선 url scheme을 등록해야 한다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-18/05.png?raw=true">

* 그 후 앞서 등록한 url scheme을 작성하고 뒤에 메시지를 보낸다.
* 이 방식으로 메시지 혹은 객체(제이쿼리 사용) 등을 보낼 수 있다. (지금은 단순 메시지)

```javascript
document.location = "myAppName://doSomething"
```

### 네이티브 호출에 반응하기

* UIWebView에 자바스크립트 호출에 반응하는 메서드가 있긴 한데 좀 더 정확히 알아보면 정확한 의미에서의 반응은 아니다. 정확히는 페이지 로드와 관련이 있어서 자바스크립트에서 호출하는 경우에 응답하지 못하는 경우도 발생한다.
* 따라서 UIWebView의 경우 자바스크립트로부터의 호출에 완벽히 반응하기 위해 AppDelegate에서 다음과 같은 함수를 통해 Notification을 보내 반응식으로 해야한다.
* 지금은 일단 Notification은 생략.


```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
        if url.host == "doSomething" {
            print("do something")
        }
        
        return true
}
```

### 실행 결과

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-18/06.png?raw=true">

### 제이쿼리 받는 방법

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
        var queryData: [String: String]?
        
        if let query = url.query {
            let queryArray = query.split { $0 == "&"}.map(String.init)
            var paramDict: [String: String] = [:]
            for queryParameter in queryArray {
                let keyValueArray = queryParameter.split{$0 == "="}.map(String.init)
                if let key = keyValueArray.first {
                    if let value = keyValueArray.last {
                        if let decodingValue = value.removingPercentEncoding {
                            paramDict.updateValue(decodingValue, forKey: key)
                        }
                    }
                }
            }
            queryData = paramDict
}
```

---

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
        document.location = "myAppName://doSomething";
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

### 추가로 알아야 할 내용

* 기본적으로 WKWebView나 UIWebView는 자바스크립트간의 통신이 비동기적이기 때문에 안드로이드처럼 바로 자바스크립트에서 값을 바로 리턴받는 식으로 동작할 수 없다.
* 이 때문에 자바스크립트가 안드로이드에 맞춰져 있다면 리턴받는 모든 함수를 새로 만들어야 한다. (지금 이 노가다 하고 있음 ㅡ.ㅡ...)
* 또 아주 불편한게 동기방식도 아닌데다가 자바스크립트가 url scheme을 짧은 시간 내에 여러번 보낸다면 마지막 보낸 url scheme만 인식함...
* 그래서 이 마저도 여러번 보내는 url scheme이 있다면 구간마다 끊어 네이티브에서 각각 반응하도록 해줘야지만 모든 url scheme에 대응할 수 있음...정말 귀찮...

### 또 추가로 알아야 할 내용

* 로컬 파일을 읽을 때 지금과 같이 아주 간단한 html인 경우에는 상관 없지만...

```html
<script type="text/javascript" src="lib/js/bip39.min.js"></script>
                    <script type="text/javascript" src="lib/js/value.js"></script>
                    <script type="text/javascript" src="lib/js/third/jquery-3.2.1.min.js"></script>
                    <script type="text/javascript" src="lib/js/create-backup-phrase.js"></script>
                    <script type="text/javascript" src="lib/js/third/jquery-i18next.min.js"></script>
                    <script type="text/javascript" src="lib/js/third/i18nextXHRBackend.min.js"></script>
                    <script type="text/javascript" src="lib/js/third/i18nextBrowserLanguageDetector.min.js"></script>
                    <script type="text/javascript" src="lib/js/i8next.min.js"></script>
                    <script type="text/javascript" src="lib/js/webviewIF.js"></script>
```

* 이렇게 수많은 내부 경로가 있을 경우에는 꼭 꼭 로컬폴더를 복사 붙여넣기 할때 이렇게 해야만 경로를 제대로 인식해서 정상 작동한다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-18/01.png?raw=true">

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-06-18/02.png?raw=true">
