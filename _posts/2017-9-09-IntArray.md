---
layout: post
title: "Int와 Array"
author: "younari"
---

## 입력받은 숫자를 숫자의 배열로 만들기 v0.1
- While문을 활용합니다.

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

## 입력받은 숫자를 숫자의 배열로 만들기 v0.2
- 재귀를 활용합니다.

{% highlight swift %}
func digits(_ number: Int) -> [Int] {
    if number >= 10 {
        let firstDigits = digits(number / 10)
        let lastDigit = number % 10
        return firstDigits + [lastDigit]
    } else {
        return [number]
    }
}
{% endhighlight %}

## 배열의 최대값과 최소값을 반환하기

{% highlight swift %}
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
{% endhighlight %}
