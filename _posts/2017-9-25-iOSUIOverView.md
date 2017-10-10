---
layout: post
title: "iOS UI Overview"
author: "younari"
---

# iOS User Interface Overview
- [연습용 Xcode Project 바로가기](https://github.com/younari/tastySwift/tree/master/0925_UIViewPractice)
- [Apple SDK 문서 읽기](https://developer.apple.com/documentation/uikit/uilabel)

### Point, Pixel
- Xcode에서 개발 시 사용하는 단위는 모두 **point**이다. 실제 디자이너에게 전달 받을 이미지는 포인트 기준으로 1x, 2x, 3x **pixel**로 받아야 한다.

### Frame Base
- 좌측 상단 0,0 을 기준으로, x,y 좌표를 산정하는 것
- **x, y, width, Height**
- 뷰의 위치는 **상위뷰**를 기준으로 위치를 잡는다. (상위 Layer 기준)

### Auto Layout
- 추후 설명 추가 예정

# Framework
- 특정 운영 체제를 위한 응용 프로그램 표준 구조를 구현 하는 **클래스와 라이브러리 모임**이다. 

### UIKit framework
- Command + Shift + 0 으로 찾기
- **Cocoa Touch Framework**
- **Ex)** `import UIKit`
- **UI Kit** : Cocoa Touch Framework에 추가된 UI관련 기능의 클래스가 모여있는 Framework
- **UI Class Hierarchy** : NSObject 👉🏻 UIResponder 👉🏻 UIApplication, UIViewController, UIView 👉🏻 UIImageView, UILabel, **UIControl**, UIWindow, UIScrollView 👉🏻 UIButton, UISlider, UISwitch, UITextField

# UIResponder
- The UIResponder class defines an interface for objects that respond to handle events.
- ex) `open func becomeFirstResponder() -> Bool`

# UIApplication
- App Launching 후 가장 먼저 만들어지는 것

# AppDelegate
- UIApplication에 의해 AppDelegate의 인스턴스 생성, UIApplication의 상태 변화 관리
- `func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool`
- `func applicationWillResignActive(_ application: UIApplication)`
- `func applicationDidEnterBackground(_ application: UIApplication)`
- `func applicationWillEnterForeground(_ application: UIApplication)`
- `func applicationDidBecomeActive(_ application: UIApplication)`
- `func applicationWillTerminate(_ application: UIApplication)`

# UIViewController
- 모든 앱은 적어도 한개 이상의 UIViewController를 가지고 있으며, 대부분의 앱은 여러개의 UIViewController로 이뤄져 있다.
- UIViewController는 사용자의 인터렉션과 앱의 데이터 사이에서 컨트롤의 역할을 한다.
- UIViewController는 **모든 View의 관리, 사용자 이벤트 핸들링, UIViewController간의 전환** 등의 역할을 수행한다.
- 모든 UIViewController는 한개의 RootView를 필수적으로 가지고 있으며, 화면에 표시되는 모든 View는 RootView의 SubView로 존재한다.

# UIView
- An object that manages the content for a rectangular area on the screen.
- 화면에 표시되는 모든 View는 RootView의 SubView로 존재한다.
- UIView class의 property 중의 하나인, frame(=> CGRect 타입)에 값을 넣어서 init하는 방식이 있다.

{% highlight swift %}
let topView: UIView = UIView(frame: CGRect(x: 15, y: 15, width: self.view.frame.width-30, height: 100))
{% endhighlight %}


## 참고. CGRect?
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


# UILabel
- A view that displays one or more lines of read-only text, often used in conjunction with controls to describe their intended purpose.
- frame, textColor, text, font, textAlignment
- 라벨 만들기 실습 (UIColor, UIFont 활용) 

