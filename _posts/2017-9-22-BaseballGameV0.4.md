---
layout: post
title: "스위프트 베이스볼 게임 만들기 v0.4"
author: "younari"
---

> v0.4 버전에서는 Enum, Computed Property, Property Observer 사용을 다듬고, 유저의 부적절한 행동(중복 숫자 입력, 3자리 이상 숫자 입력 등)에 대한 안내 문구를 강화하였습니다.

# 게임 연산 로직을 담은 Smart Brain

- [Xcode 프로젝트 파일 바로가기](https://github.com/younari/tastySwift/tree/master/0921_BaseballGame)
- [스위프트 베이스볼 게임 만들기 v0.1](https://younari.github.io/2017-09-21/BaseballGame)
- [스위프트 베이스볼 게임 만들기 v0.2](https://younari.github.io/2017-09-22/BaseballGameV0.2)
- [스위프트 베이스볼 게임 만들기 v0.3](https://younari.github.io/2017-09-21/BaseballGameV0.3)

{% highlight swift %}
class SmartBrain {
    
    private enum PickCase {
        case allStrike
        case ballAndStrike(strike: Int, ball: Int)
        case out
        func printString() -> String { // 라벨 출력 용도의 메소드
            switch self {
            case .out:
                return "3 Out 💩"
            case .ballAndStrike(strike: let s, ball: let b):
                return "\(s) Strike, \(b) Ball"
            case .allStrike:
                return "정답이에요👌🏻👏🏻♥️"
            }
        }
    }
    
    func compareCheck(arr1: [Int], arr2: [Int], inputStr: String) -> (SL: String, HL: String) {
        var strike: Int = 0
        var ball: Int = 0
        var caseCheck: PickCase
        
        for i in 0..<arr1.count {
            if arr1[i] == arr2[i] {
                strike += 1
            }else if arr2.contains(arr1[i]) {
                ball += 1
            }
        }
        
        // Property Observer
        var historyStr: String?
        var displayStr: String = "" {
            didSet {
                historyStr = inputStr + " 👉🏻 " + displayStr + "\n"
            }
        }
        
        if strike + ball == 0 {
            displayStr = PickCase.out.printString()
        }else if strike == 3 {
            displayStr = PickCase.allStrike.printString()
        }else {
            displayStr = PickCase.ballAndStrike(strike: strike, ball: ball).printString()
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


# UIViewController
- UIViewController는 최대한 UI 관련된 일만 하게끔 처리

{% highlight swift %}

import UIKit
class ViewController: UIViewController {
    
    //Mark - 00. UI 연관 Property
    private var isRunning: Bool = false
    private var displayLabelList: [UILabel]?
    private var selectedNumberList: [Int]?
    private var randomNumberList: [Int]?
    private var historyStr: String?
    private var displayLabelStr: String { // [Int] -> String 변환 후 전시하기 위함, Computed property
        get {
            var str: String = ""
            for i in 0..<3 { str += "\(selectedNumberList![i])" }
            return str
        }
    }
    
    //Mark - 01. UILabel
    @IBOutlet weak var scoreLabel: UILabel!
    @IBOutlet weak var displayLabel01: UILabel!
    @IBOutlet weak var displayLabel02: UILabel!
    @IBOutlet weak var displayLabel03: UILabel!
    @IBOutlet weak var historyLabel: UITextView!
    
    // Mark - 02. ViewDidLoad
    override func viewDidLoad() {
        super.viewDidLoad()
        displayLabelList = [displayLabel01,displayLabel02,displayLabel03] // LB 초기화
    }
    
    //Mark - 03. Start button : 시작
    @IBAction func btnReplay(_ sender: UIButton) {
        isRunning = true
        randomNumberList = brain.makeRandomList()
        scoreLabel.text = "시작해볼까요? 👻"
        resetProperty()
        historyStr = "" // 초기화
        historyLabel.text = ""
    }
    
    //Mark - 04. Clear Button (숫자 지우기)
    @IBAction func btnCancel(_ sender: UIButton) {
        resetProperty()
    }
    
    //Mark - 05. UIButton: btnNum click
    @IBAction func btnNumClick(_ sender: UIButton) {
        if isRunning {
            let selectedNum = sender.tag
            if selectedNumberList!.count < displayLabelList!.count && !selectedNumberList!.contains(selectedNum) {
                selectedNumberList?.append(selectedNum)
                let lastIndex = selectedNumberList!.count - 1
                let inputLabel = displayLabelList![lastIndex]
                inputLabel.text = "\(selectedNum)"
            }else if selectedNumberList!.count >= displayLabelList!.count {
                scoreLabel.text = "확인 버튼을 누르세요👇🏻"
            }else if selectedNumberList!.contains(selectedNum) {
                scoreLabel.text = "숫자 중복은 안돼요 🤡"
            }
        }
    }
    
    //Mark - 06. 연산: btnCheck - UIButton
    //연산 로직을 담고 있는 SmartBrain(모델) 의 instance 생성
    let brain: SmartBrain = SmartBrain()
    
    @IBAction func btnCheck(_ sender: UIButton) {
        if isRunning && selectedNumberList!.count == displayLabelList!.count {
            let finalStr = brain.compareCheck(arr1: selectedNumberList!, arr2: randomNumberList!, inputStr: displayLabelStr)
            scoreLabel.text = finalStr.SL
            historyStr! += finalStr.HL
            historyLabel.text = historyStr
        }
        resetProperty()
    }
    
    //초기화 함수
    func resetProperty() {
        selectedNumberList = []
        for label in displayLabelList! {
            label.text = "-"
        }
    }
    
}

{% endhighlight %}
