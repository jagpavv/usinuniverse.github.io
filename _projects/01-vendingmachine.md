---
title: 자판기 앱
layout: post
---

# 첫번째 연습 - 자판기 앱

### UI 구현

[![VendingMachine](https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/img/thumbnail/VendingMachine.png?raw=true)](https://vimeo.com/255879542)

* 클릭하면 구동 영상을 감상할 수 있습니다.

### 작동방식

* 자판기 형식으로, 원하는 금액을 선택 후 충전금액과 여행지의 여행비가 일치 혹은 충전금액이 더 클 경우 여행지를 예약한다.

### 기술 구현

* 충전할때마다 충전금과 총액을 표시
* 충전금과 여행비를 비교하여 계산

### 어려웠던 점

* 단위 표시를 하는 방법 (1000000원 -> 1,000,000원)
* formatter를 통해 해결했다.

### 코드

* ViewController.swift

```swift
import UIKit

var formatter = NumberFormatter()

class ViewController: UIViewController {
    
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        formatter.numberStyle = .decimal
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
///////////////////////////////////////////////////////////////
    
    //상태창
    @IBOutlet weak var status: UILabel!
    
    //보유금(디스플레이)
    @IBOutlet weak var displayMoney: UILabel!
    
    //보유금
    var currentMoney: Int = 0
    
    //도시 열거
    enum Citis: Int {
        case hawaii, santorini, venezia, praha
    }
    
    //금액 열거
    enum Money: Int {
        case tenMil = 4, fiveMil, mil
    }
    
    //단위 추가 위한 함수
    func convertIntToString(some: Int) -> String {
        let number = NSNumber(value: some)
        return formatter.string(from: number)!
    }
    
    //도시 선택 버튼
    @IBAction func selectCity(_ sender: UIButton) {
        
        let selectedCity: Citis = Citis(rawValue: sender.tag)!
        switch selectedCity {
        case .hawaii:
            autoCheck(price: 3000000, whereTo: "하와이")
        case .santorini:
            autoCheck(price: 2500000, whereTo: "산토리니")
        case .venezia:
            autoCheck(price: 3500000, whereTo: "베네치아")
        case .praha:
            autoCheck(price: 2750000, whereTo: "프라하")
        }
    }
    
    //도시 선택에 따른 자동 계산
    func autoCheck(price: Int, whereTo: String) {
        if currentMoney >= price {
            
            currentMoney -= price
            
            let decimalString = convertIntToString(some: currentMoney)
            displayMoney.text = "현재 보유금 \(decimalString)원"
            status.text = "\(whereTo) 여행을 예약합니다."
        } else {
            status.text = "잔액이 부족합니다."
        }
    }
    
    //금액 선택 버튼
    @IBAction func selectMoney(_ sender: UIButton) {
        
        let myMoney: Money = Money(rawValue: sender.tag)!
        switch myMoney {
        case .tenMil:
            autoMoney(clickedNum: 1000000)
        case .fiveMil:
            autoMoney(clickedNum: 500000)
        case .mil:
            autoMoney(clickedNum: 100000)
        }
    }
    
    //금액 선택에 따른 자동 계산
    func autoMoney(clickedNum: Int) {
        currentMoney += clickedNum
        let decimalClickNum = convertIntToString(some: clickedNum)
        let decimalString = convertIntToString(some: currentMoney)
        status.text = "\(decimalClickNum)원을 충전하였습니다."
        displayMoney.text = "현재 보유금 \(decimalString)원"
    }

}
```
