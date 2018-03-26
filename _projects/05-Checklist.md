---
title: 체크리스트
layout: post
---

# 다섯번째 연습 - 체크리스트

### UI 구현

[![Checklist](https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/project%20images/05/01.png?raw=true)](https://vimeo.com/241466595)

* 클릭하면 구동 영상을 감상할 수 있습니다.

### 체크리스트 사용법

* +(Add) 버튼을 통해 체크리스트를 추가한다.
* 수정은 우측 버튼을 통해 한다.
* 다 된 체크리스트는 터치하여 체크한다.
* 삭제는 밀어서 삭제 해당 체크리스트를 삭제한다.

### 기술 구현

* 아무런 데이터가 없을 때 빈 테이블만 존재하는 것을 방지했다.
* 체크리스트 추가시 텍스트를 입력하지 않으면 Done 버튼(우측상단, 키보드)을 누르지 못하도록 했다.
* 세그웨이를 통해 체크리스트를 추가할 때에는 내비게이션 바 타이틀을 Add Checklist로, 수정시에는 Edit Checklist로 했다.
* 뷰 컨트롤러간 데이터 전달을 직접적인 전달이 아닌 델리게이트를 통한 간접적인 전달이 되도록 했다.

### 어려웠던 점

* 델리게이트 패턴

```
수정중
```

### 코드

* ChecklistItem.swift

```swift
import Foundation

class ChecklistItem: NSObject {
    // 저장될 체크리스트
    var text = ""
    // 체크 여부
    var checked = false
    
    
    
    // 체크리스트를 누르면 체크하고 다시 누르면 체크를 해제
    func toggleChecked() {
        checked = !checked
    }
}
```

* ChecklistItemViewController.swift

```swift
import UIKit

class ChecklistViewController: UITableViewController, DatailItemViewControllerDelegate {
    // 체크리스트가 저장될 변수
    var items: [ChecklistItem] = []
    
    
    
    // 섹션안에 몇개의 셀을 만들지 결정
    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return items.count
    }
    
    // 셀의 내용 결정하여 리턴
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        // 셀 재사용
        let cell = tableView.dequeueReusableCell(withIdentifier: "Checklistitem", for: indexPath)
        let item = items[indexPath.row]
        configureText(for: cell, with: item) // 체크리스트 내용
        configureCheckmark(for: cell, with: item) // 체크리스트 체크 여부
        return cell
    }
    
    // 체크리스트 내용
    func configureText(for cell: UITableViewCell, with item: ChecklistItem) {
        let label = cell.viewWithTag(1000) as! UILabel
        label.text = item.text
    }
    
    // 체크리스트 체크 여부
    func configureCheckmark(for cell: UITableViewCell, with item: ChecklistItem) {
        let label = cell.viewWithTag(1001) as! UILabel
        if item.checked {
            label.text = "√"
        } else {
            label.text = ""
        }
    }
    
    // segueway를 통해 데이터 전달
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        if segue.identifier == "AddItem" {
            let controller = segue.destination as! DetailItemViewController
            controller.delegate = self
        } else if segue.identifier == "EditItem" {
            let controller = segue.destination as! DetailItemViewController
            controller.delegate = self
            if let indexPath = tableView.indexPath(for: sender as! UITableViewCell) {
                controller.itemToEdit = items[indexPath.row]
            }
        }
    }
    
    // Cancel 버튼
    func detailViewControllerDidCancel(_ controller: DetailItemViewController) {
        navigationController?.popViewController(animated: true)
    }
    
    // Done 버튼
    func detailViewController(_ controller: DetailItemViewController, didFinishAdding item: ChecklistItem) {
        let newRowIndex = items.count
        items.append(item)
        let indexPath = IndexPath(row: newRowIndex, section: 0)
        let indexPaths = [indexPath]
        tableView.insertRows(at: indexPaths, with: .automatic)
        navigationController?.popViewController(animated: true)
    }
    
    // Edit 버튼
    func detailViewController(_ controller: DetailItemViewController, didFinishEdtiting item: ChecklistItem) {
        if let index = items.index(of: item) {
            let indexPath = IndexPath(row: index, section: 0)
            if let cell = tableView.cellForRow(at: indexPath) {
                configureText(for: cell, with: item)
            }
        }
        navigationController?.popViewController(animated: true)
    }
    
    // UX 관련 설정
    // 셀을 선택했을 때 나타나는 반응들을 정의
    override func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        if let cell = tableView.cellForRow(at: indexPath) {
            let item = items[indexPath.row]
            item.toggleChecked()
            configureCheckmark(for: cell, with: item) // 체크리스트 체크 여부
        }
        tableView.deselectRow(at: indexPath, animated: true) // 선택된 셀이 계속 하이라이팅 되지 않도록 함
    }
    
    // 셀을 밀어서 삭제함
    override func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCellEditingStyle, forRowAt indexPath: IndexPath) {
        items.remove(at: indexPath.row)
        tableView.reloadData()
    }
}
```

* DetailViewController.swift

```swift
import UIKit

protocol DatailItemViewControllerDelegate: class {
    func detailViewControllerDidCancel(_ controller: DetailItemViewController)
    func detailViewController(_ controller: DetailItemViewController, didFinishAdding item: ChecklistItem)
    func detailViewController(_ controller: DetailItemViewController, didFinishEdtiting item: ChecklistItem)
}

class DetailItemViewController: UITableViewController, UITextFieldDelegate {
    
    var itemToEdit: ChecklistItem?
    
    // Done 버튼
    @IBOutlet var doneBTNOulet: UIBarButtonItem!
    // 체크리스트 추가 텍스트 필드
    @IBOutlet var textField: UITextField!
    // 델리게이트 선언
    weak var delegate: DatailItemViewControllerDelegate?
    
    
    
    override func viewDidLoad() {
        if let item = itemToEdit {
            navigationController?.title = "Edit Checklist"
            textField.text = item.text
            doneBTNOulet.isEnabled = true
        }
    }
    
    // Done 버튼
    // 텍스트를 입력하고 버튼을 누르면 체크리스트가 추가
    @IBAction func doneBTN(_ sender: Any) {
        if let itemToEdit = itemToEdit {
            itemToEdit.text = textField.text!
            delegate?.detailViewController(self, didFinishEdtiting: itemToEdit)
        } else {
            let item = ChecklistItem()
            item.text = textField.text!
            item.checked = false
            delegate?.detailViewController(self, didFinishAdding: item)
        }
    }
    
    // Cancel 버튼
    // 버튼을 누르면 지금까지의 입력은 취소되고 돌아감
    @IBAction func cancelBTN(_ sender: Any) {
        delegate?.detailViewControllerDidCancel(self)
    }
    
    // UX 관련 설정
    // 화면 나타나기전 키보드 올리기
    override func viewWillAppear(_ animated: Bool) {
        textField.becomeFirstResponder()
    }
    
    // 텍스트 필드가 위치한 셀은 선택하지 못하게 함
    override func tableView(_ tableView: UITableView, willSelectRowAt indexPath: IndexPath) -> IndexPath? {
        return nil
    }
    
    // 텍스트 필드에 텍스트가 입력될 때마다 불리는 메소드
    // 텍스트가 빈칸인 경우 Done 버튼을 비활성화, 그렇지 않다면 활성화
    func textField(_ textField: UITextField, shouldChangeCharactersIn range: NSRange, replacementString string: String) -> Bool {
        let oldText = textField.text!
        let stringRange = Range(range, in:oldText)
        let newText = oldText.replacingCharacters(in: stringRange!, with: string)
        if newText.isEmpty {
            doneBTNOulet.isEnabled = false
        } else {
            doneBTNOulet.isEnabled = true
        }
        return true
    }
}
```
