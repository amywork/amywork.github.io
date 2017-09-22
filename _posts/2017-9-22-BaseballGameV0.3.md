---
layout: post
title: "스위프트 베이스볼 게임 만들기 v0.3"
author: "younari"
---

> v0.3 버전에서는 Enum을 활용하여 Strike, Ball 케이스를 분기하고, 변수에 Property Observer를 활용했습니다.

# 조금 더 똑똑하게 Smart Brain 만들기

- [Xcode 프로젝트 파일 바로가기](https://github.com/younari/tastySwift/tree/master/0921_BaseballGame)
- [스위프트 베이스볼 게임 만들기 v0.1](https://younari.github.io/2017-09-21/BaseballGame)
- [스위프트 베이스볼 게임 만들기 v0.2](https://younari.github.io/2017-09-22/BaseballGameV0.2)

### 수정사항: Enum, Property Observer 추가
- Strike, Ball의 케이스를 담고있는 Enum 케이스 SET을 만들고, 케이스에 따라 String을 return하는 내부 메소드를 정의했습니다.
- Property Observer을 활용하여 History Label에 전시될 텍스트가 Score Label의 텍스트에 따라 didSet 되도록 설정했습니다.



{% highlight swift %}
class SmartBrain {
    
    private enum PickCase {
        case allStrike
        case ballAndStrike(strike: Int, ball: Int)
        case out
        func printString() -> String {
            switch self {
            case .out:
                return "3 Out 💩"
            case .ballAndStrike(strike: let s, ball: let b):
                return "S: \(s), B: \(b)"
            case .allStrike:
                return "YES👌🏻👏🏻♥️"
            }
        }
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
        
        // Property Observer
        var historyStr: String?
        var displayStr: String = "" {
            didSet {
                historyStr = myStr + " ➜ " + displayStr + "\n"
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