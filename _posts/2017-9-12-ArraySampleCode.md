---
layout: post
title: Data structure 01. Array
---

> 자료구조 - Array의 문법을 알아보고 함수를 작성해본다.

## ✨ Array

- 배열(영어: array)은 번호(인덱스)와 번호에 대응하는 데이터들로 이루어진 자료 구조를 나타낸다. 일반적으로 배열에는 같은 종류의 데이터들이 순차적으로 저장되어, 값의 번호가 곧 배열의 시작점으로부터 값이 저장되어 있는 상대적인 위치가 된다.
- Swift의 배열의 구조는 메모리 공간을 딱 한정하지는 않고, append와 같은 기능을 수행하기 위해 유동적으로 조정될 수 있다.

### Array Sample Code

- 배열을 선언하는 방법은 아래와 같다.

```swift
var friendsList:[String] = ["string1", "string2"]
var friendsList2:[String] = Array<String>()
var friendsList3:[String] = [String]()
```

- 배열에 값을 추가할 때는 `append` 메소드 호출

{% highlight swift %}

func addFriend(name: String) {
        friendsList.append(name)
}

addFriend(name: "Younari")
{% endhighlight %}

- 내가 원하는 데이터가 배열에 있는지 찾는 함수를 만들어보자.

{% highlight swift %}
func findFriend(name: String, targetList:[String]) -> Bool {
    for fname in targetList {
        if name == fname {
            return true
        }
    }
    return false
}

// 아래 두가지는 같은 결과를 return 할 것이다.
findFriend(name: "Younari", targetList: friendsList)
friendsList.contains("Younari")
{% endhighlight %}



- 특정 데이터의 인덱스를 찾아서, 해당 인덱스를 배열에서 지울 수도 있다.

{% highlight swift %}
func removeFriend(name: String) {
    if friendsList.contains(name) {
        let index: Int = friendsList.index(of: name)!
        friendsList.remove(at: index)
    }
}
{% endhighlight %}

### 배열의 대표적인 method
- `append`
- `contains`
- `remove`
- `insert`
- `count`
- `index`
