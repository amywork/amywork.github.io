---
layout: post
title: "날짜와 관련된 함수 만들기"
author: "younari"
---

> 연월일 입력시 요일을 반환하는 함수입니다. 
윤년인지 체크하고, 월의 끝자리수를 구해서, 최종적으로 365일 중 어느 지점에 위치했는지 판별하여 요일을 반환하는 함수입니다.

## 1. checkLeap: 윤년인지 아닌지 검사하는 함수


{% highlight swift %}
func checkLeap(year: Int) -> Bool {
    var checkValue: Bool = false
    if year % 4 == 0 && (year % 100 != 0 || year % 400 == 0){
        checkValue = true
    }else {
        checkValue = false
    }
    return checkValue
}
{% endhighlight %}

## 2. endDayOfMonth: 연/월을 입력하면, 해당 월의 끝자리 수를 구해주는 함수


{% highlight swift %}
func endDayOfMonth(year: Int, month: Int) -> Int {
    
    var endDay: Int = 0
    let inputMonth: Int = month
    
    let monA: Set = [1,3,5,7,8,10,12]
    let monB: Set = [4,6,9,11]
    
    if monA.contains(inputMonth)  {
        endDay = 31
    }else if monB.contains(inputMonth) {
        endDay = 30
    }
    
    if inputMonth == 2 {
        if checkLeap(year: year) {
            endDay = 29
        }else {
            endDay = 28
        }
    }
    return endDay
}
{% endhighlight %}

## 3. getWeekDay: 요일을 구하는 함수 : 월/일을 입력 받아서 요일을 구하기


{% highlight swift %}
func getWeekDay(atYear:Int, atMonth:Int, atDay:Int) -> String {
    
    var dayDay:[String] = ["일", "월", "화", "수", "목", "금", "토"]
    var returnValue: String = ""
    var totalDay: Int = 0
    
    for i in 1..<atMonth {
        totalDay += endDayOfMonth(year: atYear, month: i)
    }
    
    var index: Int = 0
    if (totalDay + atDay) % 7 == 0 {
        index = 6
    }else {
        index = (totalDay + atDay) % 7 - 1
    }
    
    returnValue = dayDay[index]
    
    return "\(atYear)년 \(atMonth)월 \(atDay)일은 " + returnValue + "요일 입니다."
}

print(getWeekDay(atYear: 2017, atMonth: 9, atDay: 11))
{% endhighlight %}

<br>
<hr>
<br>

## 번외. 요일 구하는 함수 만들기

{% highlight swift %}
func endOfMonth(atMonth: Int) -> Int {
    let set30: [Int] = [1,3,5,7,8,10,12]
    var endDay: Int = 0
    if atMonth == 2 {
        endDay = 28
    }else if set30.contains(atMonth) {
        endDay = 31
    }else {
        endDay = 30
    }
    
    return endDay
}

func getWeekDay(month: Int, day: Int) -> String {
   
    var week: [String] = ["일", "월", "화", "수", "목", "금", "토"]
    var totalDay: Int = 0

    if month > 1 {
        for i in 1..<month {
            let endDay: Int = endOfMonth(atMonth: i)
            totalDay += endDay
        }
        
        totalDay = totalDay + day
        
    }else if month == 1 {
        totalDay = day
    }
    
    var index: Int = (totalDay) % 7

    if index > 0 {
        index = index - 1
    }
    
    return week[index]
}
{% endhighlight %}
