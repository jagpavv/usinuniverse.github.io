---
title: XML 파싱
layout: post
---

### XML

* XML이란? eXtensible Markup Language의 약자로 인터넷을 통해 데이터를 쉽게 주고받을 수 있도록하여 HTML의 한계를 극복할 목적으로 만들어진 마크업 언어이다.
* XML은 태그로 구성되어 있으며 시작 태그와 끝 태그가 서로 짝을 이루고 있다.
* 시작 태그와 끝 태그 사이에는 마크업이 아닌 문자열로 내용이 작성될 수 있으며 하위 계층을 이루는 태그 쌍이 포함될 수 있어 이를 통해 계층 구조를 만들 수 있다.
* XML 예시

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<response>
	<header>
		<resultCode>00</resultCode>
		<resultMsg>NORMAL SERVICE.</resultMsg>
	</header>
	<body>
		<items>
			<item>
				<contents>20,000원 | 462페이지 | 소프트커버    한국영상자료원 원로영화인 구술사 프로젝트의 두 번째 성과물. 감독 김기덕, 이강원, 배우 이경희, 이민, 윤인자, 촬영기사 서정민, 전조명, 조명기사 박창호, 평론가 임영, 시나리오 작가 한우정 등 원로영화인 10인의 풍부하고 심도 깊은 구술 증언을 통해 1950년대 후반에 맞이한 ‘한국영화의 르네상스’를 조명했다. &lt;맨발의 청춘&gt;으로 청춘영화 붐을 선도한 김기덕, 1950년대 최고의 문제작 &lt;자유부인&gt;의 이민, 한국영화 최초의 키스 신으로 유명한 &lt;운명의 손&gt;의 배우 윤인자, 지금도 현역 촬영기사로 활동하고 있는 서정민과 전조명, 이만희 감독과 함께한 시나리오 작가 한우정 등의 흥미로운 증언이 그들의 개인 앨범에서 직접 골라준 생생한 현장 사진과 함께 실려 있다.   &lt;font color=888888&gt;  저자 :  정종화 (한국영상자료원 연구교육팀 연구원)  공영민 (중앙대 첨단영상대학원 영상예술학과 석사 과정)  박진호 (경성대 강사, 중앙대 첨단영상대학원 영상예술학과 박사 과정)  배수경 (경원대 강사, 중앙대 첨단영상대학원 영상예술학과 석사)  심혜경 (중앙대 강사, 중앙대 첨단영상대학원 영상예술학과 박사 과정)</contents>
				<imageurl>http://admin.koreafilm.or.kr/kofa_admin/data/book/public_08.gif</imageurl>
				<pubclass>한국영화사 구술총서</pubclass>
				<pubclassseq></pubclassseq>
				<pubform>도서</pubform>
				<publisher>한국영상자료원 엮음 | 이채</publisher>
				<pubtitle>한국영화를 말한다 : 한국영화의 르네상스 1</pubtitle>
				<pubyear>2005년</pubyear>
				<textlanguage>KOR</textlanguage>
			</item>
		</items>
		<numOfRows>10</numOfRows>
		<pageNo>1</pageNo>
		<totalCount>209</totalCount>
	</body>
</response>
```


### XML Parsing

* XML Parsing이란? XML 형식의 데이터는 그대로 사용하는 것이 아니라 데이터를 형식에 맞게 분석하는 과정이 필요하다. 이 분석과정이 바로 Parsing이다.
* 이 분석을 처리하는 모듈을 Parser라고 한다.
* iOS에서는 파운데이션 프레임워크를 통해 XMLParser 를 사용할 수 있다.


### XML Parsing 과정

* 아래의 과정은 XML 형식의 오픈소스 http://api.koreafilm.or.kr/openapi-data2/service/api105/getOpenDataList 에서 publisher 정보만을 가져와 테이블뷰에 나타내는 과정을 나타냈다.

```swift
import UIKit

class TestTableViewController: UITableViewController, XMLParserDelegate {
    
    var currentElement = ""             // 요소가 저장될 변수
    var data = [String: String]()       // 오픈소스 데이터
    var datas = [[String: String]]()    // 오픈소스 데이터를 담는 딕셔너리
    var author = ""                     // publisher 가 담길 변수
    
    
    
    func requestInfo() {
        let url = "http://api.koreafilm.or.kr/openapi-data2/service/api105/getOpenDataList"     // 오픈소스 주소
        guard let xmlParser = XMLParser(contentsOf: URL(string: url)!) else { return }
        xmlParser.delegate = self
        xmlParser.parse()
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        requestInfo()
    }
    
    // 해당 메소드는 시작 태그를 만나면 실행
    func parser(_ parser: XMLParser, didStartElement elementName: String, namespaceURI: String?, qualifiedName qName: String?, attributes attributeDict: [String : String] = [:]) {
        currentElement = elementName
        if currentElement == "item" {
            data = [String: String]()
            author = ""
        }
    }
    
    // 현재 태그에 담긴 문자열을 author 변수에 저장
    func parser(_ parser: XMLParser, foundCharacters string: String) {
        if currentElement == "publisher" {
            author = string
        }
    }
    
    // 해당 메소드는 끝 태그를 만나면 실행
    func parser(_ parser: XMLParser, didEndElement elementName: String, namespaceURI: String?, qualifiedName qName: String?) {
        if elementName == "item" {
            data["author"] = author
            datas.append(data)
        }
    }
    
    // 테이블뷰의 로우 생성
    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return datas.count
    }
    
    // 테이블뷰의 셀 내용 정의
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath)
        cell.textLabel?.text = datas[indexPath.row]["author"]
        return cell
    }
}
```

* 실행결과는 다음과 같다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/17-10-21/01.png?raw=true" width="40%">

* 간혹 다음과 같은 오류가 뜨는 경우가 있다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/17-10-21/02.png?raw=true">

* 위의 오류는 SSL 보안 프로토콜이 적용되어 있지않아 ATS 보안 설정없이 접속하게 되면 정상적인 통신이 불가능하여 생기는 오류이다.
* 따라서 ATS 설정을 추가해야 서버에서 데이터를 읽어올 수 있다.
* 다음은 ATS 설정하는 방법이다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/17-10-21/03.png?raw=true">

* 위처럼 info.plist에서 App Transport Security Settings -> Allow Arbitrary Loads -> YES 설정을 하면 정상적으로 접근할 수 있다.
