---
layout: post
title: "스위프트 UI 구현하기"
author: "younari"
---

# 01. UITextField, layer
- .borderWidth = 1/UIScreen.main.scale

{% highlight swift %}
func designTextField(_ textField: UITextField) {
    textField.layer.cornerRadius = 3
    textField.layer.borderColor = UIColor.lightGray.cgColor
    textField.layer.borderWidth = 1/UIScreen.main.scale // 딱 1px
}
{% endhighlight %}


# 02. UILabel
- frame, textColor, text, font, textAlignment

{% highlight swift %}
exploreLabel.frame = CGRect(x: 0, y: 160, width: self.view.frame.size.width, height: 60)
exploreLabel.textColor = .white
exploreLabel.text = "Explore"
exploreLabel.font = UIFont.systemFont(ofSize: 46, weight: .heavy)
exploreLabel.textAlignment = .center
{% endhighlight %}

# 03. UIImageView
- alpha, frame, contentMode

{% highlight swift %}
exploreImg.image = #imageLiteral(resourceName: "Background-1")
exploreImg.alpha = 0.6
exploreImg.frame = CGRect(x: 0, y: 0, width: self.view.frame.size.width, height: self.view.frame.size.height)
exploreImg.contentMode = .scaleToFill
{% endhighlight %}


# 04. UIButton
- setTitle, setTitleColor, .titleLabel?.font, .addTarget

{% highlight swift %}
exitIcon = UIButton(frame: CGRect(x: self.view.frame.size.width/2-15, y: self.view.frame.size.height-46, width: 30, height: 30))
exitIcon.setTitle("Exit", for: .normal)
exitIcon.setTitleColor(.white, for: .normal)
exitIcon.titleLabel?.font = UIFont.systemFont(ofSize: 12)
exitIcon.addTarget(self, action: #selector(didTapExitButton), for: .touchUpInside)

// addTarget에 추가할 함수 : 화면 내리기
@objc func didTapExitButton(_ sender: UIButton) {
	dismiss(animated: true, completion: nil)
}
{% endhighlight %}


# 05. UIView.animate

{% highlight swift %}
UIView.animate(withDuration: 0.1, animations: {
        self.usernameTextField.frame.origin.x -= 10
        self.passwordTextField.frame.origin.x -= 10
    }, completion: { _ in
        UIView.animate(withDuration: 0.1, animations: {
            self.usernameTextField.frame.origin.x += 20
            self.passwordTextField.frame.origin.x += 20
        }, completion: { _ in
            UIView.animate(withDuration: 0.1, animations: {
                self.usernameTextField.frame.origin.x -= 10
                self.passwordTextField.frame.origin.x -= 10
            })
        })
    })
}
{% endhighlight %}