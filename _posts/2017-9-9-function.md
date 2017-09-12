---
layout: post
title: "함수 만들기 연습"
author: "younari"
---

> Swift의 기초 문법을 익히기 위해 변수, 상수, 연산자, 조건문을 활용하는 기초 함수를 만들어보았습니다.

# 기초 함수 만들기
## 각 자리를 더하는 함수

{% highlight swift %}
func addAllDigit2(num :Int) -> Int {
    
    var num = num
    var positionNumber = 0
    
// (number/10) >= 0 && number != 0 라는 조건을, number > 0으로 바꿔도 문제가 없다.
    while num > 0 {
        positionNumber += num % 10
        num = num / 10
        
    }
    
    return positionNumber
    
}
{% endhighlight %}


## 숫자 Reverse
{% highlight swift %}
func revers(num: Int) -> String
{
    var returnValue: String = ""
    var number = num
 
    while number > 0
    {
        let lastNum = number%10
        returnValue += "\(lastNum)"
        number /= 10
    }
    
    return returnValue
}

print(revers(num:123))//321
print(revers(num:341))//143
{% endhighlight %}

## 윤년 판별
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

## 큰 수 반환하기
{% highlight swift %}
func compareInt(x: Int, y:Int) -> String {
    
    var returnValue: String = ""
    
    if x-y > 0 {
        returnValue = "\(x)"
    }else if x-y < 0 {
        returnValue = "\(y)"
    }else if x-y == 0 {
        returnValue = "두 숫자가 같습니다."
    }
    return returnValue
}
{% endhighlight %}

## 여러가지 도형 함수
{% highlight swift %}
let PI: Double = 3.14
// 01 Square function
func square(type:String, s:Int) -> Int
{
    var returnValue: Int = 0
    if type == "Area" {
        returnValue = s * s
    }else if type == "Perimeter" {
        returnValue = 4 * s
    }else {
        print("타입이 틀렸습니다")
        returnValue = 0
    }
    return returnValue
}
// 02 Rectangle function
func rectangle(type:String, w:Int, l:Int) -> Int
{
    var returnValue: Int = 0
    if type == "Area" {
        returnValue = w * l
    }else if type == "Perimeter" {
        returnValue = (2*w)+(2*l)
    }
    return returnValue
}
// 03 Circle function
func circle(type:String, radius:Double) -> Double
{
    var returnValue: Double = 0
    if type == "Area" {
        returnValue = PI * radius * radius
    }else if type == "Circumference" {
        returnValue = 2 * PI * radius
    }
    return returnValue
}
// 04 Triangle Area function
func triangleArea(b:Double, h:Double) -> Double
{
    var returnValue: Double = 0
    returnValue = 0.5 * b * h
    return returnValue
}
// 05 Trapezold Area function
func trapezoldArea(a:Double, b:Double, h:Double) -> Double
{
    var returnValue: Double = 0
    returnValue = 0.5 * h * (a + b)
    return returnValue
}
// 06 Cube Volume function
func cubeVolume(s:Int) -> Int
{
    var returnValue: Int = 0
    returnValue = s * s * s
    return returnValue
}
// 07 Rectangle Solid Volume function
func rectangleSolidVolume(l:Int, w:Int, h:Int) -> Int
{
    var returnValue: Int = 0
    returnValue = l * w * h
    return returnValue
}
// 08 Cylinder, Sphere, Cone Volume function
func volumeOfCSV(type:String, r:Double, h:Double) -> Double
{
    var returnValue: Double = 0
    if type == "Cylinder" {
        returnValue = PI * r * r * h
    }else if type == "Sphere" {
        returnValue = 4/3 * PI * r * r * r
    }else if type == "Cone" {
        returnValue = 1/3 * PI * r * r * h
    }
    return returnValue
}
var result = volumeOfCSV(type:"Cone", r:30, h:1)
print(result)
{% endhighlight %}
