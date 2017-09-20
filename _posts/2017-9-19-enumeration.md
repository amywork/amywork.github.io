---
layout: post
title: "Swift Enum"
author: "younari"
---

# Swift Terminology - Enumeration
##### Sources from [The Swift Programming Language (Swift 4)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html)

> An enumeration defines a common type for a group of related values and enables you to work with those values in a type-safe way within your code.


# 기본 문법
- 다른 언어에서는 타입:값을 매칭해두지만, 스위프트에서는 열거된 항목 자체(=케이스)이다.

```
enum CompassPoint {    case north    case south    case east    case west}
```

- `var directionToHead = CompassPoint.west`- `directionToHead = .north`


# 스위치문과 함께 사용하기

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
    case upc(Int, Int, Int, Int)
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

# Raw Value
- enum 선언시 타입을 Character, Int, String 으로 지정하고 원시값 지정이 가능하다.
- Enumeration cases can come **prepopulated** with default values (called raw values), which are **all of the same type.**

{% highlight swift %}
enum ASCIIControlCharacter: Character {
    case tab = "\t"
    case lineFeed = "\n"
    case carriageReturn = "\r"
}
{% endhighlight %}


### 디폴트는 0부터 ~

{% highlight swift %}
enum Planet: Int {
    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
} // mercury = 0

let possiblePlanet:Planet = Planet(rawValue: 1)! // venus
print(possiblePlanet.rawValue) // 1
{% endhighlight %}

### 값 지정 가능

{% highlight swift %}
enum Planet: Int {
    case mercury=3, venus, earth, mars, jupiter, saturn, uranus, neptune
} // mercury = 3

let possiblePlanet:Planet = Planet(rawValue: 1)! // venus
print(possiblePlanet.rawValue) // 1
{% endhighlight %}


# Recursive Enumerations
- 재귀열거형은 다른 인스턴스 열거형이 Associated Values로 사용되는 열거형이다.
