---
title: 문자열 인코딩
layout: post
---

### 문자열 변환이 필요한 이유

* 일반적인 경우 문자열에는 어떤 글자가 들어가도 상관이 없지만, 이런 문자열을 바탕으로 URL로 변환시키는 과정에서는 한글이 들어가거나 기타 다른 문자가 들어갔을 때 오류가 나타난다.
* 다음의 경우는 '종로구' 라는 한글로 인해 URL이 nil이 되는 경우이다

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/17-10-24/01.png?raw=true">

* 클릭하면 크게 볼 수 있습니다.

---

* 이런 경우 addingPercentEncoding(withAllowedCharacters: <CharacterSet>)를 통해서 문자열을 변환할 수 있다.
* 함수의 인자값으로 오는 <CharacterSet>에는 다양한 옵션이 있으며 옵션에 따라 변환되는 문자열의 결과물이 달라진다.
* 실행결과는 다음과 같다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/17-10-24/02.png?raw=true">

* 클릭하면 크게 볼 수 있습니다.
