---
layout: post
title: "Swift 계산기 만들기 v0.1"
author: "Amy"
---

### Swift로 계산기 만들기, 1차 시도(작업 시간: 약 3시간)
#### 코드 구성도
- @IBOutlet 결과값이 출력될 displayLabel 선언
- @IBAction 숫자 버튼(btnNumberClick) 클릭 액션 정의
- @IBAction 연산기호 버튼(operation) 클릭 액션 정의
- btnNumberClick때마다 displayText에 btn.titleLabel.text 추가
- 숫자를 담아둘 numbers 배열 선언
- 클릭되었던 연산기호를 분간할 수 있는 signs 변수 선언
- 출력 값을 임시 저장해둘 tmp 변수 선언
- 연산시 중복 사용되는 부분 calculate 함수로 따로 분리
- @IBAction 리셋버튼(btnResetClick) 클릭 액션 정의 

#### 코드 작성
{% highlight swift %}
class ViewController: UIViewController {
    
    @IBOutlet var displayLabel: UILabel!

    override func viewDidLoad() {
        super.viewDidLoad()
        displayLabel.text = "0"
    }

    var displayText: String = "0"
    var numbers: [Int] = []
    var signs: Int = 1
    var tmp: Int = 0
    

    // 입력 받은 숫자를 displayText에 저장하며 라벨에 출력
    @IBAction func btnNumberClick(btn: UIButton) {
        
        if displayLabel.text == "0" {
            displayText = (btn.titleLabel?.text)!
        }else {
           displayText += (btn.titleLabel!.text)!
        }
        
        displayLabel.text = displayText
    }


    // 연산시 중복 사용되는 부분 calculate 함수로 따로 분리
    func calculate() {
        numbers.append(Int(displayText)!)
        if signs == 1 {
            tmp += numbers.last!
        }else if signs == 2 {
            tmp -= numbers.last!
        }else if signs == 3 {
            tmp *= numbers.last!
        }else if signs == 4 {
            tmp /= numbers.last!
        }
        displayLabel.text = String(tmp)
    }
    
    
    // 연산 기호 클릭시 조건문 분기
    @IBAction func operation(sign: UIButton) {
        
        // displayText 문자열이 비어있다는 것은 연산기호가 중복으로 눌린 상태이므로 건너뜀
        if displayText == "" {

        }else {
            
            // + 연산기호를 눌렀을 경우
            if sign.titleLabel!.text == "+" {
                
                // numbers 배열이 비어있다는 것은 첫번째 연산이라는 뜻
                if numbers.isEmpty {
                    numbers.append(Int(displayText)!)
                    tmp = numbers.last!
                    signs = 1
                // numbers 배열이 비어있지 않다는 것은 연산중이라는 뜻
                }else {
                    calculate()
                }
                
                // 연산 종료 후 signs 값 재할당
                // 연산 종료 후 displayText 초기화
                signs = 1
                displayText = ""
                
            }else if sign.titleLabel!.text == "-" {
                
                if numbers.isEmpty {
                    numbers.append(Int(displayText)!)
                    tmp = numbers.last!
                    signs = 2
                }else {
                    calculate()
                }
                
                displayLabel.text = String(tmp)
                signs = 2
                displayText = ""
                
            }else if sign.titleLabel!.text == "x" {
                
                if numbers.isEmpty {
                    numbers.append(Int(displayText)!)
                    tmp = numbers.last!
                    signs = 3
                }else {
                    calculate()
                }
                
                displayLabel.text = String(tmp)
                signs = 3
                displayText = ""
                
            }else if sign.titleLabel!.text == "÷" {
                
                if numbers.isEmpty {
                    numbers.append(Int(displayText)!)
                    tmp = numbers.last!
                    signs = 4
                }else {
                    calculate()
                }
                
                displayLabel.text = String(tmp)
                signs = 4
                displayText = ""
                
            }else if sign.titleLabel!.text == "=" {
                
                calculate()
                displayLabel.text = String(tmp)
                numbers = []
                tmp = 0
                displayText = ""
            }
            
        }
        
    }
    
    
    @IBAction func btnResetClick(btn: UIButton) {
        displayText = "0"
        displayLabel.text = displayText
    }
    
}

{% endhighlight %}
