---
title: 컬렉션뷰 스티키 헤더
layout: post
---

### 스티키 헤더

* 스티키 헤더란? 컬렉션뷰의 헤더 영역이 바로 스크롤되어 사라지지 않고 해당 섹션이 끝나 다음 섹션을 만날때까지 유지되는 것을 칭한다.
* 스티키 헤더가 없을 때에는 다음과 같다.

[![stickyheader](https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/postingIMG/11-15/02.png?raw=true)](https://vimeo.com/242765042)

* 클릭하면 구동 영상을 감상할 수 있습니다.

---

* 스티키 헤더의 추가는 다음과 같다.

```swift
import UIKit

class ViewController: UIViewController, UICollectionViewDelegate, UICollectionViewDataSource {
    // 컬렉션뷰에 들어갈 데이터
    var collectionViewData = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20]
    var collectionViewData2 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14]
    
    // 컬렉션뷰 프로퍼티 정의
    @IBOutlet var collectionView: UICollectionView!
    
    
    // 컬렉션뷰의 섹션 개수 정의
    func numberOfSections(in collectionView: UICollectionView) -> Int {
        return 2
    }
    
    // 컬렉션뷰의 셀 개수 정의
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        if section == 0 {
            return collectionViewData.count
        } else {
            return collectionViewData2.count
        }
    }
    
    // 컬렉션뷰의 셀 내용 정의
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "collectionViewCell", for: indexPath)
        if let label = cell.viewWithTag(100) as? UILabel {
            if indexPath.section == 0 {
                label.text = "\(collectionViewData[indexPath.row])"
            } else {
                label.text = "\(collectionViewData2[indexPath.row])"
            }
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
        
        layout?.sectionHeadersPinToVisibleBounds = true ////////////////////// <----- 스티키 헤더 설정
        
        // 당겨서 업데이트 정의
        let refresh = UIRefreshControl()
        refresh.addTarget(self, action: #selector(self.refresh), for: .valueChanged)
        collectionView.refreshControl = refresh
    }
}
```

* 결과는 다음과 같다.

[![stickyheader](https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/postingIMG/11-15/01.png?raw=true)](https://vimeo.com/242765068)

* 클릭하면 구동 영상을 감상할 수 있습니다.
