---
layout: post
title: "Swift Type"
author: "younari"
---

> Swift Type Review

# 01. Integer
- **Int도 Instance이기 때문에, MAX와 MIN 프로퍼티가 존재한다.**
- **Int**: 부호를 포함하는 정수
- **UInt**: 0을 포함한 양의 정수
- **Int8비트,16비트,32비트,64비트**
- **UInt8비트,16비트,32비트,64비트**
- 시스템 아키텍쳐에 따라서 INT 기본 값이 달라짐
- 아이폰5 이상은 성능이 매우 좋아져서, 거의 64비트, 즉 Int가 8바이트
- 접두어에 따라 진수를 나타낼 수 있다. (2진법 0b, 8진법 0o, 16진법 0x)
- ex. **0xFFFFFF** -> 16진수


# 02. Bool

- **Swift** : ture / false
- **Objective C** : YES / NO


# 03. Double, Float
- **Double**: 64비트 (15자리)
- **Float**: 32비트 (6자리)
- **Double** 사용 권장


# 04. Character
- 단어나 문장이 아닌 문자 하나
- 스위프트는 유니코드 문자를 사용
- 영어는 물론, 유니코드 지원 언어, 특수 기호 모두 사용 가능
- 문자를 표현하기 위해서는 앞뒤에 쌍 따옴표(“ ”)를 붙여야 한다.



# 05. String
- String 역시 Instance 이기 때문에 문자열을 다루기 위한 다양한 기능(메소드)이 제공된다. 
- **hasPrefix, uppercased, isEmpty**등
- Interpolation(삽입)
- `var name:String = "Younari"`
- `print("my name is \(name)")`



# 06. Tuple
- Objective C에는 없는 타입, Swift에만 있음
- 함수의 리턴 타입으로 Tuple을 쓸 경우 유용함 : 한 방에 2개 리턴할 수 있음
- 타입의 조합으로 소괄호 안에 씀
- `(Int, String, Int)`
- 각 타입마다 이름을 지정해 줄수도 있다.
- `(name:String, age:Int)`
- 한 번에 a,b 변수 두개에 값 지정 가능
- `var (a,b) : (Int, String) = (3, "tuple")`

{% highlight swift %}
var coin:(Int,Int,Int,Int) = (3,1,5,3)
print("10원짜리 : \(coin.0)")
print("50원짜리 : \(coin.1)") 
print("100원짜리 : \(coin.2)") 
print("500원짜리 : \(coin.3)”)
{% endhighlight %}


{% highlight swift %}
var person:(name:String, age:Int, weight:Double) = ("joo", 30, 180.2)
print("몸무게 : \(person. weight)")
print("이름 : " + person.name)
print("나이 : \(person.age)")
{% endhighlight %}



# 07. Any, AnyObject, Nil
- Any : 스위프트 내의 모든 타입을 나타냄 = God of god
- `var list: [String:Any]`
- Any Object : 스위프트 내의 모든 객체 타입을 나타낸다.(클래스) = god
- nil : 데이터가 없음을 나타내는 키워드 ~ 초기화 조차 되지 않았다는 뜻
- Swift는 nil에 엄격함


# 08. 형 변환
- Swift에서는 타입 캐스팅의 정확한 의미는, 해당 타입의 Instance를 초기화 하며 생성하는 것으로, 즉 parameter에 다른 타입을 넣어서 init 하는 것이다. 아래 Double(total) 에서 () 즉, init 할 때 total 이라는 Int parameter 를 넣은 것으로, 초기화가 성공하면 형변환에 성공하는 것이라고 볼 수 있다.
- `var total: Int = 107`
- `var average: Double`
- `average = Double(total)/5`
- **Int to String** : stringNum = String(intNum)
- **Int to Double** : doubleNum = Double(intNum)
