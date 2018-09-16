---
layout: post
title: "삼각수 만들기"
author: "Amy"
---

> 1부터 입력한 숫자까지의 합, 삼각수를 구하는 함수입니다.

### 01. 변수를 3개 활용하는 방법

{% highlight swift %}
var inputValue = 15
var triangleNum: Int = 0
var temp: Int = 0

while temp < inputValue
{
    temp += 1
    triangleNum = temp + triangleNum
}
{% endhighlight %}


### 02. 마지막 수부터 거꾸로 계속 더하는 방법
{% highlight swift %}
var inputValue = 15
var triangleNum: Int = 0

while inputValue == 0
{
    triangleNum = inputValue + triangleNum
    inputValue -= 1
}
{% endhighlight %}

### 03. 기존 공식 활용 (1+N)/2
{% highlight swift %}
print(inputValue*(inputValue+1)/2)
{% endhighlight %}
