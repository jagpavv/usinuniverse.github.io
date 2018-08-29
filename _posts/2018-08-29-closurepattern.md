---
tite: Delegate pattern vs Closure pattern
layout: post
---

# Delegate pattern vs Closure pattern

### 참고사항

* [Delegate 패턴으로 값 전달하기](https://github.com/usinuniverse/usinuniverse.github.io/blob/master/_posts/2018-03-27-delegatepattern.md)
* [Delegate vs Notification](https://github.com/usinuniverse/usinuniverse.github.io/blob/master/_posts/2017-11-07-delegatevsnotification.md)

### Delegate pattern

```swift
protocol NetworkManagerDelegate {
    func didComplete(result: Bool)
}

class NetworkManager {
    var delegate: NetworkManagerDelegate?
    
    func fetchDataFrom(url: String) {
        doSomething.request(url) { (result) in
            self.delegate?.didComplete(result: result)
        }
    }
}

class ViewController: UIViewController, NetworkManagerDelegate {
    // MARK: - Properties
    // MARK: Custom Variable
    let networkManager = NetworkManager()
    
    
    
    // MARK: - Methods
    // MARK: View Life Cycle
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        networkManager.delegate = self
        networkManager.fetchDataFrom(url: "google.com")
    }
    
    func didComplete(result: Bool) {
        if result {
            // do something
        } else {
            // do something
        }
    }
    
}
```

* 위 경우에 단점을 들자면
  * 강한 참조 발생(`weak var delegate: NetworkManagerDelegate?`, `protocol NetworkManagerDelegate: class {}`로 해결 가능)
  * 옵셔널의 경우 더티 코드(`@objc`)
  * 구조체 전달 불가
  
* 강제적으로 요구사항을 준수하는 것이 편한 경우도 있으나 어쨌든 위의 경우에는 Protocol과 Delegate Sender Class를 만들어야 한다.
* 이제 Closure pattern으로 바꿔보자.

### Closure pattern

```swift
class NetworkManager {
    func fetchDataFrom(url: String, completionHandelr: @escaping (Bool) -> Void) {
        doSomething.request(url) { result in
            completionHandelr(result)
        }
    }
}

class ViewController: UIViewController {
    // MARK: - Properties
    // MARK: Custom Variable
    let networkManager = NetworkManager()
    
    
    
    // MARK: - Methods
    // MARK: View Life Cycle
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        networkManager.fetchDataFrom(url: "google.com") { (result) in
            if result {
                // do something
            } else {
                // do something
            }
        }
    }
    
}
```

* 위 코드를 보면 Delegate pattern과 확연히 다른점을 알 수 있다.
  * `networkManager.delegate = self` 같은 코드를 안써도 된다.
  * `@objc`를 덕지덕지 붙이지 않아도 쓰고 싶은 것만 골라서 쓸 수 있다.
* 때문에 훨씬 더 간결한 표현이 가능하다.
