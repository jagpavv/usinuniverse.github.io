---
title: 숫자 야구 게임
layout: post
hide: true
---

# 두번째 연습 - 숫자 야구 게임

### UI 구현

[![BaseballGame](https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/project%20images/02/01.png?raw=true)](https://vimeo.com/255879578)

* 클릭하면 구동 영상을 감상할 수 있습니다.

### 게임 룰

* 새게임을 누르고 컴퓨터가 생각한 숫자 3개를 스트라이크, 볼로 유추하여 정답을 맞춘다.

### 기능 구현

* 게임을 처음 시작하는 경우와 게임이 끝난 경우 '새게임'버튼을 눌러야만 한다.
* 중복값을 입력하지 못하게 했다.
* 숫자를 모두 입력하지 않은 경우 결과 확인을 못하게 했다.
* 입력했던 숫자를 지울 수 있게 했다.
* 과거 입력했던 숫자들을 볼 수 있게 했다.

### 어려웠던 점

* 제약조건을 수행하는 로직을 짜는 것이 힘들었다. (ex.입력했던 숫자를 지우거나, 중복값 입력방지, 숫자를 모두 입력해야 게임 진행 가능 등)

### 코드

* ViewController.swift

```swift
import UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

    
    //결과공개
    @IBOutlet weak var resultLB: UILabel!
    //히스토리
    @IBOutlet weak var historyLB: UITextView!
    //첫번째 입력한 숫자
    @IBOutlet weak var firstNumLB: UILabel!
    //두번째 입력한 숫자
    @IBOutlet weak var secondNumLB: UILabel!
    //세번째 입력한 숫자
    @IBOutlet weak var thirdNumLB: UILabel!
    
    //버튼 입력 가능여부 판단용
    var ableBTN: Bool = false
    
    //사용자 입력값 저장
    var userArray: [Int] = []
    
    //컴퓨터의 값 임시저장할 배열
    var computerArray: [Int] = []
    //컴퓨터의 랜덤값 임시저장
    var randomNum: Int = 0
    
    //게임정보
    //스트라이크 갯수
    var strike: Int = 0
    //볼 갯수
    var ball: Int = 0
    //라운드 표시
    var round: Int = 0
    
    //버튼이름 지정
    enum ButtonName: Int {
        case check = 10, delete, reset
    }
    
    //번호 확정 or 번호 리셋
    @IBAction func confirm(_ sender: UIButton) {
        //////////////////////////////////////////////////////옵셔널붙는 이유
        let clickedButton: ButtonName = ButtonName(rawValue: sender.tag)!
        switch clickedButton {
        //결과확인 버튼
            case .check:
            strike = 0
            ball = 0
            //하나도 누르지 않은 경우
            if firstNumLB.text == "" {
                resultLB.text = "첫번째 숫자를 입력하세요."
            //두번째를 누르지 않은 경우
            } else if secondNumLB.text == "" {
                resultLB.text = "두번째 숫자를 입력하세요."
            //세번째를 누르지 않은 경우
            } else if thirdNumLB.text == "" {
                resultLB.text = "세번째 숫자를 입력하세요."
            } else {
                //세자리를 모두 채운 경우 화면에 입력된 숫자를 지운다.
                firstNumLB.text = ""
                secondNumLB.text = ""
                thirdNumLB.text = ""
                round += 1
                //컴퓨터의 랜덤값 지정
                if computerArray == [] {
                    while computerArray.count < 3 {
                        randomNum = Int(arc4random_uniform(10))
                        if !computerArray.contains(randomNum) {
                            computerArray.append(randomNum)
                        }
                    }
                }
                //답안지 확인용
                print(userArray)
                print(computerArray)
                
                //사용자가 입력한값과 컴퓨터의 값 비교하여 출력
                for i in 0...2 {
                    //순서까지 맞다면 스트라이크
                    if userArray[i] == computerArray[i] {
                        strike += 1
                    //순서까지 맞지 않는 경우에만 한해서 나머지 값과 비교하여 볼을 추가
                    } else if userArray[i] == computerArray[0] {
                        ball += 1
                    } else if userArray[i] == computerArray[1] {
                        ball += 1
                    } else if userArray[i] == computerArray[2] {
                        ball += 1
                    }
                }
                
                //결과값 출력 및 히스토리 출력
                if strike == 3 {
                    resultLB.text = "\(round)회만에 정답을 맞추셨습니다!"
                    historyLB.text = historyLB.text + "정답은 바로 \(userArray)였습니다!"
                    //더이상 버튼 입력 불가
                    ableBTN = false
                } else {
                    //시도횟수 추가 및 기타 정보 출력
                    resultLB.text = "\(round)회 \(strike)스트라이크, \(ball)볼"
                    historyLB.text = historyLB.text + "\(round)회 \(userArray)입력, \(strike)스트라이크, \(ball)볼.\n"
                    userArray = []
                }
            }
            
        //지우기 버튼
        case .delete:
            if thirdNumLB.text != "" {
                thirdNumLB.text = ""
                userArray.remove(at: 2)
                resultLB.text = "세번째 값을 지웠습니다."
            } else if secondNumLB.text != "" {
                secondNumLB.text = ""
                userArray.remove(at: 1)
                resultLB.text = "두번째 값을 지웠습니다."
            } else if firstNumLB.text != "" {
                firstNumLB.text = ""
                userArray.remove(at: 0)
                resultLB.text = "첫번째 값을 지웠습니다."
            } else {
                resultLB.text = "지울 값이 없습니다."
            }
            
        //리셋 버튼 -> 모든 값을 초기화한다.
        case .reset:
            firstNumLB.text = ""
            secondNumLB.text = ""
            thirdNumLB.text = ""
            userArray = []
            computerArray = []
            resultLB.text = "새게임이 시작됐습니다."
            historyLB.text = ""
            strike = 0
            ball = 0
            round = 0
            ableBTN = true
        }
    }
    
    //번호 버튼
    @IBAction func numberBTN(_ sender: UIButton) {
        if ableBTN == true {
            if firstNumLB.text == "" {
                firstNumLB.text = "\(sender.tag)"
                userArray.append(sender.tag)
            } else if secondNumLB.text == "" {
                sameNumCheck(inputNum: sender.tag, inputLabel: secondNumLB)
            } else if thirdNumLB.text == "" {
                sameNumCheck(inputNum: sender.tag, inputLabel: thirdNumLB)
            }
        } else {
            resultLB.text = "새게임을 시작하세요."
        }
    }
    
    //번호의 중복 처리하는 함수
    func sameNumCheck(inputNum: Int, inputLabel: UILabel) {
        if userArray.count <= 3 {
            if !userArray.contains(inputNum) {
                inputLabel.text = "\(inputNum)"
                userArray.append(inputNum)
            } else if userArray.contains(inputNum) {
                resultLB.text = "중복값을 피하세요."
            }
        }
    }
}
```
