---
layout: post
title: "스위프트 베이스볼 게임 만들기 v0.3"
author: "younari"
---

> v0.3 버전에서는 Enum을 활용하여 Strike, Ball 케이스를 분기하고, 변수에 Property Observer를 활용했습니다.

# 조금 더 똑똑하게 Smart Brain 만들기

- [Xcode 프로젝트 파일 바로가기](https://github.com/younari/tastySwift/tree/master/0920_BaseballGame)
- [스위프트 베이스볼 게임 만들기 v0.1](https://younari.github.io/2017-09-21/BaseballGame)
- [스위프트 베이스볼 게임 만들기 v0.2](https://younari.github.io/2017-09-22/BaseballGameV0.2)

### 수정사항: Enum, Property Observer 추가
- Strike 및 Ball의 케이스를 담고있는 Enum 케이스 SET을 만들었습니다.
- Property Observer을 활용하여 History Label에 전시될 텍스트가 Score Label의 텍스트에 따라 willSet 되도록 설정했습니다.



{% highlight swift %}

class SmartBrain {
    
    // Strike 및 Ball의 케이스를 담고있는 Enum
    private enum PickCase {
        case allStrike //"YES👌🏻👏🏻♥️"
        case ballAndStrike(strike: Int, ball: Int) //"S: \(strike), B:  \(ball)"
        case out //"3 Out 💩"
    }

    func compareCheck(arr1: [Int], arr2: [Int], myStr: String) -> (SL: String, HL: String) {
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
        
        if strike + ball == 0 {
            caseCheck = PickCase.out
        }else if strike == 3 {
            caseCheck = PickCase.allStrike
        }else {
            caseCheck = PickCase.ballAndStrike(strike: strike, ball: ball)
        }
        

        
        // Property Observer
        var historyStr: String?
        var displayStr: String = "" {
            didSet {
                historyStr = myStr + " ➜ " + displayStr + "\n"
            }
        }
        
        switch caseCheck {
        case .out:
            displayStr = "3 Out 💩"
        case .ballAndStrike:
            displayStr = "S: \(strike), B:  \(ball)"
        case .allStrike:
            displayStr = "YES👌🏻👏🏻♥️"
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