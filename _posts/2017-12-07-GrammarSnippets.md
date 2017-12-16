---
layout: post
title: "Swift Grammar Snippets"
author: "younari"
---

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