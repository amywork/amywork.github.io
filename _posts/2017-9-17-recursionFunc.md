---
layout: post
title: "재귀를 활용하는 함수"
author: "younari"
---

# 팩토리얼 값을 구하는 함수
> Factorial: 1부터 어떤 양의 정수 n까지의 정수를 모두 곱한 것, n!

{% highlight swift %}

func factorial(num: Int) -> Int {
    
    var total: Int = num
    if num == 0 {
        return 1
    }
    
    total *= factorial(num: num-1)
    
    return total
    
}

{% endhighlight %}


# 피보나치 수열 N번째 값 구하기

### 01. 첫 번째 접근

{% highlight swift %}
func pibonachi(num: Int) -> Int {
    var index: Int = 0
    if num == 1 || num == 2 {
        return 1
    }
    index += pibonachi(num: num-2) + pibonachi(num: num-1)
    return index
}
{% endhighlight %}

### 02. 더 간단히 표현하기 (변수 제거!)
{% highlight swift %}
func piboSecond(num: Int) -> Int {
    if num == 1 || num == 2 { return 1 }
    return piboSecond(num: num-1) + piboSecond(num: num-2)
}
{% endhighlight %}

### 03. 더 더 간단히 표현하기 (삼항 연산자)
{% highlight swift %}
func piboThird(num: Int) -> Int {
    return (num == 1 || num == 2) ? 1 : piboThird(num: num-1) + piboThird(num: num-2)
}
{% endhighlight %}


### 04. 피보나치 수열의 합산 구하기
{% highlight swift %}
func pivoTotal(num: Int) -> Int {   
    var total: Int = 0
    for i in 1...num {
        total += piboThird(num: i)
    }
    return total
}
{% endhighlight %}