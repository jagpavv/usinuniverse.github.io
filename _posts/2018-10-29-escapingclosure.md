---
title: escaping closure
layout: post
---

# escaping closure

### escaping closure를 사용하는 이유(내가)

* 자세한건 검색하면 많이 나와서 한줄로 요약하자면 콜백 구현을 위해서이다.
* 왜 콜백을 구현하느냐 하는 것은 예제를 보면서 알아보자.

### 예제 1

* 가장 많이 사용하는 Alamofire를 통해서 이미지를 받아온다고 가정하자.

```swift
func setImage(url: String) -> UIImage {
    var result = UIImage()
    
    Alamofire.request(url).responseImage { (response) in
        if let image = response.result.value {
            result = image
        }
    }
    
    return result
}
```

* 가장 흔한 구조인데, 이와 같이 실행하면 반환값은 `UIImage()`다.
* 그 이유는 비동기 방식, 즉 코드의 실행 완료를 기다리지 않고 다음 라인으로 진행하기 때문이다.
* 즉 Alamofire의 이미지 다운로드를 기다리지 않고, 실행만 시켜두고 `return` 라인으로 이미 진입하기 때문에 발생하는 결과다.
* 이제 이 문제를 해결해보자.

### 예제 2

```swift
func setImage(url: String, completionHandler: @escaping (_ isSuccess: Bool, _ image: UIImage) -> Void) {
    Alamofire.request(url).responseImage { (response) in
        if let image = response.result.value {
            completionHandler(true, image)
        } else {
            // do something
        }
    }
}
```

* 예제 1과 크게 다르지 않지만 `escaping closure`를 활용해서 이미지 다운로드 구문이 종료되면 closure를 실행하게 해서 실행 순서를 보장할 수 있다.
* 이제 다음과 같이 실행할 수 있다.

```swift
...
setImage(url: url) { (isSuccess, image) in
    if isSuccess {
        // do something
    }
}
...
```

