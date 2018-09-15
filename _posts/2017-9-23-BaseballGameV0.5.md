---
layout: post
title: "스위프트 베이스볼 게임 만들기 v0.5"
author: "Amy"
---

> ViewController에서 UI 처리를 하는 것이 일반적이지만, 클래스 구조화 연습을 위해 Interface Generator 라는 UI 전용 클래스를 생성하여, ViewController의 UI 처리 기능을 모두 해당 클래스로 이관하고, 연산 기능은 GameBrain 클래스를 통해 처리하였습니다. 추가로 brain 모델에서 Enum 케이스에 대한 Switch문을 추가하여 Model 코드를 정돈하였습니다.

- [Xcode 프로젝트 파일 바로가기](https://github.com/amywork/tastySwift/tree/master/0925_BallGameV0.2)
- [스위프트 베이스볼 게임 만들기 v0.1](https://amywork.github.io/2017-09-21/BaseballGame)
- [스위프트 베이스볼 게임 만들기 v0.2](https://amywork.github.io/2017-09-22/BaseballGameV0.2)
- [스위프트 베이스볼 게임 만들기 v0.3](https://amywork.github.io/2017-09-21/BaseballGameV0.3)
- [스위프트 베이스볼 게임 만들기 v0.4](https://amywork.github.io/2017-09-22/BaseballGameV0.4)


# View Controller

{% highlight swift %}

import UIKit
class ViewController: UIViewController {

    //Mark - 01. UILabel
    @IBOutlet weak var scoreLabel: UILabel!
    @IBOutlet weak var displayLabel01: UILabel!
    @IBOutlet weak var displayLabel02: UILabel!
    @IBOutlet weak var displayLabel03: UILabel!
    @IBOutlet weak var historyLabel: UITextView!
    
    var generator = InterfaceGenerator()
   
    // Mark - 02. ViewDidLoad
    override func viewDidLoad() {
        super.viewDidLoad()
        generator.displayLabelList = [displayLabel01,displayLabel02,displayLabel03]
    }
    
    //Mark - 03. Start button : 시작
    @IBAction func btnReplay(_ sender: UIButton) {
        generator.play(scoreLB: scoreLabel, historyLB: historyLabel)
    }
    
    
    //Mark - 04. Clear Button (숫자 지우기)
    @IBAction func btnCancel(_ sender: UIButton) {
        generator.delete()
    }
    
    //Mark - 05. UIButton: btnNum click
    @IBAction func btnNumClick(_ sender: UIButton) {
        generator.btnClick(sender, scoreLB: scoreLabel)
    }

    //Mark - 06. GameBrain 모델을 통한 btnCheck 연산
    @IBAction func btnCheck(_ sender: UIButton) {
        generator.check(scoreLB: scoreLabel, historyLB: historyLabel)
    }
    
}
{% endhighlight %}


# Interface Generator

{% highlight swift %}
import UIKit
import Foundation
class InterfaceGenerator {
    private var brain = GameBrain()
    var displayLabelList: Array<UILabel> = []
    var isRunning: Bool = false
    var selectedNumberList: [Int] = []
    var randomNumberList: [Int] = []
    var historyStr: String = ""
    var displayLabelStr: String {
        get {
            var str: String = ""
            for i in 0..<3 { str += "\(selectedNumberList[i])" }
            return str
        }
    }
    
    func play(scoreLB: UILabel, historyLB: UITextView) {
        isRunning = true
        selectedNumberList = []
        historyStr = ""
        randomNumberList = brain.makeRandomList()
        for label in displayLabelList {
            label.text = "-"
        }
        scoreLB.text = "시작해볼까요?👻"
        historyLB.text = ""
    }
    
    func delete() {
        for label in displayLabelList {
            label.text = "-"
        }
        selectedNumberList = []
    }

    func btnClick(_ sender: UIButton, scoreLB: UILabel) {
        if isRunning {
            let selectedNum = sender.tag
            if selectedNumberList.count < displayLabelList.count && !selectedNumberList.contains(selectedNum) {
                selectedNumberList.append(selectedNum)
                let lastIndex = selectedNumberList.count - 1
                let inputLabel = displayLabelList[lastIndex]
                inputLabel.text = "\(selectedNum)"
            }else if selectedNumberList.count >= displayLabelList.count {
                scoreLB.text = "Check 버튼을 눌러주세요!"
            }else if selectedNumberList.contains(selectedNum) {
                scoreLB.text = "숫자 중복은 안돼요 🤡"
            }
        }
    }
    

    func check(scoreLB: UILabel, historyLB: UITextView) {
        if isRunning && selectedNumberList.count == displayLabelList.count {
            let finalStr = brain.compareCheck(arr1: selectedNumberList, arr2: randomNumberList, inputStr: displayLabelStr)
            scoreLB.text = finalStr.SL
            historyStr += finalStr.HL
            historyLB.text = historyStr
            selectedNumberList = []
            for label in displayLabelList {
                label.text = "-"
            }
        }else if isRunning && selectedNumberList.count < displayLabelList.count {
            scoreLB.text = "숫자 3개를 모두 선택해주세요"
        }else if !isRunning {
            scoreLB.text = "play 버튼을 눌러주세요"
        }
    }

}
{% endhighlight %}

# Game Brain

{% highlight swift %}

import Foundation
class GameBrain {

    private enum AllCase {
        case allStrike
        case ballAndStrike(strike: Int, ball: Int)
        case out
        func printString() -> String {
            switch self {
            case .out:
                return "3 Out 💩"
            case .ballAndStrike(strike: let s, ball: let b):
                return "\(s) Strike, \(b) Ball"
            case .allStrike:
                return "정답입니다👌🏻👏🏻♥️"
            }
        }
    }
    
    func compareCheck(arr1: [Int], arr2: [Int], inputStr: String) -> (SL: String, HL: String) {
        var strike: Int = 0
        var ball: Int = 0
        var checkCase: AllCase
        var historyStr: String?
        var displayStr: String = "" {
            didSet {
                historyStr = inputStr + " 👉🏻 " + displayStr + "\n"
            }
        }
        
        for i in 0..<arr1.count {
            if arr1[i] == arr2[i] {
                strike += 1
            }else if arr2.contains(arr1[i]) {
                ball += 1
            }
        }
        
        if strike + ball == 0 { checkCase = .out }
        else if strike == 3 { checkCase = .allStrike }
        else { checkCase = .ballAndStrike(strike: strike, ball: ball) }

        switch checkCase {
        case .allStrike:
            displayStr = AllCase.allStrike.printString()
        case .ballAndStrike(strike: let S, ball: let B):
            displayStr = AllCase.ballAndStrike(strike: S, ball: B).printString()
        case .out:
            displayStr = AllCase.out.printString()
        }
        
        return (SL: displayStr, HL: historyStr!)
    }
    
    // 랜덤의 3자리 Int 생성 함수
    func makeRandomList() -> [Int] {
        var randomNumberList: [Int] = []
        while randomNumberList.count < 3 {
            let randomNum: Int = Int(arc4random_uniform(10))
            if !randomNumberList.contains(randomNum) {
                randomNumberList.append(randomNum)
            }
        }
        return randomNumberList
    }
    
}

{% endhighlight %}


