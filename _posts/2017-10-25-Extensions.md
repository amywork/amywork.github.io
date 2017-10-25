---
layout: post
title: "Swift 고급 문법"
author: "younari"
---

> Subscript, Extensions, Generic

# Subscript
- 클래스, 구조체, 열거형의 collection, list, sequence의 멤버에 접근 가능한 단축문법
- **연산 프로퍼티에 Subscript라는 예약어가 있다고 생각하면 쉽다.**
- 사용시 **대괄호**

{% highlight swift %}

subscript(index: Type) -> Type {	get {
		// return an appropriate subscript value here
	}set(newValue) {
		// perform a suitable setting action here	}
}

{% endhighlight %}


{% highlight swift %}

enum CellMenuType {
    case infoMenu // (0,0)
    case changePW // (0,1)
    case logout // (0,2)
    case memberout // (0,3)
    case non
}

extension SettingTableViewCell {
    subscript (section: Int, row: Int) -> CellMenuType {
        if section == 0 {
            if row == 0 {
                return CellMenuType.infoMenu
            }else if row == 1 {
                return CellMenuType.changePW
            }else if row == 2 {
                return CellMenuType.logout
            }else if row == 3 {
                return CellMenuType.memberout
            }
        }
        return CellMenuType.non
    }
}

// 사용시 아래와 같이 대괄호
let cellMenuType = cell?[indexPath.section, indexPath.row]

{% endhighlight %}


# Extensions
- 코드 정리, 추상화를 위함
- 기존 클래스, 구조, 열거 형 또는 프로토콜 유형에 새로운 기능을 추가
- 저장 프로퍼티 제외하고 기능 추가 가능
- UITableViewDataSource 같이 부가적인 메소드들만 extension으로 따로 뺴서 정의 가능


# Generic
- 코드 정리, 추상화를 위함
- 어떤 타입에서도 활용 가능한 유연한 코드를 구현하기 위함


# Sample Code
### Struct Stack using generic
{% highlight swift %}
struct Stack<Z> {
    
    private var stackTemp: [Z] = []
    
    mutating func push(_ data: Z) {
        stackTemp.append(data)
    }
    
    mutating func pop() -> Z {
        return stackTemp.removeLast()
    }
    
}
{% endhighlight %}

### Struct Queue using generic
{% highlight swift %}
struct Queue<T> {
    
    private var queArr:[T] = []
    
    mutating func enqueue(_ data: T) {
        queArr.append(data)
    }
    
    mutating func dequeue() {
        queArr.removeFirst()
    }
    
}
{% endhighlight %}


### func `swap` using generic
{% highlight swift %}
func swapTwoValues<T>(_ a: inout T, _ b: inout T) { 
	let temporaryA = a	a=b	b = temporaryA}
{% endhighlight %}
