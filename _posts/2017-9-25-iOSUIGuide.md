---
layout: post
title: "iOS UI Overview"
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
        logoLabel.font = UIFont.systemFont(ofSize: 16) // UIFont.font는 Instance
        logoLabel.textColor = UIColor.darkGray // logoLabel.shadowColor = UIColor.darkGray
        
		// Q.MARK -- font Weight
        //UIFont.Weight : ultraLight, thin, light, regular, medium, semibold, bold, heavy, black
        logoLabel.font = UIFont.boldSystemFont(ofSize: 16)
        logoLabel.font = UIFont.systemFont(ofSize: 16, weight: UIFont.Weight.heavy)
	    // Q.MARK -- Custom Font type은 어떻게 설정하나요?
		// logoLabel.font = UIFont(name: "Font Name", size: 16)
{% endhighlight %}


{% highlight swift %}
// MARK -- Generate Magazine Contents Module > ImageView > contentTitleLabel > contentSummaryLabel
        let contentSummaryLabel: UILabel = UILabel()
        contentModuleView.addSubview(contentSummaryLabel)
        contentSummaryLabel.frame = CGRect(x: 16, y: 402, width: self.view.frame.width-32, height: 110)
        contentSummaryLabel.text = "Introducing The Kinfolk Entrepreneur! Our latest book meets over 40 international entrepreneurs who offer tips, advice and inspiration for anyone hoping to forge their own professional path. Available to order at kinfolk.com/entrepreneur. Published by @artisan_books."
        
        //MARK -- UILabel.attributedText : lineBreakMode, textAlignment, lines
        //Q.MARK -- Attributed String : NSAttributedStringKey
        let stringValue = "Introducing The Kinfolk Entrepreneur! Our latest book meets over 40 international entrepreneurs who offer tips, advice and inspiration for anyone hoping to forge their own professional path. Available to order at kinfolk.com/entrepreneur. Published by @artisan_books."
        let attrString = NSMutableAttributedString(string: stringValue)
        let paragraphStyle = NSMutableParagraphStyle()
        paragraphStyle.lineSpacing = 3 // change line spacing between paragraph
        paragraphStyle.minimumLineHeight = 0 // change line spacing between each line
        attrString.addAttribute(NSAttributedStringKey.paragraphStyle, value: paragraphStyle, range: NSRange(location: 0, length: stringValue.characters.count))
        contentSummaryLabel.attributedText = attrString
        
        //Q.MARK -- attrubutedText
        contentSummaryLabel.lineBreakMode = .byTruncatingTail
        // byTruncatingTail is default
        //byCharWrapping, byClipping, byTruncatingHead, byWordWrapping, byTruncatingMiddle
        contentSummaryLabel.allowsDefaultTighteningForTruncation = false // The default is false.
        contentSummaryLabel.textAlignment = .left
        contentSummaryLabel.numberOfLines = 6
        contentSummaryLabel.isEnabled = false // This property determines only how the label is drawn. Disabled text is dimmed somewhat to indicate it is not active. This property is set to true by default.
    
        //Q.MARK -- adjustsFontSizeToFitWidth == AutoShrink
        contentSummaryLabel.adjustsFontSizeToFitWidth = true
        contentSummaryLabel.minimumScaleFactor = 6
{% endhighlight %}

## UIImageView
- An object that displays a single image or a sequence of animated images in your interface.

{% highlight swift %}
 // MARK -- Generate Magazine Contents Module -- ImageView
        let contentImageView: UIImageView = UIImageView()
        contentModuleView.addSubview(contentImageView)
        
        // MARK -- 상위뷰 표현 어떻게? -- 상위뷰의 이름을 쓰기
	    contentImageView.frame = CGRect(x: 8, y: 8, width: 359, height: 359)
        contentImageView.image = UIImage(named: "fritzHansen_04.jpg")  // UIImageView.image (image는 instance)
        contentImageView.contentMode = UIViewContentMode.scaleAspectFill // contentMode (UIView의 속성)
        
        //Q.MARK -- image가 isHighlighted (눌렀을 때)
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
- 버튼 클릭에 따라 로고와 매거진 타이틀의 폰트 및 텍스트 String 바꿔보기
- `contentReadBtn.setTitle("Read More", for: UIControlState.normal)` 
- `contentReadBtn.setTitleColor(UIColor.black, for: UIControlState.normal)`

{% highlight swift %}

//MARK -- Content Read Buttom
        let contentReadBtn: UIButton = UIButton(type: UIButtonType.system)// init 할 때만 type 선정 가능, 기본은 custom type
        contentModuleView.addSubview(contentReadBtn)
        contentReadBtn.setTitle("Read More", for: UIControlState.normal) // 버튼 내용
        contentReadBtn.setTitleColor(UIColor.black, for: UIControlState.normal) // 버튼 내용
        contentReadBtn.backgroundColor = UIColor.white
        contentReadBtn.frame = CGRect(x: contentModuleView.frame.width-116, y: contentModuleView.frame.height-46, width: 100, height: 30)
        contentReadBtn.addTarget(self, action: #selector(ViewController.contentReadBtnClick(_:)), for: UIControlEvents.touchUpInside)
        
        
        let homeBtn: UIButton = UIButton(type: UIButtonType.system)
        gnbView.addSubview(homeBtn)
        homeBtn.setTitle("Home", for: UIControlState.normal)
        homeBtn.setTitleColor(UIColor.white, for: UIControlState.normal)
        homeBtn.backgroundColor = #colorLiteral(red: 1, green: 0.4932718873, blue: 0.4739984274, alpha: 1)
        homeBtn.frame = CGRect(x: gnbView.frame.width-116, y: view.frame.origin.y+36, width: 100, height: 20)
        homeBtn.addTarget(self, action: #selector(ViewController.goHome), for: UIControlEvents.touchUpInside)
        
    }
    
// MARK -- Read Button Click Enum Cases
    var titleCase = titleChangeCase.on
    enum titleChangeCase {
        case on
        case off
    }
    
    @objc func contentReadBtnClick(_ sender: UIButton) {
        switch titleCase {
        case .on:
        contentTitleLabel.text = "Want to read more?"
        titleCase = .off
        case .off:
        contentTitleLabel.text = "Magazine Title"
        titleCase = .on
        }
    }
    
//MARK -- Home Button Click Enum Cases
    var logoSizeCase = LogoSize.on
    enum LogoSize {
        case on
        case off
    }
    
    @objc func goHome(_ sender: UIButton) {
        switch logoSizeCase {
        case .on:
            logoLabel.text = "Kinfolk is Good"
            logoSizeCase = .off
        case .off:
            logoLabel.text = "Kinfolk"
            logoSizeCase = .on
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