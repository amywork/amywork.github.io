---
layout: post
title: "Swift 계산기 만들기 v0.2"
author: "Amy"
---

### Swift로 계산기 만들기, 2차 시도(작업 시간: 약 1시간)
### - 중복 조건문 함수화, 변수 최소화
#### 코드 구성도
- 배열 변수 제거
- 값을 저장할 Int형으로 prev, current 변수 선언
- 계산기 Label에 전시할 displayText를 String 변수로 선언
- 연산 기호 값을 판단할 prevSign 값을 String 변수로 선언

#### 코드 작성
{% highlight swift %}

class ViewController: UIViewController {
    
    @IBOutlet var displayLabel: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        displayLabel.text = "0"
    }
    
    var displayText: String = "0"
    
    var prevSign: String = "+"
    
    var prev: Int = 0
    var current: Int = 0
    
    // 전시(displayLabel.text)될 숫자 및 displayText 정의
    @IBAction func btnNumberClick(btn: UIButton) {
        if displayLabel.text == "0" {
            displayText = (btn.titleLabel?.text)!
        }else {
            displayText += (btn.titleLabel!.text)!
        }
        displayLabel.text = displayText
    }
    
    // 리셋 기능 정의
    @IBAction func btnResetClick(btn: UIButton) {
        displayText = "0"
        displayLabel.text = displayText
        prev = 0
        current = 0
    }
    
    // 연산 기능 정의
    @IBAction func operation(sign: UIButton) {
        if displayText != "" {
            if sign.titleLabel!.text! == "+" {
                prevOperation()
            }else if sign.titleLabel!.text! == "-" {
                prevOperation()
            }else if sign.titleLabel!.text! == "x" {
                prevOperation()
            }else if sign.titleLabel!.text! == "÷" {
                prevOperation()
            }else if sign.titleLabel!.text! == "=" {
                prevOperation()
                displayLabel.text = String(prev)
                prev = 0
            }
        prevSign = sign.titleLabel!.text!
        displayText = ""
        }
    }
    
    func prevOperation() {
        if prev == 0 {
            prev = Int(displayText)!
        }else {
            current = Int(displayText)!
            if prevSign == "+" {
                prev += current
                displayLabel.text = String(prev)
            }else if prevSign == "-" {
                prev -= current
                displayLabel.text = String(prev)
            }else if prevSign == "x" {
                prev *= current
                displayLabel.text = String(prev)
            }else if prevSign == "÷" {
                prev /= current
                displayLabel.text = String(prev)
            }
        }
    }
}



{% endhighlight %}
