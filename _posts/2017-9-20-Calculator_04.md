---
layout: post
title: "Enum, Closure 활용한 계산기 만들기"
author: "younari"
---

## MVC 모델 적용 & Enum, Closure 활용
- **1.** 연산 모델 Class 만들기 (CalculatorModel)
- **2.** Operation Case에 대한 Enum 만들기 (단일 연산, 이항 연산, = 연산)
- **3.** 연산 기호 : Operation Case 딕셔너리 만들기
- **4.** Enum의 Associated Value에 클로저가 들어감

### 연산 처리를 담당하는 클래스 (모델)
{% highlight swift %}
class CalculatorModel {
    
    // 연산 타입 정의 : 단일, 이항, 등호
    // Associated Value 값에 클로저 대입
    private enum OperatoinCase {
        case unary((Double) -> Double)
        case binary((Double,Double) -> Double)
        case equal
    }

    
    // 연산 기호 : 연산 타입 딕셔너리 생성
    private var operDic: [String:OperatoinCase] = [
        "+": .binary({(num1, num2) -> Double in return num1 + num2}),
        "-": .binary({(num1, num2) -> Double in return num1 - num2}),
        "*": .binary({(num1, num2) -> Double in return num1 * num2}),
        "/": .binary({(num1, num2) -> Double in return num1 / num2}),
        "cos": .unary(cos),
        "√": .unary(sqrt),
        "±": .unary({(num1: Double) -> Double in return -num1}),
        "=": .equal
    ]

    
    // 연산을 기다리는 옵셔널 Double 변수 생성
    var waitingNum: Double?
    
    // 연산 기호 클릭 시 waitingNum에 디스플레이 값 대입
    func setNumber(displayNum: Double){
        waitingNum = displayNum
    }
    
    // 바이너리 연산을 위한 스트럭트 WaitingBinary의 인스턴스
    private var waitingBinary: WaitingBinary?
    
    // 연산 클릭시 연산 수행
    func perfomrOperation(mathSymbol: String) {
        if let operCase = operDic[mathSymbol] {
            switch operCase {
            case .unary(let function):
                if waitingNum != nil {
                    waitingNum = function(waitingNum!)
                }
            case .binary(let binaryFunction):
            if waitingNum != nil {
                getResult()
                waitingBinary = WaitingBinary(firstNum: waitingNum!, waitingFunc: binaryFunction)
                }
            case .equal:
                getResult()
            }
    	}
    }
    
    // 바이너리 연산 값 구하기
    private func getResult() {
        if waitingBinary != nil && waitingNum != nil {
            waitingNum = waitingBinary?.doBinaryOp(with: waitingNum!)
        }
    }
    
    // 바이너리 값 저장하고 있을 스트럭트
    private struct WaitingBinary {
        let firstNum: Double
        let waitingFunc: (Double,Double) -> Double
        
        func doBinaryOp(with secondNum: Double) -> Double {
            return waitingFunc(firstNum, secondNum)
        }
    }
    
    // 뷰콘트롤러에 넘겨줄 연산 완료된 값
    var returnValue: Double? {
        return waitingNum
    }
    
    
}
{% endhighlight %}

### 뷰 콘트롤러
{% highlight swift %}
class UpgradeCalculatorViewControler: UIViewController {

    //MARK: - UI Property
    @IBOutlet weak var displayLB: UILabel!

    //MARK: - IBAction
    //숫자 입력받기 OK
    var isTyping: Bool = false
    @IBAction func digit(_ sender: UIButton) {
        let currentDigit = sender.currentTitle!
        
        if isTyping {
            let textCurrentlyInDisplay = displayLB!.text!
            displayLB!.text = textCurrentlyInDisplay + currentDigit
        }else {
            displayLB!.text = currentDigit
            isTyping = true
        }
    }
    
    // 리셋
    @IBAction func resetHandler(_ sender: UIButton) {
        isTyping = false
        displayValue = 0
    }
    
    // 여기저기 왔다갔다 할 디스플레이 값
    var displayValue: Double {
        get {
            return Double(displayLB.text!)!
        }
        set {
        	displayLB.text! = String(newValue)
        }
    }
    
    // 연산 모델 호출!
    var calModel = CalculatorModel()

    // operation 버튼을 눌렀을 때 생기는 일
    @IBAction func operation(_ sender: UIButton) {
        isTyping = false
        calModel.setNumber(displayNum: displayValue)
        
        guard let symbol = sender.currentTitle else { return }
        calModel.perfomrOperation(mathSymbol: symbol)
        
        if calModel.returnValue != nil {
            displayValue = calModel.returnValue!
        }
    }

}
{% endhighlight %}