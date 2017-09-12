---
layout: post
title: "단위와 관련된 함수 만들기"
author: "younari"
---
 
> cm, inch, m, 평, MB, KB, 시, 분, 초 등 단위 변환과 관련된 여러가지 함수입니다.

### - 시,분,초 입력 받아 초를 반환하는 함수 만들기

{% highlight swift %}
func timeToSecond(time: Int) -> Int {
    var t: Int = time
    var tarr: [Int] = []
    while t > 0 {
        tarr.append(t%100)
        t = t/100
    }
    return tarr[0] + tarr[1]*60 + tarr[2]*60*60
}

var test = timeToSecond(time: 11320)
print(test)
{% endhighlight %}


{% highlight swift %}
func secondToTime(second: Int) -> Int {
    var returnValue: Int = 0
    var tmp = second
    var hour = tmp/3600
    tmp = tmp%3600
    var miniute = tmp/60
    tmp = tmp%60
    var second = tmp
    returnValue = second + miniute*100 + hour*10000
    return returnValue
}
{% endhighlight %}

### - inch와 centi 단위 변환하기

{% highlight swift %}
func inchToCenti(inch: Double) -> Double {
    return inch * 2.54
}

func centiToInch(centi: Double) -> Double {
    return centi / 2.54
}   
{% endhighlight %}

{% highlight swift %}
func unitChange(inputType: String, outputType: String, inputValue: Double) -> Double {
    
    var returnValue: Double = 0.0
    if inputType == "cm" {
        if outputType == "m" {
            returnValue = inputValue / 100
        }else if outputType == "inch" {
            // inch 구하는 함수를 미리 만들어놓고 호출 가능
            returnValue = centiToInch(centi: inputValue)
        }
    }
    
    return returnValue
}
{% endhighlight %}

### - 이것저것 단위 변환하기

{% highlight swift %}
func SquaremeterToPyeong(m2: Double) -> Double {
    return m2 / 3.3058
}

func pyeongToSquaremeter(p: Double) -> Double {
    return p * 3.3058
}

func celciusToFahrenheit(c: Double) -> Double {
    return c * 1.8 + 32
}

func fahrenheitTocelcius(f: Double) -> Double {
    return (f - 32) / 1.8
}

func KiloToMega(kb: Double) -> Double {
    return kb * 1024
}

func MegaToKilo(mb: Double) -> Double {
    return mb / 1024
}
{% endhighlight %}