---
title: 컬렉션뷰 당겨서 업데이트
layout: post
---

### 컬렉션뷰 당겨서 업데이트

* 당겨서 업데이트는 스크롤 뷰에 정의되어 있으며 스크롤뷰를 상속받는 컬렉션뷰 혹은 테이블뷰에서 사용이 가능하다.
* 이번에는 컬렉션뷰에서 사용하는 경우를 다룬다.
* 코드는 다음과 같다.

```swift
import UIKit

class ViewController: UIViewController, UICollectionViewDelegate, UICollectionViewDataSource {
    // 컬렉션뷰에 들어갈 데이터
    var collectionViewData = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20]
    
    // 컬렉션뷰 프로퍼티 정의
    @IBOutlet var collectionView: UICollectionView!
    
    
    
    // 컬렉션뷰의 셀 개수 정의
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return collectionViewData.count
    }
    
    // 컬렉션뷰의 셀 내용 정의
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "collectionViewCell", for: indexPath)
        if let label = cell.viewWithTag(100) as? UILabel {
            label.text = "\(collectionViewData[indexPath.row])"
        }
        return cell
    }
    
    // 당겨서 업데이트시 실행될 내용
    @objc func refresh() {
        let add = collectionViewData.count + 1
        collectionViewData.append(add)
        let index = IndexPath(row: collectionViewData.count - 1, section: 0)
        collectionView.insertItems(at: [index])
        collectionView.refreshControl?.endRefreshing()
    }
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // 컬렉션뷰 셀 사이즈 커스터마이징
        let width = (view.frame.size.width - 20) / 3
        let layout = collectionView.collectionViewLayout as? UICollectionViewFlowLayout
        layout?.itemSize = CGSize(width: width, height: width)
        
        // 당겨서 업데이트 정의
        let refresh = UIRefreshControl()
        refresh.addTarget(self, action: #selector(self.refresh), for: .valueChanged)
        collectionView.refreshControl = refresh
    }
}
```

---

* 실행결과는 다음과 같다.

[![collectionView](https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/17-10-25/01.png?raw=true)](https://vimeo.com/242764972)

* 클릭하면 구동 영상을 감상할 수 있습니다.
