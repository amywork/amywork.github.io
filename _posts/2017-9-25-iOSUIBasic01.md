---
layout: post
title: "iOS UI Basic v0.1"
author: "younari"
---

# iOS User Interface Overview
- [연습용 Xcode Project 바로가기](https://github.com/younari/tastySwift/tree/master/0925_UIViewPractice)
- [Apple SDK 문서 읽기](https://developer.apple.com/documentation/uikit/uilabel)

## 00. Point, Pixel
- Xcode에서 개발 시 사용하는 단위는 모두 **point**이다. 실제 디자이너에게 전달 받을 이미지는 포인트 기준으로 1x, 2x, 3x **pixel**로 받아야 한다.

## 01. Frame Base
- 좌측 상단 0,0 을 기준으로, x,y 좌표를 산정하는 것
- **x, y, width, Height**
- 뷰의 위치는 **상위뷰**를 기준으로 위치를 잡는다. (상위 Layer 기준)

## 02. Auto Layout
- 추후 설명 추가 예정

# Framework
- 특정 운영 체제를 위한 응용 프로그램 표준 구조를 구현 하는 **클래스와 라이브러리 모임**이다. 

# UIKit framework
- Command + Shift + 0 으로 찾기
- **Cocoa Touch Framework**
- **Ex)** `import UIKit`
- **UI Kit** : Cocoa Touch Framework에 추가된 UI관련 기능의 클래스가 모여있는 Framework
- **UI Class Hierarchy** : NSObject 👉🏻 UIResponder 👉🏻 UIApplication, UIViewController, UIView 👉🏻 UIImageView, UILabel, **UIControl**, UIWindow, UIScrollView 👉🏻 UIButton, UISlider, UISwitch, UITextField


## UIResponder
- The UIResponder class defines an interface for objects that respond to handle events.
- ex) `open func becomeFirstResponder() -> Bool`

## UIApplication
## UIViewController
## UIView
- An object that manages the content for a rectangular area on the screen.

{% highlight swift %}
let topView: UIView = UIView(frame: CGRect(x: 15, y: 15, width: self.view.frame.width-30, height: 100))
{% endhighlight %}

- UIView class의 property 중의 하나인, frame(=> CGRect 타입) 에 값을 넣어서 init하는 방식

#### - CGRect?
- **struct CGRect**: A structure that contains the location and dimensions of a rectangle. (CG = Core Graphic)
- In the default **Core Graphics** coordinate space, the origin is located in the lower-left corner of the rectangle and the rectangle extends towards the upper-right corner. If the context has a flipped-coordinate space—often the case on iOS—the origin is in the upper-left corner and the rectangle extends towards the lower-right corner.

{% highlight swift %}
let topView: UIView = UIView(frame: CGRect(x: 15, y: 15, width: self.view.frame.width-30, height: 100))

topView.frame = CGRect(origin: CGPoint(x: 30, y: 30), size: CGSize(width: 100, height: 100))
topView.backgroundColor = UIColor.yellow
topView.layer.borderWidth = 8
topView.layer.borderColor = UIColor.black.cgColor
//UIColor라는 class 안에 black이라는 프로퍼티가 있고, cgColor로 변환
topView.layer.cornerRadius = 20
self.view.addSubview(topView)
{% endhighlight %}

## UILabel
- A view that displays one or more lines of read-only text, often used in conjunction with controls to describe their intended purpose.
- 라벨 만들기 실습 (UIColor, UIFont 활용) 

{% highlight swift %}
// MARK -- Generate Kinfolk Logo
logoLabel = UILabel(frame: CGRect(x: 16, y: 36, width: self.view.frame.width-32, height: 20))

// MARK -- addSubview의 위치는 상관 없으나, 그래도 변수 선언하고 바로 추가하기
gnbView.addSubview(logoLabel)
    
logoLabel.text = "Kinfolk"
logoLabel.textAlignment = .left
logoLabel.font = UIFont.systemFont(ofSize: 16) 
logoLabel.textColor = UIColor.darkGray
logoLabel.shadowColor = UIColor.darkGray
        

// MARK -- UIFont.Weight : ultraLight, thin, light, regular, medium, semibold, bold, heavy, black
logoLabel.font = UIFont.boldSystemFont(ofSize: 16)
logoLabel.font = UIFont.systemFont(ofSize: 16, weight: UIFont.Weight.heavy)

// MARK -- Custom Font type
logoLabel.font = UIFont(name: "Font Name", size: 16)
{% endhighlight %}
        

## UIImageView
- An object that displays a single image or a sequence of animated images in your interface.

{% highlight swift %}
let contentImageView: UIImageView = UIImageView()
contentModuleView.addSubview(contentImageView)
contentImageView.frame = CGRect(x: 8, y: 8, width: 359, height: 359)
contentImageView.image = UIImage(named: "fritzHansen_04.jpg")
contentImageView.contentMode = UIViewContentMode.scaleAspectFill // contentMode (UIView의 속성)
        
// MARK -- image가 isHighlighted 
contentImageView.isHighlighted = true
contentImageView.highlightedImage = UIImage(named: "fritzHansen_02.jpg")
contentImageView.isUserInteractionEnabled = true
{% endhighlight %}        


## UIControl
- normal, highlighted, isEnabled, disabled(read only), isSelected, selected(read only), addTarget(method)
- Button, Switch, Slider, Textview 의 상위 클래스

## UIButton
- 사용자의 이벤트를 받아 처리해주는 UI
- currentImage, currentBackgroundImage
- currentAttributedTitle. currentTitleColor

### - UIButton의 자주 사용하는 프로퍼티 살펴보기
- setTitle, setTitleColor, addTarget

{% highlight swift %}
let btn: UIButton = UIButton(type: .custom) // Default: Custom
view.addSubview(btn)
btn.setTitle("normal", for: .normal) 
btn.setTitleColor(.black, for: .normal)
btn.setTitle("highlight", for: .highlighted)
btn.setTitleColor(.black, for: .highlighted)//
btn.setTitle("selected", for: .selected) 
btn.setTitleColor(.black, for: .selected)
btn.addTarget(self, action: #selector(ViewController.btnClick), for: .touchUpInside) 
btn.backgroundColor = .white
btn.frame = CGRect(x: magazineFooterView.frame.width/2-50, y: magazineFooterView.frame.height-50, width: 100, height: 20)
{% endhighlight %}



### - sender.isSelected
- btn의 isSelected값에 변화주기

{% highlight swift %}
@objc func btnClick(_ sender: UIButton) {
    if sender.isSelected {
        sender.isSelected = false
    }else {
        sender.isSelected = true
    }
}
{% endhighlight %}


## UI Text Field
- 사용자의 input을 받는 UI Text Field

{% highlight swift %}
let sendMsgTxtField = UITextField(frame: CGRect(x: 0, y: view.frame.size.height-50, width: view.frame.size.width, height: 50 ))
self.view.addSubview(sendMsgTxtField)
sendMsgTxtField.borderStyle = .line
sendMsgTxtField.backgroundColor = #colorLiteral(red: 1, green: 1, blue: 1, alpha: 1)
sendMsgTxtField.textColor = #colorLiteral(red: 0, green: 0, blue: 0, alpha: 1)
sendMsgTxtField.placeholder = "Kinfolk에게 의견을 보내주세요😊"
/****UITextfield Protocol 채택 및 delegate 사용*****/
sendMsgTxtField.delegate = self
{% endhighlight %}
