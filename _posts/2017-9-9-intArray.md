---
layout: post
title: "배열 속 Int 간의 크기 비교하"
author: "younari"
---

> 배열에 담긴 각각의 숫자들의 크기를 비교하는 함수입니다.

## 배열 속에 담긴 INT들의 대소 비교

{% highlight swift %}
func compareNext(inputArray: [String]) {
    var resultArray: [String] = []
    let count: Int = inputArray.count
    for i in 0..<count {
    if i==0 {
		resultArray.append(inputArray[0])
	}else if Int(inputArray[i])! < Int(inputArray[i-1])! {
        resultArray.append(inputArray[i])
	}
    }
	print(resultArray)
}
compareNext(inputArray: ["1", "3", "2", "8", "9"])
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
