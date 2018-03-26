---
title: 서비차 설정하기
layout: post
---

### 서치바

* 많은 양의 데이터를 다룰 때 카테고리별로 분류된 데이터를 제공할 수 있지만 서치바를 통한 검색기능을 제공하여 빠른 데이터 제공이 가능하다.
* 이번에는 테이블뷰와 연동하여 테이블 뷰 내에서 원하는 데이터를 찾고, 그 결과를 테이블뷰에 다시 로드하는 과정이다.
* 전체 코드는 다음과 같다.


```swift
import UIKit


class TableViewController: UITableViewController, UISearchBarDelegate {
    
    @IBOutlet var searchBar: UISearchBar!

    @IBOutlet var resultTableView: UITableView!
    
    var location = Location()
    
    var filteredLocation = Location()
    
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
        searchBar.delegate = self
    }
    
    // 필터된 데이터 배열의 갯수만큼 로우 생성
    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return filteredLocation.data.count
    }
    
    // 셀 생성
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
        
        cell.textLabel?.text = filteredLocation.data[indexPath.row]
        
        return cell
    }
    
    // 서치바에 입력된 String을 바탕으로 filteredLocation 배열 생성 그리고 테이블 뷰 리로드
    func searchBar(_ searchBar: UISearchBar, textDidChange searchText: String) {
        filteredLocation.data = searchText.isEmpty ? location.data : location.data.filter({ (dataString: String) -> Bool in
            return dataString.contains(searchText)
        })
        resultTableView.reloadData()
    }
}

class Location {
    var data = ["서울시", "부산시", "성남시", "수원시", "인천시", "안양시"]
}
```

* 실행결과는 다음과 같다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/postingIMG/11-20/01.png?raw=true">
<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/postingIMG/11-20/02.png?raw=true">
<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/postingIMG/11-20/03.png?raw=true">
