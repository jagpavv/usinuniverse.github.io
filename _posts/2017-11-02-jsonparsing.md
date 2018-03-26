---
title: JSON 파싱
layout: post
---

### JSON 이란?
* JSON은 JavaScript Object Notation의 약자로 데이터 포맷의 한 종류이다.
* JSON은 이름에서 알 수 있듯 JavaScript에서 파생되었지만 언어 독립형 포맷이라 다른 프로그래밍 언어에서도 자유롭게 쓸 수 있다.

### JSON의 구조
* JSON은 Number, String, Boolean, Array, Dictionary, null 로 구성되어 있다.
* JSON Dictionary는 "{, }" 중괄호를 이용해 표현한다. (ex. {"name": "usinuniverse"})
* JSON Dictionary의 키 값은 늘 String 타입이어야 한다.
* 또 JSON Dictionary는 JSON Object 라고도 불린다.
* JSON Array는 "[, ]" 대괄호를 이용해 표현한다. (ex. ["name": {"firstname": "usin", "lastname": "universe"}])
* JSON의 구조는 Dictionary 안에 Array가 들어갈 수 있고 반대의 경우도 마찬가지이다. 그래서 종종 가독성이 떨어지기도 하지만 구조를 잘 이해하고 있다면 쉽게 파악할 수 있다.

### Alamofire + SwiftyJSON을 이용하여 JSON 쉽게 Parsing 하기
* 우선 Parsing 할 JSON 데이터는 https://api.randomuser.me 에서 받아온다. (JSON 데이터를 랜덤으로 만들어준다.)
* 받아온 데이터를 확인해보자

```json
{
  "results" : [
    {
      "picture" : {
        "medium" : "https://randomuser.me/api/portraits/med/women/56.jpg",
        "thumbnail" : "https://randomuser.me/api/portraits/thumb/women/56.jpg",
        "large" : "https://randomuser.me/api/portraits/women/56.jpg"
      },
      "nat" : "US",
      "login" : {
        "sha256" : "f9806d13f851697ea5a60ba8c384aacb7dea10d2914cf885d965e6c0d79032ff",
        "sha1" : "b7ed3db8c21b86497789c084b1b35d401bf1b236",
        "md5" : "b3c3a8449cc85e8d310036efe9b1067f",
        "salt" : "XRF4InTe",
        "username" : "blackladybug452",
        "password" : "speedy"
      },
      "location" : {
        "state" : "louisiana",
        "street" : "1985 fincher rd",
        "city" : "long beach",
        "postcode" : 50018
      },
      "id" : {
        "name" : "SSN",
        "value" : "410-66-7077"
      },
      "email" : "gloria.fields@example.com",
      "name" : {
        "last" : "fields",
        "title" : "mrs",
        "first" : "gloria"
      },
      "phone" : "(058)-938-5135",
      "registered" : "2010-05-18 04:55:56",
      "gender" : "female",
      "cell" : "(642)-213-0386",
      "dob" : "1969-02-03 21:38:08"
    }
  ],
  "info" : {
    "version" : "1.1",
    "seed" : "6fee59433f4076cd",
    "page" : 1,
    "results" : 1
  }
}
```

* 이 JSON 데이터에서 username을 받아오고자 한다.
* 이때의 접근 방법은 다음과 같다.

```swift
let url = "https://api.randomuser.me"

Alamofire.request(url).responseJSON { (response) in
            if response.result.value != nil {
                let jsonData = JSON(response.result.value)
                let userName = jsonData["results"][0]["login"]["username"].stringValue
                
                print(userName) // blackladybug452
            }
        }
```

* Alamofire를 통해 받아온 value를 JSON 포맷으로 변환해서 바로 사용할 수 있어 아주 간편한 방법이다.
