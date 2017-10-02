---
layout: post
title: "버튼 인터렉션 만들어보기"
author: "younari"
---

> Xcode ViewController에서, UIView와 UIButton을 활용하여 버튼을 만들고, 버튼에 색상을 입혀 On/Off 하며 색을 바꾸는 인터렉션을 만들어봅니다.

# UIButton
- [연습용 Xcode Project 바로가기](https://github.com/younari/tastySwift/tree/master/0926_ButtonPractice)
- UIButton의 type은 init 할 때만 지정할 수 있다.
- type default는 커스텀 타입이다.
- buttonType은 get만 있는 read Only 읽기 전용이다.
- UIControl은 상태(normal, selected)가 존재하기 때문에, 버튼의 타이틀을 입력할 때 (setTitle) 상태도 함께 입력해야 한다.

### 01. 버튼 9개를 담을 [UIButton] 배열을 만든다.
### 02. 버튼을 만드는 함수 makeBtn()을 만든다.
- 0..<9 번 반복하며, UIButton의 instance를 만든다.
- tag, backgroundColor, borderColor, borderWidth를 부여한다.
- main view에 꼭 addView 시키고, btnArr(배열)에 append 시킨다.

### 03. 버튼의 Layout을 잡아주는 함수를 만든다.
- **makeBtnLayout(btnArr: [UIButton])**
- btnArr에 있는 btn(UI버튼)들에 대해, for문을 돌리며 layout을 잡는다.
- btn.tag를 통해 row와 colunm 값을 부여하고 (3으로 나눈 몫과 나머지 활용)
- btm.frame을 통해 형상을 부여한다.

{% highlight swift %}
var btnArr: [UIButton] = []
    
func makeBtn() {
    for index: Int in 0..<9 {
        let btn = UIButton()
        btn.tag = index
        self.view.addSubview(btn)
        btn.backgroundColor =  #colorLiteral(red: 1, green: 0.4932718873, blue: 0.4739984274, alpha: 1)
        btn.layer.borderColor = UIColor.black.cgColor // borderColor = CGcolor
        btn.layer.borderWidth = 1
        btnArr.append(btn)
    }
}
    
func makeBtnLayout(btnArr: [UIButton]) {
    for btn in btnArr {
        let index = btn.tag
        let row = index / 3 // 0, 0, 0, 1, 1, 1, 2, 2, 2
        let column = index % 3 // 0, 1, 2, 0, 1, 2, 0, 1, 2
        
        let width = self.view.frame.size.width/3
        let height = self.view.frame.size.height/3
        btn.frame = CGRect(x: CGFloat(column)*width, y: CGFloat(row)*height, width: width, height: height)
        btn.addTarget(self, action: #selector(ViewController.switchColor), for: .touchUpInside)
    }
}
{% endhighlight %}



### 04. addTarget: SwitchColor Function
- Enum Case를 ON/OFF 분기
- tag가 홀수인지 짝수인지 여부에 따라 색상 전환 기능 추가

{% highlight swift %}
var tileCase = OnOff.on
    enum OnOff {
        case on
        case off
    }
    
    func changeColorA(_ sender: UIButton) {
        let tag = sender.tag
        if isEvenNumber(num: tag) {
            sender.backgroundColor = #colorLiteral(red: 0.2392156869, green: 0.6745098233, blue: 0.9686274529, alpha: 1)
        }else {
            sender.backgroundColor = #colorLiteral(red: 1, green: 0.8193424344, blue: 0, alpha: 1)
        }
    }
    
    func changeColorB(_ sender: UIButton) {
        let tag = sender.tag
        if isEvenNumber(num: tag) {
            sender.backgroundColor = #colorLiteral(red: 1, green: 0, blue: 0, alpha: 1)
        }else {
            sender.backgroundColor = #colorLiteral(red: 0.9818221927, green: 0.1470750272, blue: 1, alpha: 1)
        }
    }

    @objc func switchColor(_ sender: UIButton) {
        for btn in btnArr {
            switch tileCase {
            case .on:
                changeColorA(btn)
                tileCase = .off
            case .off:
                changeColorB(btn)
                tileCase = .on
            }
        }
    }
    
    func isEvenNumber(num: Int) -> Bool {
        if num%2 == 0 {
            return true
        }else {
            return false
        }
    }
{% endhighlight %}




### 05. ViewDidLoad() 이후 메소드 호출

{% highlight swift %}
 override func viewDidLoad() {
        super.viewDidLoad()
        makeBtn()
        makeBtnLayout(btnArr: btnArr)
    }
{% endhighlight %}
