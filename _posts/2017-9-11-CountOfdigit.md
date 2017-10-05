---
layout: post
title: "자릿수 판별 함수"
author: "younari"
---

> 숫자를 입력하면 해당 숫자의 자릿수를 Int로 반환하는 함수입니다. 10으로 나눈 몫을 활용해 자릿수 count를 증가시켰습니다.

{% highlight swift %}

func countOfDigit(_ number: Int) -> Int {
    
    var num = number
    var count: Int = 0
    while num > 0 {
        num /= 10
        count += 1
    }
    return count
}

countOfDigit(12345) // 5자리수

{% endhighlight %}


