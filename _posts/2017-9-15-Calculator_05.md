---
layout: post
title: "Swift 계산기 만들기 v0.5"
author: "Amy"
---

### 연산 기호 중복 클릭 처리 기능 추가, 단항 연산 오류 수정
- [Xcode 프로젝트 바로가기](https://github.com/amywork/tastySwift/tree/master/0919_CalculatorBrain)


### 연산 처리를 담당하는 클래스 (모델)
{% highlight swift %}
import Foundation
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
    
    // 연산 클릭시 연산 수행
    var operand: Double?
        func perfomrOperation(mathSymbol: String) {
        if let operationCase = operDic[mathSymbol] {
            switch operationCase {
                
            case .unary(let function):
                if leftNumber != nil && operand == nil {
                    operand = function(leftNumber!)
                }else if leftNumber != nil && operand != nil {
                    operand = function(operand!)
                }
                
            case .binary(let binaryFunc):
                if leftNumber != nil && operand == nil {
                    operand = leftNumber!
                    waitingBinary = WaitingBinary(firstNum: operand!, waitingFunc: binaryFunc)
                }else if leftNumber != nil && operand != nil {
                    operand! = waitingBinary!.doBinaryOp(with: leftNumber!)
                    waitingBinary = WaitingBinary(firstNum: operand!, waitingFunc: binaryFunc)
                }else if operand != nil && leftNumber == nil {
                    waitingBinary = WaitingBinary(firstNum: operand!, waitingFunc: binaryFunc)
                }
                leftNumber = nil
                
            case .equal:
                getResult()
            }
        }
    }
    
    // Binary 연산을 저장하고 있을 Struct
    private struct WaitingBinary {
        let firstNum: Double
        let waitingFunc: (Double,Double) -> Double
        func doBinaryOp(with secondNum: Double) -> Double {
            return waitingFunc(firstNum, secondNum)
        }
    }

    // Binary 연산의 결과값 구하기 (Equal 연산)
    var returnValue: Double? // ViewController에 넘겨줄 연산 완료 값
    private func getResult() {
        if waitingBinary != nil && leftNumber != nil {
            returnValue = waitingBinary!.doBinaryOp(with: leftNumber!)
        }
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
        displayValue = 0.0
        isTyping = false
        calModel.setNumber(displayNum: displayValue)
        calModel.operand = nil
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
        
        if symbol == "=" {
            displayValue = calModel.returnValue!
        }else {
            displayValue = calModel.operand!
        }
        
    }
}
{% endhighlight %}