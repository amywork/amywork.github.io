---
layout: post
title: "로그인 화면 구현하기"
author: "younari"
---

> SignUp & SignIn - 로그인 화면 구현하기

# SignUp & SignIn
- [Xcode Project 바로가기](https://github.com/younari/tastySwift/tree/master/0927_SignUp)


## 01. User Model 클래스 생성
- ID와 비밀번호로 구성된 User 구조체를 담은 배열, model을 생성한다.
- model 내에 유저가 있는지 검사하는 메소드, 새로운 유저를 추가하는 메소드를 담는다.

{% highlight swift %}
final class UserModel {
    
    struct User {
        var username: String
        var password: String
    }
    
    var model: [User] = [
       User(username: "zico", password: "1234"),
       User(username: "dean", password: "5678"),
       User(username: "penomeco", password: "0101")
    ]
    
    // hasUser 검사 method
    func hasUser(name: String, pwd: String) -> Bool {
        for user in model {
            if user.username == name && user.password == pwd {
                return true
            }
        return false
        }
    }
        
    // newUser 추가 method
    func addUser(name: String, pwd: String) {
        let newUser = User(username: name, password: pwd)
        model.append(newUser)
    }

}
{% endhighlight %}

## 02. 로그인 화면 구성하기
- UIButton, UITextField를 활용하여 UI를 구성한다.
- **속성** usernameTextField, passwordTextField, loginButton
- **메소드01** didTapLoginButton : 로그인 버튼을 눌렀을 때의 액션
- **메소드02** didEndOnExit : 텍스트 필드에서 리턴키를 눌렀을 때 resignFirstResponder를 통해 키보드를 내리는 함수

{% highlight swift %}
import UIKit
class SignInViewController: UIViewController {

    /*****Property*****/
    var userModel = UserModel() // Model의 인스턴스 생성
    @IBOutlet weak var usernameTextField: UITextField!
    @IBOutlet weak var passwordTextField: UITextField!
    @IBOutlet weak var loginButton: RoundButton!
    
    func designTextField(_ textField: UITextField) {
        textField.layer.cornerRadius = 3
        textField.layer.borderColor = UIColor.lightGray.cgColor
        textField.layer.borderWidth = 1/UIScreen.main.scale
    }
    
    /*****viewDidLoad*****/
    override func viewDidLoad() {
        super.viewDidLoad()
        // TextField 의 디자인 입히기
        designTextField(usernameTextField)
        designTextField(passwordTextField)
        
        // Keyboard 내리기
        // addTarget은 UIResponder를 상속받은 클래스에게 모두 있다.
        usernameTextField.addTarget(self, action: #selector(didEndOnExit), for: UIControlEvents.editingDidEndOnExit)
        passwordTextField.addTarget(self, action: #selector(didEndOnExit), for: UIControlEvents.editingDidEndOnExit)
        loginButton.addTarget(self, action: #selector(didEndOnExit), for: UIControlEvents.editingDidEndOnExit)
    }

    /*****SignIn 버튼을 눌렀을 때의 Action*****/
    @IBAction func didTapLoginButton(_ sender: RoundButton) {
        // 옵셔널 바인딩 & 예외 처리 : Textfield가 빈문자열이 아니고, nil이 아닐 때
        guard let username = usernameTextField.text, !username.isEmpty else { return }
        guard let password = passwordTextField.text, !password.isEmpty else { return }
        
        // Model이 해당 유저를 가지고 있는지 검사
        let loginSuccess: Bool = userModel.hasUser(name: username, pwd: password)
        if loginSuccess {
            print("로그인 성공")
            let main = MainViewController() // MainViewController() : main으로 넘어가기
            self.present(main, animated: true, completion: nil) // present
        }else {
            UIView.animate(withDuration: 0.2, animations: {
                self.usernameTextField.frame.origin.x -= 10
                self.passwordTextField.frame.origin.x -= 10
            }, completion: { _ in
                UIView.animate(withDuration: 0.2, animations: {
                    self.usernameTextField.frame.origin.x += 20
                    self.passwordTextField.frame.origin.x += 20
                }, completion: { _ in
                    UIView.animate(withDuration: 0.2, animations: {
                        self.usernameTextField.frame.origin.x -= 10
                        self.passwordTextField.frame.origin.x -= 10
                    })
                })
            })
        }
    }
    

    @objc func didEndOnExit(_ sender: UITextField) {
        if usernameTextField.isFirstResponder {
            passwordTextField.becomeFirstResponder()
        }
    }

    /*****Segue, 할일을 하기 직전에 클로저를 담아서 보내줘 : prepare*****/
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        // segue.destination은 UIViewController의 상속을 받은 Down캐스팅(형변환)이기 때문에 안전하게 옵셔널 체이닝
        // segue.source 는 previous ViewController인 SignInViewController
        if let nextViewController = segue.destination as? SignUpViewController {
           // SignUpViewController의 didTaskClosure에 클로저를 만들어서 담아 보낸다.
            nextViewController.didTaskClosure = {
                (name: String, password: String) -> Void in return
                self.userModel.addUser(name: name, pwd: password)
            }
        }
	}
}
{% endhighlight %}

## 03. 위의 코드에서 prepare 함수 뜯어보기
- SignUp 버튼을 클릭했을 때 SignInViewController -> SignUpViewController로 이동하게끔 스토리보드에서 Segue를 지정해두었기 때문에,
- prepare 메소드를 통해 화면 전환이 될 때 클로저를 담아서 보내도록 설정한다.

{% highlight swift %}
    /*****Segue, 할일을 하기 직전에 클로저를 담아서 보내줘 : prepare*****/
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        // segue.destination은 UIViewController의 상속을 받은 Down캐스팅(형변환)이기 때문에 안전하게 옵셔널 체이닝
        // segue.source 는 previous ViewController인 SignInViewController
        if let nextViewController = segue.destination as? SignUpViewController {
           // SignUpViewController의 didTaskClosure에 클로저를 만들어서 담아 보낸다.
            nextViewController.didTaskClosure = {
                (name: String, password: String) -> Void in return
                self.userModel.addUser(name: name, pwd: password)
            }
        }
	}
}
{% endhighlight %}


## 04. 회원가입 화면 구성하기
- UIButton, UITextField를 활용하여 UI를 구성한다.
- **속성** usernameTextField, passwordTextField, passwordConformTextField signUpBtn
- **메소드01** signUpBtn : 회원가입 버튼을 눌렀을 때의 액션
- **메소드02** didTapExitButton : 화면 닫기를 눌렀을 때의 액션 
- **메소드03** didEndOnExit : 텍스트 필드에서 리턴키를 눌렀을 때 resignFirstResponder를 통해 키보드를 내리는 함수

{% highlight swift %}
import UIKit
class SignUpViewController: UIViewController {

    /*****Property*****/
    @IBOutlet weak var usernameTextField: UITextField!
    @IBOutlet weak var passwordTextField: UITextField!
    @IBOutlet weak var passwordConformTextField: UITextField!
    // SignInViewController 에서 넘어올 클로저를 받을 변수
    var didTaskClosure: ((String, String) -> Void)? = nil
    
    /*****Action*****/
    @IBAction func signUpBtn(_ sender: UIButton) {
        guard let username = usernameTextField.text else { return }
        guard let password = passwordTextField.text else { return }
        guard let conformPwd = passwordConformTextField.text else { return }
        
        if password == conformPwd {
            didTaskClosure?(username,password)
            dismiss(animated: true, completion: nil) // 화면 내리기
        }else {
            print("회원가입에 실패하였습니다.")
        }
    }
    
    
    /*****ViewDidLoad*****/
    /*****Return Key addTarget*****/
    override func viewDidLoad() {
        super.viewDidLoad()
        usernameTextField.addTarget(self, action: #selector(didEndOnExit), for: UIControlEvents.editingDidEndOnExit)
        passwordTextField.addTarget(self, action: #selector(didEndOnExit), for: UIControlEvents.editingDidEndOnExit)
        passwordConformTextField.addTarget(self, action: #selector(didEndOnExit), for: UIControlEvents.editingDidEndOnExit)
    }
    
    
    // Exit Function
    @IBAction func didTapExitButton(_ sender: UIButton) {
        dismiss(animated: true, completion: nil) // 화면 내리기
    }
    
    /*****Keyboard 내리기 Function*****/
    @objc func didEndOnExit(_ sender: UITextField) {
        if usernameTextField.isFirstResponder {
            passwordTextField.becomeFirstResponder()
        }else if passwordTextField.isFirstResponder {
            passwordConformTextField.becomeFirstResponder()
        }
    }
    
}
{% endhighlight %}