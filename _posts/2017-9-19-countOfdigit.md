---
layout: post
title: "자릿수 판별 함수 만들어보기"
author: "younari"
---

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
