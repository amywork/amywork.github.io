---
layout: post
title: "소수 판별 함수 만들기"
author: "younari"
---

> 2부터 입력된 숫자까지의 모든 소수를 찾아서 반환하는 함수(소수 판별 함수)입니다.

## 소수 판별 함수 만들기 (true or false)

{% highlight swift %}
func isPrime(num: Int) -> Bool {
    var result: Bool = true
    
    if num < 2 {
        result = false
    }else if num > 2 {
        for i in 2..<num {
            if num%i == 0 {
                result = false
            }
        }
    }
    return result
}
{% endhighlight %}

## 소수 반환하는 함수 만들기

{% highlight swift %}
func allPrimeNumber(endNum: Int) -> Array<Any>
{
    var newList:[Int] = []
    var resultList:[Int] = []
    let inputNum: Int = endNum
    
    // 01. 입력 받은 숫자까지의 배열을 만들어둔다.
    for i in 1...inputNum
    {
        newList.append(i)
    }
    
    // 02. 배열 속의 숫자가 소수인지 검사해서,
    let count: Int = newList.count
    for j in 0..<count {
        // 만약 j가 소수이면, resultList 배열에 넣는다,
        if isPrime(num: newList[j])  {
            resultList.append(newList[j])
        }
    }
    
    return resultList
}
// 03. 소수만 담긴 새 배열을 프린트한다.
// print(allPrimeNumber(endNum:13)) // [2,3,5,7]
{% endhighlight %}

## 다른 방법으로도 풀어보기

{% highlight swift %}
func getPrime2(num: Int) -> [Int] {
    let endNum: Int = num
    var allNumbers:Set<Int> = []
    var primeNumbers:[Int] = []
    for checkNum in 2...endNum {
        if !allNumbers.contains(checkNum) {
            primeNumbers.append(checkNum)
            var index = checkNum * 2
            while index <= endNum {
                allNumbers.insert(index)
                index += checkNum
            }
        }
    }
    return primeNumbers
}
{% endhighlight %}