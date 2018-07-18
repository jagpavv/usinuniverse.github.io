---
tite: Xcode 디버깅(Breakpoint)
layout: post
---

# Xcode 디버깅(Breakpoint)

### Breakpoint란?

* 직역하자면 중단지점 정도가 되는 이 Breakpoint의 역할은 소스코드 중간에 삽입되어 해당 부분에서 앱의 진행을 멈추고 디버깅을 할 수 있게 한다.

### Breakpoint 기본적인 사용법

* 기본적인 사용법으로는 코드의 라인 넘버를 클릭하면 활성화되고, 드래그 앤 드롭으로 삭제할 수도, 다시 한 번 클릭해서 비활성화 시킬 수 있다.
* 👇🏿(이렇게)

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-07-18/01.gif?raw=true">

---

### Breakpoint 파고들기

* 우선 간단한 코드부터 보자.
* 불필요한 부분이 있지만 Breakpoint 활용법을 보여주고자 일부러 넣은 부분도 있으니 불편하더라도 무시하고, 전체 내용은 과일 이름을 테이블 뷰에 보여주는게 전부다.

```swift
import UIKit



class ViewController: UIViewController {
    
    // MARK: - Properties
    // MARK: Custom
    
    var fruits: [String]?
    
    
    
    // MARK: - Methods
    // MARK: View Life Cycle
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.fruits = self.configureFruits()
        print(self.fruits!)
    }
    
    // MARK: Custom
    
    func configureFruits() -> [String] {
        return ["Apple", "Banana", "Melon", "Lemon", "Strawberry"]
    }
    
    // MARK: Memory Management
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
}

// MARK: - Extension
// MARK: UI Table View Data Source

extension ViewController: UITableViewDataSource {
    
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return self.fruits?.count ?? 0
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath)
        guard let fruits = self.fruits else { return cell }
        
        cell.textLabel?.text = fruits[indexPath.row]
        
        return cell
    }
    
}



```

* Breakpoint로 앱을 중단하면 Xcode에서 다음과 같은 버튼을 확인할 수 있다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-07-18/02.png?raw=true">

* 각각의 이름은 순서대로 Deactive Breakpoints, Continue program execution, Step over, Step into, Step out 이다.

---

### Deactive Breakpoints

* 이 기능은 지금까지 추가한 Breakpoint 들을 일일이 클릭하지 않아도 전부 비활성화, 활성화 시킬 수 있는 버튼이다.

### Continue program execution

* 이 기능은 다음 Breakpoint로 이동하게 한다.
* 이때 다음 Breakpoint가 없다면 더 이상 중단되지 않는다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-07-18/03.gif?raw=true">

### Step over

* 이 기능은 굳이 여러개의 Breakpoint를 추가하지 않더라도 다음 라인의 코드에서 자동으로 중단된다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-07-18/04.gif?raw=true">

### Step into

* 이 기능은 해당 라인의 코드에서 사용되는 메서드나 변수를 디버깅할 수 있게 한다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-07-18/05.gif?raw=true">

### Step out

* Step into와 반대 기능

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-07-18/06.gif?raw=true">

---

### 기타 팁

* 다음처럼 Share Breakpoint로 다른 팀원과 Breakpoint를 공유할 수 있다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-07-18/07.gif?raw=true">

* 또는 앱을 시작하자마자 발생하는 Exeption error를 디버깅하기 위해서 Exeption breakpoint를 만들 수도 있다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-07-18/08.gif?raw=true">
