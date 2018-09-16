---
layout: post
title: "Error Handling"
author: "Amy"
---

> Swift provides first-class support for throwing, catching, propagating, and manipulating recoverable errors at runtime.

- [Error Handling Official Documents](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html#//apple_ref/doc/uid/TP40014097-CH42-ID508)

# Error Enum: Error Protocol을 채택한 Error Enum을 만든다.

{% highlight swift %}
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
{% endhighlight %}

# 에러를 발생시키는 메소드 만들기
- `func foo(_:) throws {}`
- Error Protocol를 채택한 Enum만을 throw 할 수 있다.

{% highlight swift %}
func vend(itemNamed name: String) throws {
        guard let item = inventory[name] else {
            throw VendingMachineError.invalidSelection
        }
        
        guard item.count > 0 else {
            throw VendingMachineError.outOfStock
        }
        
        guard item.price <= coinsDeposited else {
            throw VendingMachineError.insufficientFunds(coinsNeeded: item.price - coinsDeposited)
        }
{% endhighlight %}

# Error를 Catch하는 방법

## 🙃 01. do-catch

{% highlight swift %}
do {
    try buyFavoriteSnack(person: "Alice", vendingMachine: vendingMachine)
} catch VendingMachineError.invalidSelection {
    print("Invalid Selection.")
} catch VendingMachineError.outOfStock {
    print("Out of Stock.")
} catch VendingMachineError.insufficientFunds(let coinsNeeded) {
    print("Insufficient funds. Please insert an additional \(coinsNeeded) coins.")
}
{% endhighlight %}

## 🙃 02. Converting to Optional Value

{% highlight swift %}
let x = try? someThrowingFunction()
{% endhighlight %}


# Sample Code

{% highlight swift %}
import Foundation
import UIKit

enum NumberCheckType: Error {
    case negativeNum
    case biggerNum
    case noData
}

class HandlingErrorViewController: UIViewController {

    func isSmallNum(baseNum: Int, targetNum: Int?) throws -> Int {
        guard let compareNum = targetNum else {
            throw NumberCheckType.noData
        }
        
        if baseNum < 0 || compareNum < 0 {
            throw NumberCheckType.negativeNum
        }
        
        if compareNum > baseNum {
            throw NumberCheckType.biggerNum
        }
        
        return compareNum
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        do {
            let num = try isSmallNum(baseNum: 10, targetNum: -1)
            print(num)
        }catch NumberCheckType.biggerNum {
            print("error.biggerNum, 더 작은 숫자를 입력해주세요")
        }catch NumberCheckType.noData {
            print("noData, 값을 입력해주세요")
        }catch NumberCheckType.negativeNum {
            print("negativeNum, 양수를 입력해주세요")
        }catch {
          print("설마 다른 에러가 있을까요?")
        }
    }

}
{% endhighlight %}
