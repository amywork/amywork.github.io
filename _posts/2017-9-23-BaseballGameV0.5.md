---
layout: post
title: "스위프트 베이스볼 게임 만들기 v0.5"
author: "younari"
---

> Enum 케이스에 대한 Switch문을 추가하여 Model 코드를 정돈하였습니다.

- [Xcode 프로젝트 파일 바로가기](https://github.com/younari/tastySwift/tree/master/0925_BallGameV0.2)
- [스위프트 베이스볼 게임 만들기 v0.1](https://younari.github.io/2017-09-21/BaseballGame)
- [스위프트 베이스볼 게임 만들기 v0.2](https://younari.github.io/2017-09-22/BaseballGameV0.2)
- [스위프트 베이스볼 게임 만들기 v0.3](https://younari.github.io/2017-09-21/BaseballGameV0.3)
- [스위프트 베이스볼 게임 만들기 v0.4](https://younari.github.io/2017-09-22/BaseballGameV0.4)


# Model: GameBrain

{% highlight swift %}
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
        else if strike == 3 { checkCase = .allStrike
        }else {
            checkCase = .ballAndStrike(strike: strike, ball: ball)
        }

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
