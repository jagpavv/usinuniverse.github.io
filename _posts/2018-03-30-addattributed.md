---
title: 문자열 원하는 영역에 효과주기
layout: post
---

### 문자열 원하는 영역에 효과주기

* 문자열에 줄 수 있는 효과는 볼드, 하이라이트 등 다양한 효과가 있다.
* 해당 효과를 주기 위해선 String이 아닌 AttributedString이어야 한다.
* 이번 예시에서는 배경색을 바꿔 하이라이트 처리를 주려고 한다.

```swift
import UIKit
import Foundation

// 일반 String 초기화
let normalString = "안녕하세요. 여기는 해피코딩 월드입니다."

// Mutable, 즉 수정 가능한 AttributedString 으로 초기화
let attriString = NSMutableAttributedString(string: normalString)

// 해피코딩에 하이라이트 효과를 줄 것이기 때문에, 해당 문장에서 해피코딩이 포함된 NSRange를 얻어야함.
// NSRange는 location 값과 length 값으로 이루어져 있음.
// location 값은 시작점을 뜻하며 0부터 시작한다.
// length 값은 길이를 뜻하며 1부터 시작한다.
let range = (normalString as NSString).range(of: "해피코딩") // normalString 안에서 "해피코딩"이 위치한 NSRange 값을 얻을 수 있다.

attriString.addAttribute(.backgroundColor, value: UIColor.yellow, range: range)
```

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-03-30/01.png?raw=true">

### 반복적인 처리를 하기 위해선

* 위처럼 해당 효과를 단 한번만 적용한다면 상관이 없지만 만약 해당 문장에 처리를 해야할 단어가 여럿 존재한다면 모든 경우에 NSRange 값을 알아야만 한다.
* 그러나 이 경우 기본적으로 제공하는 framework에는 정의된 함수가 없기에 만들어야 한다.
* 해피코딩이 들어간 문자열에 효과를 주고난 다음에는 NSRange의 location값과 length값을 변경해가면서 나머지 부분을 계속 탐색해야 한다.

```swift
import UIKit
import Foundation

// 일반 String 초기화
let normalString = "안녕하세요. 여기는 해피코딩 월드입니다. 안녕하세요. 여기는 해피코딩 월드입니다. 안녕하세요. 여기는 해피코딩 월드입니다."

// Mutable, 즉 수정 가능한 AttributedString 으로 초기화
let attriString = NSMutableAttributedString(string: normalString)

// 문장 전체의 NSRange를 만든다.
var range = NSMakeRange(0, attriString.length)

// 찾고자 하는 단어의 Length를 만든다.
let keyword = "해피코딩"
let keywordLength = keyword.count

// location이 length값과 같거나 작을동안, 즉 문장의 length가 10인 경우 location(시작점)이 10을 초과하는 순간부터는 오류가 발생하기 때문에 그 전까지만 반복하면 된다.
while range.location <= attriString.length {
    range = (attriString.mutableString as NSString).range(of: keyword, options: [], range: range)
    
    if range.location <= attriString.length {
        attriString.addAttribute(.backgroundColor, value: UIColor.yellow, range: range)
        range = NSMakeRange(range.location + keywordLength, attriString.length - (range.location + keywordLength))
    }
}
```

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-03-30/02.png?raw=true">
