---
title: 데이터 구조(tree)
layout: post
---

### 데이터 구조란?

* 데이터 구조는 큰 데이터를 효과적으로 관리하기 위해 필요하다.
* 가령 도서관에 책이 분류도, 책장에도 놓여있지 않고 무분별하게 쌓여있다면 찾는 시간이 어마어마하게 걸리지만, 가나다순 혹은 분야별 등 분류 기준에 의해 정리되어 있다면 훨씬 효과적으로 관리 할 수 있다.
* 데이터 구조의 가장 기본적인 것이 tree 데이터 구조다.
* tree 데이터 구조는 뿌리, 줄기, 잎으로 구성되어 있으며 현실에서는 기업의 조직도나 디렉토리로 생각하면 쉽다.

<img src="https://koenig-media.raywenderlich.com/uploads/2016/06/Tree-2-650x300.png">

### tree 데이터 구조 기본지식

* Root(뿌리)는 데이터 구조의 시작점이며 0레벨이다.
* 뿌리에서 가지, 나무로 뻗어나갈 수록 레벨이 1씩 증가한다.

<img src="https://koenig-media.raywenderlich.com/uploads/2016/06/root-2-1.png">

* Node(가지)는 트리 구조 데이터의 블록이며 Root 또한 Node다.

<img src="https://koenig-media.raywenderlich.com/uploads/2016/06/root-4.png">

* Leaf(잎)은 최종 데이터이며 때로는 Node에서 추가적인 Node로 이어지지 않는 경우 Terminal node 라고 불린다. (Root에서 바로 Leaf가 되는 경우처럼.)

<img src="https://koenig-media.raywenderlich.com/uploads/2016/06/leaf.png">

### tree 데이터 구조 만들기

```swift
class Node {
    var value: String
    var children = [Node]()
    weak var parent: Node?
    
    
    init(value: String) {
        self.value = value
    }
    
    func add(child: Node) {
        children.append(child)
        child.parent = self
    }
}
```

* Node는 여러명의 자식을 둘 수 있기 때문에 배열로 정의하고, 반대로 부모는 하나뿐이거나 없는 경우도 있기 때문에 옵셔널로 정의한다.
* 또 add 메서드는 자식을 둠으로써 스스로가 부모가 되는 방식이다.

```swift
let beverage = Node(value: "beverage")

let hotBeverage = Node(value: "hot")
let coldBeverage = Node(value: "cold")

beverage.add(child: hotBeverage)
beverage.add(child: coldBeverage)
```

<img src="https://koenig-media.raywenderlich.com/uploads/2016/06/Example.png">

* 음료라는 큰 뿌리를 만들고, 뜨거운 음료와 차가운 음료를 만들었다.

```swift
let beverages = Node(value: "beverages")

let hotBeverage = Node(value: "hot")
let coldBeverage = Node(value: "cold")

let tea = Node(value: "tea")
let coffee = Node(value: "coffee")
let cocoa = Node(value: "cocoa")

let blackTea = Node(value: "black")
let greenTea = Node(value: "green")
let chaiTea = Node(value: "chai")

let soda = Node(value: "soda")
let milk = Node(value: "milk")

let gingerAle = Node(value: "ginger ale")
let bitterLemon = Node(value: "bitter lemon")

beverages.add(child: hotBeverage)
beverages.add(child: coldBeverage)

hotBeverage.add(child: tea)
hotBeverage.add(child: coffee)
hotBeverage.add(child: cocoa)

coldBeverage.add(child: soda)
coldBeverage.add(child: milk)

tea.add(child: blackTea)
tea.add(child: greenTea)
tea.add(child: chaiTea)

soda.add(child: gingerAle)
soda.add(child: bitterLemon)
```

<img src="https://koenig-media.raywenderlich.com/uploads/2016/06/Example-2.png">

* 다음과 같은 구조를 만들었다.

-----

### 데이터 구조 확인하기

* 데이터 구조를 확인하기 위해 Root를 print 해보면 결과가 제대로 나오지 않는다.
* 그 이유는 컴파일러는 사용자 정의 객체를 인쇄할 방법을 모르기 때문이다.
* 따라서 구조를 확인하기 위해서는 CustomStringConvertible 프로토콜을 채택해야 한다.
* CustomStringConvertible을 채택하면 String 형식의 이름 설명이 있는 연산 프로퍼티를 구현해야 한다.
* 자식의 자식까지 확인이 필요하므로 재귀적으로 작동해야 한다.

```swift
extension Node: CustomStringConvertible {
    var description: String {
        var text = "\(value)"
        
        if !children.isEmpty {
            text += " {" + children.map { $0.description }.joined(separator: ", ") + "} "
        }
        return text
    }
}
```

* 이제 print를 해보면 제대로 나온다.

```swift
print(beverages) // beverages {hot {tea {black, green, chai} , coffee, cocoa} , cold {soda {ginger ale, bitter lemon} , milk} } 
```

-----

### 데이터 찾기

```swift
extension Node {
    func search(value: String) -> Node? {
        
        if value == self.value {
            return self
        }
        
        for child in children {
            if let found = child.search(value: value) {
                return found
            }
        }
        
        return nil
    }
}
```

* 이 메서드는 트리안에 찾고자 하는 값이 있는지 확인하여 관련된 Node를 리턴해준다.
* 결과는 다음과 같다.

```swift
beverages.search(value: "cocoa") // returns the "cocoa" node
beverages.search(value: "chai") // returns the "chai" node
beverages.search(value: "hot") // returns "hot {tea {black, green, chai} , coffee, cocoa"
beverages.search(value: "bubbly") // returns nil

tea.search(value: "black") // returns the "black" node
tea.search(value: "herb") // returns nil
tea.search(value: "tea") // returns "tea {black, green, chai}"
```
