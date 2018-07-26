---
title: 코드로 만드는 auto layout
layout: post
---

# 코드로 만드는 auto layout

### 1. 너비, 높이 고정

```swift
// 1번 방법
let blackView = UIView()
blackView.backgroundColor = UIColor.black
blackView.frame.size.width = 240
blackView.frame.size.height = 150
blackView.center.x = self.view.center.x
blackView.center.y = self.view.center.y
        
self.view.addSubview(blackView)

// 2번 방법
let blackView = UIView(frame: CGRect(x: self.view.center.x - 120, y: self.view.center.y - 75, width: 240, height: 150))
blackView.backgroundColor = UIColor.black

self.view.addSubview(blackView)
```

* 결과

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-07-26/01.png?raw=true">

### 2. 제약 고정

```swift
let blackView = UIView()
blackView.backgroundColor = UIColor.black
blackView.translatesAutoresizingMaskIntoConstraints = false
        
self.view.addSubview(blackView)
        
let topConstraint = NSLayoutConstraint(item: blackView, attribute: .top, relatedBy: .equal, toItem: self.view, attribute: .top, multiplier: 1.0, constant: 150)
let bottomConstraint = NSLayoutConstraint(item: blackView, attribute: .bottom, relatedBy: .equal, toItem: self.view, attribute: .bottom, multiplier: 1.0, constant: -150)
let leadingConstraint = NSLayoutConstraint(item: blackView, attribute: .leading, relatedBy: .equal, toItem: self.view, attribute: .leading, multiplier: 1.0, constant: 50)
let trailingConstraint = NSLayoutConstraint(item: blackView, attribute: .trailing, relatedBy: .equal, toItem: self.view, attribute: .trailing, multiplier: 1.0, constant: -50)
        
self.view.addConstraints([topConstraint, bottomConstraint, leadingConstraint, trailingConstraint])
```

* 결과

<img src="https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/posting%20images/18-07-26/02.png?raw=true">
