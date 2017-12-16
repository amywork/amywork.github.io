---
layout: post
title: "Code Snapshots"
author: "younari"
---

> 클로저, 튜플, map, flatMap 등 [Fastcampus swift](http://www.fastcampus.co.kr/dev_camp_rxswift/) 강좌를 들으며 재밌었던 코드 스니펫 모음

# Closure :: Curling

{% highlight swift %}

func add(value: Int) -> (Int) -> Int {
    return { value2 in return value + value2 }
}

let addTwo = add(value: 2)
addTwo(10)
add(value: 10)(2)

{% endhighlight %}

# Tuple :: with Named

{% highlight swift %}

let nameTuple: (first: Int, second: Int) = (3,6)
nameTuple.first
nameTuple.second

{% endhighlight %}

# Tuple :: with Switch

{% highlight swift %}

let tuple: (Int,Int) = (3,5)
switch tuple {
case let (a,_):
    print("a: \(a)")
}

{% endhighlight %}


# Map, FlatMap
- **Map**: Array의 원소들을 다른 원소들로 바꿔주는 것 :: Domain(X) -> Codomain(Y)
- `[T] -> T -> U -> [U]`
- **FlatMap**: Array 사이에 nil이 있을 때 nil을 제거하기 위함
- `[T] -> T -> U? -> [U]`

{% highlight swift %}

let array = [0,10,2,203,43,23,3241,32103,]
array.map { (item: Int) -> String in
    return "\(item+10)"
}

let stringArr = ["good", "https://google.com", "https://agit.io", "some"]
stringArr.flatMap { (string: String) -> String? in
    return URL(string: string)?.host
}

{% endhighlight %}
