---
layout: post
title: "스위프트 열거형(Enum) 알아보기"
author: "younari"
---

# Swift Terminology - Enumeration
##### Sources from [The Swift Programming Language (Swift 4)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html), [Appventure.me](https://appventure.me/2015/10/17/advanced-practical-enum-examples/)

> An enumeration defines a common type for a group of related values and enables you to work with those values in a type-safe way within your code.

# 필요한 상황
- 유한한 경우의 수가 있을 때, 케이스(+때로는 연관된 값)를 선언하는 것
- **Define any hierarchically organized data**
- **Keeping related information together.**


# 기본 문법
```
enum CompassPoint {    case north    case south    case east    case west}
```

- `var directionToHead = CompassPoint.west`- `directionToHead = .north`


# 스위치문과 함께 사용하기
-  Enum을 활용하는 패턴 매칭
-  You can use various pattern matching constructs through enums

{% highlight swift %}

directionToHead = .south
switch directionToHead {
case .north:
    print("Lots of planets have a north")
case .south:
    print("Watch out for penguins")
case .east:
    print("Where the sun rises")
case .west:
    print("Where the skies are blue")
}
// Prints "Watch out for penguins"

{% endhighlight %}


# 값 할당하기 (Associated Values)

### 01. 값을 할당하는 "바코드" 열거형 만들기

{% highlight swift %}
enum Barcode {
    case upc(Int, Int, Int, Int) // 이건 어찌보면 튜플이 할당된 케이스!
    case qrCode(String)
}
{% endhighlight %}

### 02. 바코드의 인스턴스 만들기

{% highlight swift %}
var productBarcode: Barcode = Barcode.upc(1, 2, 3, 4)
productBarcode = .qrCode("BONJOUR")
{% endhighlight %}


### 03. 스위치문 사용하기

##### 👋🏻 스위프트 구문 안에서 값 바인딩 시키기

{% highlight swift %}
switch productBarcode {
    case .upc(let numberSystem, let manufacturer, let product, let check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
    case .qrCode(let productCode):
    print("QR CODE: \(productCode).")
}
{% endhighlight %}


##### 👋🏻 와일드카드를 사용할 수도 있다.

{% highlight swift %}
switch bookBarcode {
    case .upc(let numberSystem, _, _, _) where numberSystem == 8:
        print("This is imported Journals.")
    case .qrCode(let productCode):
        print("QR CODE: \(productCode).")
	default:
    print("default")
}
{% endhighlight %}


### 04. 예제: 바코드 첫번째 숫자로 책 타입을 구분해보기

{% highlight swift %}
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}

var kinfolk: Barcode = Barcode.upc(8, 2, 3, 4)
var photography: Barcode = Barcode.upc(111, 2, 3, 4)
var foody: Barcode = Barcode.upc(11, 2, 3, 4)

func checkBookType(bar: Barcode) {
    switch bar {
    case .upc(let bookType, _, _, _) where bookType < 10 && 0 < bookType:
        print("This is a journal.")
    case .upc(let bookType, _, _, _) where bookType >= 10 && 20 > bookType:
        print("This is a cook Book.")
    case .upc(let bookType, _, _, _) where bookType >= 20 && 500 > bookType:
        print("This is a photography book.")
    default:
        print("Anything else")
    }
}

checkBookType(bar: kinfolk)
checkBookType(bar: photography)
checkBookType(bar: foody)
{% endhighlight %}

# Enum Values: Raw Value
- enum 선언시 타입을 Character, Int, String 으로 지정하고 원시값 지정이 가능하다.
- Enumeration cases can come **prepopulated** with default values (called raw values), which are **all of the same type.**

### Mapping to String
{% highlight swift %}
enum House: String {
    case Baratheon = "Ours is the Fury"
    case Greyjoy = "We Do Not Sow"
    case Martell = "Unbowed, Unbent, Unbroken"
    case Stark = "Winter is Coming"
    case Tully = "Family, Duty, Honor"
    case Tyrell = "Growing Strong"
}

{% endhighlight %}


### Mapping to Integer, 디폴트는 0부터 ~

{% highlight swift %}
enum Planet: Int {
    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
} // mercury = 0

let possiblePlanet:Planet = Planet(rawValue: 1)! // venus
print(possiblePlanet.rawValue) // 1
{% endhighlight %}

### Mapping to Integer, 시작값 지정 가능

{% highlight swift %}
enum Planet: Int {
    case mercury=3, venus, earth, mars, jupiter, saturn, uranus, neptune
} // mercury = 3

let possiblePlanet:Planet = Planet(rawValue: 1)! // venus
print(possiblePlanet.rawValue) // 1
{% endhighlight %}

### Mapping to floating point 
- also note the fancy unicode in enum cases

{% highlight swift %}
enum Constants: Double {
    case π = 3.14159
    case e = 2.71828
    case φ = 1.61803398874
    case λ = 1.30357
}
{% endhighlight %}


# Properties
- 실제적인 stored properties 를 저장할 수 없다고 하더라도, computed properties를 활용해볼 수 있다. 

{% highlight swift %}
enum Device {
  case iPad, iPhone
  var year: Int {
    switch self {
	case iPhone: return 2007 // enum의 케이스 하위에 year 생성됨
	case iPad: return 2010
     }
  }
}
{% endhighlight %}


