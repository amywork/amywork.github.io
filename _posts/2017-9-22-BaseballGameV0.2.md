---
layout: post
title: "스위프트 베이스볼 게임 만들기 v0.2"
author: "younari"
---

> v0.2 버전에서는 Model 부분의 구조화를 좀 더 강화하여, View 영역에서는 UI 처리에만 집중할 수 있도록 함수를 모두 분리하였습니다.


# 01. UI를 담당하는 View Controler
- [Xcode 프로젝트 파일 바로가기](https://github.com/younari/tastySwift/tree/master/0920_BaseballGame)

{% highlight swift %}
import UIKit
class ViewController: UIViewController {

    //Mark - 00. Property
    private var isRunning: Bool = false
    private var displayLabelList: [UILabel]?
    private var selectedNumberList: [Int]?
    private var randomNumberList: [Int]?
    private var historyStr: String = ""
    private var displayLabelStr: String {
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
        displayLabelList = [displayLabel01,displayLabel02,displayLabel03]
    }
    
    //Mark - 03. Start button : 시작
    @IBAction func btnReplay(_ sender: UIButton) {
        resetProperty()
        isRunning = true
        randomNumberList = brain.makeRandomList()
        scoreLabel.text = "시작해볼까요 👻"
        historyStr = ""
        historyLabel.text = ""
    }
    
    //Mark - 04. Clear Button (숫자 지우기)
    @IBAction func btnCancel(_ sender: UIButton) {
        resetProperty()
    }

    //Mark - 05. UIButton: btnNumber click
    @IBAction func btnNumClick(_ sender: UIButton) {
        if isRunning {
            let selectedNum = sender.tag
            if selectedNumberList!.count < displayLabelList!.count && !selectedNumberList!.contains(selectedNum) {
                selectedNumberList?.append(selectedNum)
                let lastIndex = selectedNumberList!.count - 1
                let inputLabel = displayLabelList![lastIndex]
                inputLabel.text = "\(selectedNum)"
            }else {
                scoreLabel.text = "숫자 중복은 안돼요 🤡"
            }
        }
    }
    
    //Mark - 06. 연산: btnCheck - UIButton
    //6-1. 게임 Brain 모델 의 instance 생성
    let brain: GameBrain = GameBrain()
    
    @IBAction func btnCheck(_ sender: UIButton) {
        if isRunning && selectedNumberList!.count == displayLabelList!.count {
            // Brain, 연산완료후 최종 String 반환
            let finalStr = brain.finalSetting(arr1: selectedNumberList!, arr2: randomNumberList!, myStr: displayLabelStr)
            scoreLabel.text = finalStr.SL
            historyStr += finalStr.HL
            historyLabel.text = historyStr
        }
        resetProperty()
    }
    
    //초기화 Method
    func resetProperty() {
        selectedNumberList = []
        for label in displayLabelList! {
            label.text = "-"
        }
    }

}
{% endhighlight %}


# 02. 연산 모델 Game Brain

{% highlight swift %}
class GameBrain {

    // A. 두개의 비교 대상 배열을 받아서, (strike, ball) tuple로 리턴.
    private func compareCheck(arr1: [Int], arr2: [Int]) -> (s: Int,b: Int) {
        var strike: Int = 0
        var ball: Int = 0
        for i in 0..<arr1.count {
            if arr1[i] == arr2[i] {
                strike += 1
            }else if arr2.contains(arr1[i]) {
                ball += 1
            }
        }
        return (s: strike,b: ball)
    }
    
    // B. (Strike,Ball) 튜플을 받아서 전시할 문자 (스코어라벨,히스토리라벨) 튜플로 바꿔주는 함수
    private func tupleToString(tuple: (s: Int,b: Int), myStr: String) -> (SL: String, HL: String) {
        var displayStr: String = ""
        var historyStr: String = ""
        
        let result = tuple
        if result.s + result.b == 0 {
            displayStr = "3 Out 💩"
        }else if result.s == 3 {
            displayStr = "YES👌🏻👏🏻♥️"
        }else {
            displayStr = "S: \(result.s), B:  \(result.b)"
        }
    
        historyStr = myStr + " ➜ " + displayStr + "\n"
        return (SL: displayStr,HL: historyStr)
    }
    
    // A + B  =  위에 두개 함수를 하나로 합쳐보긔. 요것만 갖다쓰게끔
    func finalSetting(arr1: [Int], arr2: [Int], myStr: String) -> (SL: String, HL: String) {
        let compareResult = compareCheck(arr1: arr1, arr2: arr2)
        let returnResult = tupleToString(tuple: compareResult, myStr: myStr)
        return returnResult
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