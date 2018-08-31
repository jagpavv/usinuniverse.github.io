---
title: UICollectionViewCell 옮기기
layout: post
---

# UICollectionViewCell 옮기기

* UICollectionViewCell을 드래그 앤 드롭 방법으로 옮길 수 있는 방법은 두 가지가 있다.
* ViewController가 UICollectionView를 상속받았을 경우와 그렇지 않은 경우 각각 다른 방법으로 가능하다.

### UICollectionViewController 상속

```swift
import UIKit



class CollectionViewController: UICollectionViewController {
    // MARK: - Properties
    // MARK: Custom
    
    var collectionData = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12"]
    
    
    
    // MARK: - Methods
    // MARK: View Life Cycle
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        guard let layout = self.collectionViewLayout as? UICollectionViewFlowLayout else { return }
        layout.itemSize = CGSize(width: ((self.view.bounds.width - 20) / 3), height: ((self.view.bounds.width - 20) / 3))
    }
    
    // MARK: Memory Management
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    // MARK: UICollectionViewDataSource
    
    override func numberOfSections(in collectionView: UICollectionView) -> Int {
        return 1
    }
    
    override func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return self.collectionData.count
    }
    
    override func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "Cell", for: indexPath)
        guard let textLabel = cell.viewWithTag(100) as? UILabel else { return UICollectionViewCell() }
        textLabel.text = self.collectionData[indexPath.row]
        
        return cell
    }
    
    // 이 메서드 내에 적절한 구현만 있다면 별도의 다른 코드는 추가하지 않아도 된다. (가장 편한 방법)
    override func collectionView(_ collectionView: UICollectionView, moveItemAt sourceIndexPath: IndexPath, to destinationIndexPath: IndexPath) {
        let removed = self.collectionData.remove(at: sourceIndexPath.row)
        self.collectionData.insert(removed, at: destinationIndexPath.row)
    }
    
}
```

* 결과

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-08-31/01.gif?raw=true">

### UICollectionView 별도 구현

```swift
import UIKit



class ViewController: UIViewController {
    // MARK: - Properties
    // MARK: Custom
    
    var collectionData = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12"]
    
    // MARK: IBOutlet
    
    @IBOutlet weak var mainCollectionView: UICollectionView!
    
    
    
    // MARK: - Methods
    // MARK: View Life Cycle
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        guard let layout = mainCollectionView.collectionViewLayout as? UICollectionViewFlowLayout else { return }
        layout.itemSize = CGSize(width: ((self.view.bounds.size.width - 20) / 3), height: ((self.view.bounds.size.width - 20) / 3))
        
        // 1. Gesture 설정
        let longPressGesture = UILongPressGestureRecognizer(target: self, action: #selector(self.longPressGesture(_:)))
        self.mainCollectionView.addGestureRecognizer(longPressGesture)
    }
    
    // MARK: Memory Management
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    // MARK: Custom
    // 2. Gesture 동작 설정
    @objc func longPressGesture(_ gesture: UILongPressGestureRecognizer) {
        switch gesture.state {
        case .began:
            guard let selectedIndexPath = mainCollectionView.indexPathForItem(at: gesture.location(in: mainCollectionView)) else { break }
            self.mainCollectionView.beginInteractiveMovementForItem(at: selectedIndexPath)
        case .changed:
            self.mainCollectionView.updateInteractiveMovementTargetPosition(gesture.location(in: gesture.view!))
        case .ended:
            self.mainCollectionView.endInteractiveMovement()
        default:
            self.mainCollectionView.cancelInteractiveMovement()
        }
    }
    
}

// MARK: - Extension
// MARK: UI Collection View Data Source

extension ViewController: UICollectionViewDataSource {
    
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return self.collectionData.count
    }
    
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "Cell", for: indexPath)
        guard let textLabel = cell.viewWithTag(100) as? UILabel else { return UICollectionViewCell() }
        textLabel.text = self.collectionData[indexPath.row]
        
        return cell
    }
    
}

// MARK: UI Collection View Delegate

extension ViewController: UICollectionViewDelegate {
    // 3. 이동 가능하게 설정
    func collectionView(_ collectionView: UICollectionView, canMoveItemAt indexPath: IndexPath) -> Bool {
        return true
    }
    // 4. 내부 로직
    func collectionView(_ collectionView: UICollectionView, moveItemAt sourceIndexPath: IndexPath, to destinationIndexPath: IndexPath) {
        let removed = self.collectionData.remove(at: sourceIndexPath.row)
        self.collectionData.insert(removed, at: destinationIndexPath.row)
    }
    
}
```

* 결과

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-08-31/02.gif?raw=true">