{% highlight swift %}
exploreLabel.frame = CGRect(x: 0, y: 160, width: self.view.frame.size.width, height: 60)
exploreLabel.textColor = .white
exploreLabel.text = "Explore"
exploreLabel.font = UIFont.systemFont(ofSize: 46, weight: .heavy)
exploreLabel.textAlignment = .center
{% endhighlight %}

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


### Attributed Text (swift4)
{% highlight swift %}
import UIKit
class ViewController: UIViewController {

    @IBOutlet weak var label: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        updateUI()
    }
 
    func updateUI() {
        
        let text = "Hello World"

        let attributes = [
            NSAttributedStringKey.foregroundColor: UIColor.black,
            NSAttributedStringKey.underlineStyle: 1,
            NSAttributedStringKey.strokeWidth: 2,
            NSAttributedStringKey.kern: 4
            ] as [NSAttributedStringKey : Any]

        let attStr = NSAttributedString(string: text, attributes: attributes)
        label.attributedText = attStr
        label.font = UIFont.systemFont(ofSize: 20)
    }

}
{% endhighlight %}



# UIImageView
- An object that displays a single image or a sequence of animated images in your interface.
- alpha, frame, contentMode

{% highlight swift %}
exploreImg.image = #imageLiteral(resourceName: "Background-1")
exploreImg.alpha = 0.6
exploreImg.frame = CGRect(x: 0, y: 0, width: self.view.frame.size.width, height: self.view.frame.size.height)
exploreImg.contentMode = .scaleToFill
{% endhighlight %}

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


# UIControl
- normal, highlighted, isEnabled, disabled(read only), isSelected, selected(read only), addTarget(method)
- Button, Switch, Slider, Textview 의 상위 클래스


# UIButton
- 사용자의 이벤트를 받아 처리해주는 UI
- setTitle, setTitleColor, .titleLabel?.font, .addTarget
- currentImage, currentBackgroundImage
- currentAttributedTitle
- currentTitleColor

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


# UI Text Field
- 사용자의 input을 받는 UI Text Field
- 프로토콜 채택 가능: UITextFieldDelegate, UITextInputTraits protocol
- Controlling the appearance of the keyboard
- `var isSecureTextEntry: Bool`
- `var keyboardType: UIKeyboardType`
- Keyboards can have accessory views that appear above the keyboard (ex. custom toolbar)
- `var inputAccessoryView: UIView`

#### Textfields property
- `var clearsOnBeginEditing: Bool`
- `var adjustFontSizeToFitWidth: Bool`
- `var minimumFontSize: CGFloat`
- `var placeholder: String?`
- `var defaultTextAtrrtibutes: [String:Any]`

{% highlight swift %}
// UITextField 는 1줄 입력이 default
sendMsgTxtField = UITextField(frame: CGRect(x: 0, y: view.frame.size.height-50, width: view.frame.size.width, height: 50 ))
self.view.addSubview(sendMsgTxtField)
sendMsgTxtField.borderStyle = .line
sendMsgTxtField.backgroundColor = #colorLiteral(red: 1, green: 1, blue: 1, alpha: 1)
sendMsgTxtField.textColor = #colorLiteral(red: 0, green: 0, blue: 0, alpha: 1)
sendMsgTxtField.placeholder = "Kinfolk에게 의견을 보내주세요😊"

/****UITextfield Protocol 채택 및 delegate 사용*****/
sendMsgTxtField.delegate = self
sendMsgTxtField.adjustsFontSizeToFitWidth = true
sendMsgTxtField.minimumFontSize = 6
sendMsgTxtField.clearsOnBeginEditing = true
{% endhighlight %}

#### UITextField.layer
- .borderWidth = 1/UIScreen.main.scale

{% highlight swift %}
func designTextField(_ textField: UITextField) {
    textField.layer.cornerRadius = 3
    textField.layer.borderColor = UIColor.lightGray.cgColor
    textField.layer.borderWidth = 1/UIScreen.main.scale // 딱 1px
}
{% endhighlight %}


### 참고. UIView에 animate 효과 주기

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