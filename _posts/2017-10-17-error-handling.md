---
title: 오류 처리
layout: post
---

### 오류 처리

* 오류 처리란? 오류가 발생했을 때 nil 외에 오류에 대한 자세한 정보를 전달하기 위해 사용하는 구문이다.
* 예를 들어 문자열을 정수로 변경하고자 할 때 실패하면 nil이 반환되지만, 이 실패가 어떤 원인에 의해 실패하게 됐는지 알 수 없으므로 이에 대해 파악할 수 있게 하는 것이 오류 처리 구문이다.
* 오류를 반환할 수 있는 함수는 반환값 지정 이전에 throws를 추가해야 한다.

```swift
import Foundation

enum DateFormatError: Error { //에러 프로토콜 채택
    case overSizeString, underSizeString, incorrectFormat(part: String), incorrectData(part: String)
}

struct Date { //년, 월, 일을 저장하는 Date 객체 정의
    var year: Int
    var month: Int
    var day: Int
}

func parsDate(param: NSString) throws -> Date { //오류를 던질 수 있도록 정의
    guard param.length == 10 else { 
        if param.length > 10 {
            throw DateFormatError.overSizeString //입력된 NSString의 길이가 10자리를 초과할 시 해당 오류 구문을 던진다.
        } else {
            throw DateFormatError.underSizeString //입력된 NSString의 길이가 10자리 미만일 경우 해당 오류 구문을 던진다.
        }
    }
    
    var dateResult: Date = Date(year: 0, month: 0, day: 0) //0년, 0월, 0일을 가지고 있는 dateResult 객체 생성
    
    if let year = Int(param.substring(with: NSRange(location: 0, length: 4))) {
        dateResult.year = year
    } else {
        throw DateFormatError.incorrectFormat(part: "year Error") //정수로 변경 실패시 오류 구문을 던진다.
    }
    
    if let month = Int(param.substring(with: NSRange(location: 5, length: 2))) {
        guard month > 0 && month < 13 else {
            throw DateFormatError.incorrectData(part: "month range Error") //월이 1월 미만, 13월 이상일 경우 오류 구문을 던진다.
        }
        dateResult.month = month
    } else {
        throw DateFormatError.incorrectFormat(part: "month Error") //정수로 변경 실패시 오류 구문을 던진다.
    }
    
    if let day = Int(param.substring(with: NSRange(location: 8, length: 2))) {
        guard day > 0 && day < 32 else {
            throw DateFormatError.incorrectData(part: "day range Error") //일이 1일 미만, 32일 이상일 경우 오류 구문을 던진다.
        }
        dateResult.day = day
    } else {
        throw DateFormatError.incorrectFormat(part: "day Error") //정수로 변경 실패 오류 구문을 던진다.
    }
    
    return dateResult //최종적으로 완성된 dateResult 객체를 반환한다.
}

func getDateResult(date: NSString, type: String) { //오류를 던질 수 있는 함수에 대해서 그 오류를 잡아내기 위한 함수
    do {
        let date = try parsDate(param: date) //오류를 던질 수 있는 함수는 실행에 앞서 try 키워드를 추가해야 한다.
        
        switch type {
        case "year":
            print("\(date.year)년 입니다") //정상입력 결과
        case "month":
            print("\(date.month)월 입니다") //정상입력 결과
        case "day":
            print("\(date.day)일 입니다") //정상입력 결과
        default:
            print("입력 오류")
        }
    } catch DateFormatError.overSizeString {
        print("입력된 문자열이 너무 깁니다")
    } catch DateFormatError.underSizeString {
        print("입력된 문자열이 너무 짧습니다")
    } catch DateFormatError.incorrectFormat(let part) {
        print("\(part) 입력이 잘못됐습니다")
    } catch DateFormatError.incorrectData(let part) {
        print("\(part) 입력이 잘못됐습니다")
    } catch {
        print("알 수 없는 오류 발생")
    }
}

getDateResult(date: "2017-03-30", type: "year") //2017년 입니다
getDateResult(date: "2017-00-30", type: "month") //month range Error 입력이 잘못됐습니다
getDateResult(date: "2017-09-00", type: "day") //day range Error 입력이 잘못됐습니다
```

* 이처럼 오류를 던질 수 있는 함수는 do-catch 구문을 통해 그 오류에 대해 잡아내는 과정이 필요하다.
* 또한, 오류를 던질 수 있는 함수의 실행을 위해선 try 키워드를 추가해야 한다.
