---
title: 로그인 페이지 구현
layout: post
---

# 세번째 연습 - 로그인 페이지 구현

### UI 구현

[![LoginPage](https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/img/thumbnail/LoginPage.png?raw=true)](https://vimeo.com/257069165)

* 클릭하면 구동 영상을 감상할 수 있습니다.

### 기술 구현

* 잘못된 정보를 입력했을 때 UITextField 애니메이션을 통해 알림

### 어려웠던 점

* 뷰 컨트롤러간의 데이터 전달 방법

### 코드

* LoginViewContoller.swift

```swift
import UIKit

class LoginViewController: UIViewController {

    var userModel: UserModel = UserModel()
    
    @IBOutlet weak var idTextField: UITextField!
    @IBOutlet weak var passwordTextField: UITextField!
    @IBOutlet weak var loginButton: RoundButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        idTextField.addTarget(self, action: #selector(didEndOnExit(_:)), for: UIControlEvents.editingDidEndOnExit)
        passwordTextField.addTarget(self, action: #selector(didEndOnExit(_:)), for: UIControlEvents.editingDidEndOnExit)
    }
    
    
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        print("Login ViewController will Appear")
    }
    
    override func viewWillLayoutSubviews() {
        super.viewWillLayoutSubviews()
        print("로그인 뷰컨트롤러 레이아웃이 결정되기 전에 호출")
    }
    override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        print("로그인 뷰컨트롤러 레이아웃이 결정된 후 호출")
    }
    
    //엔터 누를 시 다음 입력 칸으로 가기위한 함수
    @objc func didEndOnExit(_ sender: UITextField) {
        if sender.tag == 1 {
            passwordTextField.becomeFirstResponder()
        }
    }
    
    //로그인 버튼
    @IBAction func tapLoginButton(_ sender: RoundButton) {
        guard let id = idTextField.text else {
            return
        }
        guard let password = passwordTextField.text else {
            return
        }
        //계정 정보가 있는지 확인
//        let isLoginSuccess: Bool = userModel.findUser(inputID: id, inputPassword: password)
        let isLoginSuccess: Bool = userModel.findUser(inputID: id, inputPassword: password)
        //로그인 성공시
        if isLoginSuccess == true {
            let alertController = UIAlertController(title: "로그인 성공", message: "Welcome To Cotton World!!!", preferredStyle: .alert)
            let okAction = UIAlertAction(title: "확인", style: .destructive, handler: { (alert) in
                print("버튼이 눌림")
            })
            alertController.addAction(okAction)
            self.present(alertController, animated: true, completion: nil)
            
            print("로그인 성공")
        } else {
            //실패시 애니메이션효과
            UIView.animate(withDuration: 0.15, animations: {
                self.idTextField.frame.origin.x -= 10
                self.passwordTextField.frame.origin.x -= 10
                self.idTextField.backgroundColor = UIColor.red.withAlphaComponent(0.2)
                self.passwordTextField.backgroundColor = UIColor.red.withAlphaComponent(0.2)
            }, completion: { _ in
                UIView.animate(withDuration: 0.15, animations: {
                    self.idTextField.frame.origin.x += 20
                    self.passwordTextField.frame.origin.x += 20
                }, completion: { _ in
                    UIView.animate(withDuration: 0.15, animations: {
                        self.idTextField.frame.origin.x -= 10
                        self.passwordTextField.frame.origin.x -= 10
                        self.idTextField.backgroundColor = UIColor.white
                        self.passwordTextField.backgroundColor = UIColor.white
                    })
                })
            })
        }
    }
    
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        if let next = segue.destination as? SignUpViewController {
            next.didTaskClosure = { (name: String, password: String) -> Void in
                
                self.userModel.addUser(newID: name, newPassword: password)
            }
        }
    }
    
    //찾기 버튼
    @IBAction func forgot(_ sender: UIButton) {
    }
    //가입 버튼
    @IBAction func signUp(_ sender: UIButton) {
    }
    
    
    func findsuer(name: String, password: String) -> Bool {
        guard let userList: [[String: String]] = UserDefaults.standard.array(forKey: "userList") as? [[String: String]] else {
            return false
        }
        
        for userData in userList {
            let memberID: String = userData["ID"]!
            if memberID == name {
                let memperPW: String = userData["PW"]!
                if memperPW == password {
                    
                }
            }
        }
        
        return false
    }
    
    
    
    
}
```

* SignUpViewController.swift

```swift
//
//  SignUpViewController.swift
//  LoginPage
//
//  Created by usinuniverse on 2017. 9. 27..
//  Copyright © 2017년 usinuniverse. All rights reserved.
//

import UIKit

class SignUpViewController: UIViewController {

    override func viewWillDisappear(_ animated: Bool) {
        super.viewWillDisappear(animated)
        print("사인업 사라지기직전에")
    }
    
    override func viewDidDisappear(_ animated: Bool) {
        super.viewDidDisappear(animated)
        print("사인업 사라진 후")
    }
    
    
    
    
    
    //아이디 입력
    @IBOutlet weak var inputID: UITextField!
    //비밀번호 입력
    @IBOutlet weak var inputPassword: UITextField!
    //비밀번호 재입력
    @IBOutlet weak var inputRepassword: UITextField!
    
    var didTaskClosure: ((String, String) -> Void)? = nil
    
    override func viewDidLoad() {
        super.viewDidLoad()
        inputID.addTarget(self, action: #selector(didEndOnExit(_:)), for: UIControlEvents.editingDidEndOnExit)
        inputPassword.addTarget(self, action: #selector(didEndOnExit(_:)), for: UIControlEvents.editingDidEndOnExit)
        inputRepassword.addTarget(self, action: #selector(didEndOnExit(_:)), for: UIControlEvents.editingDidEndOnExit)
    }
    
    //뒤로가기위한 함수
    @IBAction func tapExitButton(_ sender: UIButton) {
        dismiss(animated: true, completion: nil)
    }
    
    //엔터 누를 시 다음 칸으로 가기
    @objc func didEndOnExit(_ sender: UITextField) {
        if sender.tag == 1 {
            inputPassword.becomeFirstResponder()
        } else if sender.tag == 2 {
            inputRepassword.becomeFirstResponder()
        }
    }
    
    //가입버튼
    @IBAction func signUpButton(_ sender: UIButton) {
        guard let id = inputID.text else {
            return
        }
        guard let password = inputPassword.text else {
            return
        }
        guard let rePassword = inputRepassword.text else {
            return
        }
        if password == rePassword {
            inputID.text = ""
            inputPassword.text = ""
            inputRepassword.text = ""
            print("Sign Up Success")
            
            let userData: [String: String] = ["ID": id, "PW": password]
            let list: [[String: String]] = [userData]
            
            UserDefaults.standard.set(list, forKey: "userList")
            
//            UserDefaults.standard.set(id, forKey: "ID")
//            UserDefaults.standard.set(password, forKey: "password")
            //didTaskClosure?(id, password)
            dismiss(animated: true, completion: nil)
            
        } else {
            //중복, 잘못 입력시 애니메이션
            UIView.animate(withDuration: 0.15, animations: {
                self.inputID.frame.origin.x -= 10
                self.inputPassword.frame.origin.x -= 10
                self.inputRepassword.frame.origin.x -= 10
                self.inputID.backgroundColor = UIColor.red.withAlphaComponent(0.2)
                self.inputPassword.backgroundColor = UIColor.red.withAlphaComponent(0.2)
                self.inputRepassword.backgroundColor = UIColor.red.withAlphaComponent(0.2)
            }, completion: { _ in
                UIView.animate(withDuration: 0.15, animations: {
                    self.inputID.frame.origin.x += 20
                    self.inputPassword.frame.origin.x += 20
                    self.inputRepassword.frame.origin.x += 20
                }, completion: { _ in
                    UIView.animate(withDuration: 0.15, animations: {
                        self.inputID.frame.origin.x -= 10
                        self.inputPassword.frame.origin.x -= 10
                        self.inputRepassword.frame.origin.x -= 10
                        self.inputID.backgroundColor = UIColor.white
                        self.inputPassword.backgroundColor = UIColor.white
                        self.inputRepassword.backgroundColor = UIColor.white
                    })
                })
            })
        }
    }
}
```

* UserModel.swift

```swift
import Foundation

final class UserModel {
    //저장된 계정 정보
    var model: [User] = [
        User(id: "admin@admin.com", password: "12345678"),
        User(id: "usinuniverse@gmail.com", password: "12345678")
    ]
    
    struct User {
        var id: String
        var password: String
    }
    
    //계정 정보 확인 함수
    func findUser(inputID: String, inputPassword: String) -> Bool {
        for user in model {
            if user.id == inputID && user.password == inputPassword {
                return true
            }
        }
        return false
    }
    
    //회원 추가 함수
    func addUser(newID: String, newPassword: String) {
        let newUser = User(id: newID, password: newPassword)
        model.append(newUser)
    }
}
```
