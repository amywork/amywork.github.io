---
layout: post
title: "스위프트 Alert Controller"
author: "younari"
---

# UI Alert Controller
- [Xcode 스위프트 프로젝트 바로가기](https://github.com/younari/tastySwift)

01. UIAlertController의 인스턴스를 만든다.
02. UIAlertController에 추가할 Action인 UIAlertAction을 만든다.
03. UIAlertController에 UIAlertAction을 추가한다.
04. UIAlertController를 self(뷰콘트롤러)에 최종적으로 추가한다.

- `let alertController = UIAlertController(title: "로그인 성공", message: "로그인 성공하였습니다 ☺️", preferredStyle: .alert) // .actionSheet`
- `let cancelAction = UIAlertAction(title: "Cancel", style: .cancel, handler: nil)`
- `alertController.addAction(cancelAction)`
- `self.present(alertController, animated: true, completion: nil)`

{% highlight swift %}
if loginSuccess {
    
    // 01. UIAlertController의 인스턴스를 만든다.
    let alertController = UIAlertController(title: "로그인 성공", message: "로그인 성공하였습니다 ☺️", preferredStyle: .alert) // .actionSheet
    
    // 02. UIAlertController에 추가할 Action인 UIAlertAction을 만든다.
    let okAction = UIAlertAction(title: "OK", style: .default, handler: { (input: UIAlertAction) -> Void in
        let main = MainViewController()
        self.present(main, animated: true, completion: nil)
    })
    let cancelAction = UIAlertAction(title: "Cancel", style: .cancel, handler: nil)
    
    // 03. UIAlertController에 UIAlertAction을 추가한다.
    alertController.addAction(okAction)
    alertController.addAction(cancelAction)
    
    // 04. UIAlertController를 self(뷰콘트롤러)에 최종적으로 추가한다.
    self.present(alertController, animated: true, completion: nil)
 
}
{% endhighlight %}

# 샘플 코드

{% highlight swift %}
// Alert
if password == conformPwd {
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
{% endhighlight %}