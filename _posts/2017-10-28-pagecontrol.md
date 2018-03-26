---
title: 페이지 컨트롤
layout: post
---

### 페이지 컨트롤

* 페이지 컨트롤이란? 쉽게 말해 스크롤 뷰에서 컨텐츠의 갯수를 제공함과 동시에 사용자의 위치를 나타내는 기능을 한다.

* 페이지 컨트롤은 수학적인 계산이 필요하며, 스크롤 뷰와 연동되지 않기 때문에 코드로 구현을 해야한다.

* 코드 구현은 다음과 같다.

```swift
import UIKit

class ViewController: UIViewController {
    
    @IBOutlet weak var scrollView: UIScrollView!
    
    @IBOutlet weak var pageControl: UIPageControl!
    
    var pageNumber = [UIView]()
    
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        scrollView.contentSize = CGSize(width: view.frame.size.width * 3, height: view.frame.size.height)
        
        let greenView = UIView(frame: CGRect(x: 0, y: 0, width: view.frame.size.width, height: view.frame.size.height))
        greenView.backgroundColor = UIColor.green
        scrollView.addSubview(greenView)
        
        let blueView = UIView(frame: CGRect(x: greenView.frame.size.width, y: 0, width: view.frame.size.width, height: view.frame.size.height))
        blueView.backgroundColor = UIColor.blue
        scrollView.addSubview(blueView)
        
        let redView = UIView(frame: CGRect(x: greenView.frame.size.width + blueView.frame.size.width, y: 0, width: view.frame.size.width, height: view.frame.size.height))
        redView.backgroundColor = UIColor.red
        scrollView.addSubview(redView)
        
        pageNumber.append(greenView)
        pageNumber.append(blueView)
        pageNumber.append(redView)
        
        pageControl.numberOfPages = pageNumber.count
        pageControl.pageIndicatorTintColor = UIColor.black
        pageControl.currentPageIndicatorTintColor = UIColor.white
    }
}

extension ViewController: UIScrollViewDelegate {
    func scrollViewDidScroll(_ scrollView: UIScrollView) {
        let pageWidth = scrollView.bounds.width
        let pageFraction = scrollView.contentOffset.x / pageWidth
        pageControl.currentPage = Int(round(pageFraction))
    }
}
```

* 페이지 컨트롤의 현재 페이지를 구하는 공식은 유저의 수평 포지션을 구한 후 전체 스크롤 뷰의 넓이로 나누면 된다.

* 결과는 다음과 같다.

[![stickyheader](https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/17-10-28/01.png?raw=true)](https://vimeo.com/244973382)

* 클릭하면 구동 영상을 감상할 수 있습니다.

---

* 페이지 컨트롤의 페이지 갯수, 컬러 등은 코드와 스토리보드를 통해 설정이 가능하다.

* 아래는 코드와 스토리보드를 통해 설정할 수 있는 방법을 각각 보여준다.

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/17-10-28/02.png?raw=true">

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/17-10-28/03.png?raw=true">


 
