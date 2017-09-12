---
layout: post
title: "배열내 연속으로 중복된 숫자 제거하기"
author: "younari"
---

## 연속으로 중복된 숫자 없애기
- 입력받은 숫자를 숫자의 배열로 만들어주는 함수

{% highlight swift %}
func intToArr(num: Int) -> Array<Int> {
    
    var intArr: [Int] = []
    var number: Int = num
    while number > 0 {
        intArr.insert(number%10, at: 0)
        number = number/10
    }
    return intArr
}
{% endhighlight %}
- 제곱수를 만들어주는 함수 (num1의 num2승을 만들어주는 함수)

{% highlight swift %}
func square(value: Int, exponent: Int) -> Int {
    
    var result: Int = 1
    for _ in 0..<exponent {
        result *= value
    }
    
    return result
}
{% endhighlight %}

- 연속으로 중복된 숫자를 없애서 Int로 최종 반환해주는 함수

{% highlight swift %}
func shorter(num: Int) -> Int {
    
    let number: Int = num
    var originalArr: [Int] = intToArr(num: number)
    var shortArr: [Int] = []
    
    // 중복 제거한 숫자들을 shortArr에 넣는다.
    for i in 0..<originalArr.count {
        if i == 0 {
            shortArr.insert(originalArr[i], at: 0)
        }else if originalArr[i-1] != originalArr[i] {
            shortArr.insert(originalArr[i], at: 0)
        }
    }
    // 배열에 담긴 숫자들을 Int로 변환하여 최종 출력
    var returnValue: Int = 0
    for j in 0..<shortArr.count {
        returnValue += shortArr[j] * square(value: 10, exponent: j)
    }
    return returnValue
}
{% endhighlight %}

<hr>
<hr>

- 다른 방법으로도 풀어보기

{% highlight swift %}
func sqrOf10(num: Int) -> Int {
    
    var returnValue: Int = 1
    
    for _ in 0..<num {
        returnValue *= 10
    }
    return returnValue
}

func shorter2(num: Int) -> Int {
    
    var num: Int = num
    var shortlist: [Int] = []

    while num > 0 {
        let comparenum = num%10
        
        if shortlist.isEmpty {
            shortlist.append(comparenum)
        }else {
            let lastNum: Int = shortlist.last!
            if lastNum != comparenum {
                shortlist.append(comparenum)
            }
        }
        
        num /= 10
    }
    
    
    var index: Int = 0
    var result = 0
    
    for i in shortlist {
        result += i * sqrOf10(num: index)
        index += 1
    }
    
    return result
}
{% endhighlight %}

