---
layout: post
title: "Enum, Closure 활용한 계산기 만들기"
author: "younari"
---

## MVC 모델 적용 및 Enum, Closure 활용
- 연산 모델 Class 만들기 (CalculatorModel)
- Operation Case에 대한 Enum 만들기 (단일 연산, 이항 연산, = 연산)
- 연산 기호 : Operation Case 딕셔너리 만들기
- Enum의 Associated Value에 클로저가 들어가게끔 처리하기

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
    var leftNumber: Double? 
    
    // 연산 기호 클릭 시 leftNum에 디스플레이 값 대입
    func setNumber(displayNum: Double){
        leftNumber = displayNum
    }
    
    // 바이너리 연산을 위한 스트럭트 WaitingBinary의 인스턴스
    private var waitingBinary: WaitingBinary?
    
    // 연산 클릭시 연산 수행, 연속 연산을 위한 축적값 operand 생성
    var operand: Double?
    func perfomrOperation(mathSymbol: String) {
        if let operationCase = operDic[mathSymbol] {
            switch operationCase {
            case .unary(let function):
                if leftNumber != nil {
                    leftNumber = function(leftNumber!)
                }
            case .binary(let binaryFunction):
            if leftNumber != nil && operand == nil {
                operand = leftNumber!
                waitingBinary = WaitingBinary(firstNum: operand!, waitingFunc: binaryFunction)
            }else if leftNumber != nil && operand != nil {
                operand! += leftNumber!
                waitingBinary = WaitingBinary(firstNum: operand!, waitingFunc: binaryFunction)
            }else if operand != nil && leftNumber == nil {
                waitingBinary = WaitingBinary(firstNum: operand!, waitingFunc: binaryFunction)
            }
            leftNumber = nil
            case .equal:
                getResult()
                operand = nil
            }
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

    // 바이너리 연산 값 구하기
    private func getResult() {
        if waitingBinary != nil && leftNumber != nil {
            leftNumber = waitingBinary!.doBinaryOp(with: leftNumber!)
        }
    }
    
    // 뷰콘트롤러에 넘겨줄 연산 완료된 값
    var returnValue: Double? {
        return leftNumber
    }
    
    
}
{% endhighlight %}

### 뷰 콘트롤러
{% highlight swift %}
class UpgradeCalculatorViewControler: UIViewController {

    //MARK: - UI Property
    @IBOutlet weak var displayLB: UILabel!

    //MARK: - IBAction
    //숫자 입력
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
        displayValue = 0.0
        isTyping = false
        calModel.setNumber(displayNum: displayValue)
    }
    
    // 전시된 값을 get 하거나 set하는 displayValue
    var displayValue: Double {
        get {
            return Double(displayLB.text!)!
        }
        set {
        	displayLB.text! = String(newValue)
        }
    }
    
    // operation
    var calModel = CalculatorModel()
    @IBAction func operation(_ sender: UIButton) {
        if isTyping { // check
            calModel.setNumber(displayNum: displayValue)
            isTyping = false
        }
        
        guard let symbol = sender.currentTitle else { return }
        calModel.perfomrOperation(mathSymbol: symbol)

        if calModel.returnValue != nil {
            displayValue = calModel.returnValue!
        }
    }

}
{% endhighlight %}