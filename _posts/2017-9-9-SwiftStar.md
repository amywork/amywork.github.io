---
layout: post
title: "Swift 삼각형 별찍기"
author: "younari"
---

> 여백과 별모양 각각을 스트링을 생각해서 한 줄식 프린트했습니다. N번째 줄마다 여백 스트링을 line-n개 찍고, 별을 2n-1개씩 찍는 방법입니다.

## 총 몇 줄의 별을 찍을지 Line을 입력받아, 삼각별을 찍는 함수 v0.1


{% highlight swift %}
func drawStar(line:Int) {
    
    for n in 1...line {
        
        var returnString: String = ""
        var str1: String = ""
        var str2: String = ""
        var i: Int = 0
        var j: Int = 0
        
        while i < (line-n) {
            str1 = str1 + " "
            i += 1
        }
        
        while j < (2*n-1) {
            str2 = str2 + "*"
            j += 1
        }
        
        returnString = str1 + str2
        print(returnString)
    }
}
{% endhighlight %}

## 총 몇 줄의 별을 찍을지 Line을 입력받아, 삼각별을 찍는 함수 v0.2


{% highlight swift %}
func drawStar2(line: Int) -> String {
    
    var resultStr: String = ""
    
    for n in 0..<line
    {
        
        var starStr: String = ""
        //For1
        for _ in 0..<line-n{
            starStr += " "
        }
        
        //For2
        for _ in 0..<(2*n+1){
            starStr += "*"
        }
        resultStr += starStr + "\n"
    }
    
    return resultStr
}
{% endhighlight %}
