---
title: Alamofire 사용법
layout: post
---

### 들어가기에 앞서 REST란?

* REpresentational State Transfer의 약자로 소프트웨어 아키텍쳐로 쉽게 말하면 서버와 클라이언트 사이의 통신 방식이다.
* REST는 HTTP와 JSON을 함께 사용하여 Open API를 구현하는 방법으로 사용되곤 한다.
* 대부분의 Open API는 REST 아키텍쳐로 설계 및 구현되고 있고, 이런 원리를 따르는 시스템을 RESTFul 이란 용어로 부른다.
* REST에서의 CRUD는 다음과 같다.
* GET(Request), POST(Create), PUT(update), DELETE(Delete)

### GET

```swift
let url = "https://testurl.com"

Alamofire.request(url).responseJSON { (response) in
            switch response.result {
            case .success:
                print("success")
            case .failure:
                print("failure")
            }
        }
```

### POST

* GET을 사용하지 않는 경우에는 method를 명시해야 한다. (default 값은 GET 이다.)

```swift
let url = "https://testurl.com"

Alamofire.request(url, method: .post).responseJSON { (response) in
            switch response.result {
            case .success:
                print("success")
            case .failure:
                print("failure")
            }
        }
```

* Parameter가 필요한 경우 다음처럼 추가할 수 있다.

```swift
let url = "https://testurl.com"
        
let param = ["id": "asdf", "email": "asdf@asdf.com"]

Alamofire.request(url, method: .post, parameters: param, encoding: JSONEncoding.default).responseJSON { (response) in
            switch response.result {
            case .success:
                print("success")
            case .failure:
                print("failure")
            }
        }
```

* 나머지도 이런 방식으로 접근할 수 있다.

