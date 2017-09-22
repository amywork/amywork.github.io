---
layout: post
title: "스위프트 베이스볼 게임 만들기 v0.1"
author: "younari"
---

> 임의의 3자리 숫자(중복 불가)를 컴퓨터가 자동으로 만들고, 사용자가 해당 숫자를 맞추는 게임입니다. 자릿수가 맞으면 Strike, 자릿수는 맞지 않지만 해당 숫자가 들어있으면 Ball입니다. v0.1버전에서는 Try 횟수 제한을 하지는 않았습니다. v0.2 버전에서는 Try 횟수 제한 및 Model 부분의 구조화를 좀 더 강화할 예정입니다.


# 01. 일단 손코딩
> 작업 노트: Baseball Game을 만들기 위해, 일단 키노트에 핵심적일 것이라고 생각되는 조건들(필요한 변수 및 연산 함수)을 1시간 정도 구성해본 다음 실제 작업에 착수했더니 훨씬 수월하게 진행할 수 있었습니다.

- 게임 상태 변수 : var isRunning: Bool = false
- 숫자 라벨 배열 : var displayLabelList: [UILabel]?
- 클릭 숫자 배열 : var selectedNumberList: [Int]?
- 스트라이크 : var numStrike: Int = 0
- 볼 : var numBall: Int = 0
- 랜덤 넘버 : var randomNumberList: [Int]?
- 히스토리 영역 : var historyStr: String = ""
- **핵심 연산** : Brain 클래스로 따로 빼서 인스턴스로 메소드 호출하기
- `if displayArray.count == 3 { displayArray & randomArray 비교 함수 실행  }`
- `function compareNum ([], [])`
- `for i in 0…2`
- `if displayArray[i] == randomArray[i]  { numStrike +1 }`
- `else if randomArray.contains(displayArray[0]) { numBall +1 }`
- `else  { numOut + 1 }` // 이 조건은 생략 가능
- **scoreLabel 및 history에 S, B 값 전시**
- `if numStrike + numBall == 0 { scoreLabel = “3 Out”, history = “\(값) : 3 Out” }`
- `else if numStrike == 3 { scoreLabel = “정답입니다!”, btnReplay와 같은 효과(초기화) }`
- `else { scoreLabel.currentTitle = “S: \(strike), B:  \(ball)”,  historyLabel.currentTitle = “\(값) : S: \(strike), B:  \(ball)” }`


# 02. 뷰콘트롤러
- [Xcode 프로젝트 파일 바로가기](https://github.com/younari/tastySwift/tree/master/0920_BaseballGame)

{% highlight swift %}

//
//  ViewController.swift
//  0920_BaseballGame
//
//  Created by 김기윤 on 21/09/2017.
//  Copyright © 2017 younari. All rights reserved.
//

import UIKit

class ViewController: UIViewController {


    //Mark - 00. Property
    private var isRunning: Bool = false
    private var displayLabelList: [UILabel]?
    private var selectedNumberList: [Int]?
    var numStrike: Int = 0
    var numBall: Int = 0
    var randomNumberList: [Int]?
    var historyStr: String = ""

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
    
    //게임 Brain 모델 의 instance 생성
    var brain: GameBrain = GameBrain()
    
    //Mark - 03. Start button Click : 시작
    @IBAction func btnReplay(_ sender: UIButton) {
        resetProperty()
        isRunning = true
        randomNumberList = brain.makeRandomList()
        historyStr = ""
        historyLabel.text = ""
        scoreLabel.text = ""
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
                scoreLabel.text = "중복 숫자 불가 🤡"
            }
        }
    }
    
    
    //Mark - 06. UIButton: btnCheck, btnCancel, btnReplay
    @IBAction func btnCheck(_ sender: UIButton) {
        if isRunning && selectedNumberList!.count == displayLabelList!.count {
        
            // Brain, 계산을 부탁해
            let result = brain.compareCheck(arr1: selectedNumberList!, arr2: randomNumberList!)

            
            //scoreLabel, history에 전시
            var displayLabelStr: String = ""
            for i in 0..<3 {
                displayLabelStr += "\(selectedNumberList![i])"
            }
            
            if result.s + result.b == 0 {
                scoreLabel.text = "3 Out!"
                historyStr += displayLabelStr + " : 3 Out! \n"
            }else if result.s == 3 {
                scoreLabel.text = "YES👌🏻👏🏻♥️"
                historyStr += displayLabelStr + " YES👌🏻👏🏻♥️"
            }else {
                scoreLabel.text = "S: \(result.s), B:  \(result.b)"
                historyStr += displayLabelStr + " S: \(result.s), B:  \(result.b) \n"
            }
            
        }
        historyLabel.text = historyStr
        resetProperty()
    }
    

    //초기화 Method
    func resetProperty() {
        selectedNumberList = []
        numStrike = 0
        numBall = 0
        for label in displayLabelList! {
            label.text = "-"
        }
    }

}
{% endhighlight %}


# 03. 연산 모델

{% highlight swift %}
////
////  CompareClass.swift
////  0920_BaseballGame
////
////  Created by 김기윤 on 21/09/2017.
////  Copyright © 2017 younari. All rights reserved.


import Foundation
class GameBrain {

    // 두개의 배열을 받아서, strike / ball을 체크해서 tuple로 넘겨준다.
    func compareCheck(arr1: [Int], arr2: [Int]) -> (s: Int,b: Int) {
        
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
 
    
    
    // 3자릿수의 랜덤수 생성 함수
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