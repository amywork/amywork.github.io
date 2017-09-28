---
layout: post
title: "로그인 화면 구현하기 v0.2"
author: "younari"
---

> SignUp & SignIn - 로그인 화면 구현하기 v0.2 (UserDefaults를 활용한 데이터 저장)

# SignUp & SignIn & UserDefaults
- [Xcode Project 바로가기](https://github.com/younari/tastySwift/tree/master/0927_LoginUserDefault/0927_SignUp)
- [로그인 화면 구현하기 v0.1 바로가기](https://younari.github.io/2017-09-28/SignUpSignInV01)


## 회원가입시 UserDefaults를 통해 plist에 유저 데이터 저장하기
- 싱글톤 방식
- userList는 dictionary를 담고 있는 배열
- `var userList: [[String: String]]`
- userData는 ID와 PWD를 담고있는 dictionary
- `let userData: [String:String] = ["ID":username, "PWD":password]`
- userList에 userData 딕셔너리 append
- `userList.append(userData)`
- UserDefaults에 userList배열 저장
- `UserDefaults.standard.set(userList, forKey: "UserList")`


{% highlight swift %}
@IBAction func signUpBtn(_ sender: UIButton) {
    guard let username = username.text else { return }
    guard let password = password.text else { return }
    guard let conformPwd = conformPwd.text else { return }
    if password == conformPwd {
        // Plist에 파일 넣기 (영구 저장)
        // userList는 dictionary를 담고 있는 배열
        var userList: [[String: String]]
        if let tempList = UserDefaults.standard.array(forKey: "UserList") as? [[String:String]] {
            userList = tempList
        }else {
            userList = []
        }
    
        // userData는 ID와 PWD를 담고있는 dictionary
        let userData: [String:String] = ["ID":username, "PWD":password]
        userList.append(userData)
        UserDefaults.standard.set(userList, forKey: "UserList")
        
        
        // Alert
        let signUpSuccessAlert = UIAlertController(title: "SignUp", message: "회원가입이 완료되었습니다😉", preferredStyle: .alert)
        let okAction = UIAlertAction(title: "OK", style: .default, handler: {
            (alert) in let signInViewController = SignInViewController()
            self.present(signInViewController, animated: true, completion: nil)}
        )
        
        signUpSuccessAlert.addAction(okAction)
        
        self.present(signUpSuccessAlert, animated: true, completion: nil)
    }else {
        // Alert
        let signUpFailedAlert = UIAlertController(title: "SignUp", message: "패스워드가 일치하지 않습니다☹️", preferredStyle: .alert)
        let cancelAction = UIAlertAction(title: "닫기", style: .default, handler: nil)
        signUpFailedAlert.addAction(cancelAction)
        self.present(signUpFailedAlert, animated: true, completion: nil)
    }
}
{% endhighlight %}

## 로그인시 UserDefaults에서 userData 가져와서 회원정보 맵핑하기
- `UserDefaults.standard.object(forKey: "UserList")`

{% highlight swift %}
func findUser(name: String, password: String) -> Bool {
    guard let userList: [[String:String]] = UserDefaults.standard.array(forKey: "UserList") as? [[String : String]] else { return false }
    UserDefaults.standard.object(forKey: "UserList")
    for userData in userList {
        let memberID: String = userData["ID"]!
        let memberPWD: String = userData["PWD"]!
        if name == memberID && password == memberPWD {
            return true
        }
    }
    return false
}
{% endhighlight %}