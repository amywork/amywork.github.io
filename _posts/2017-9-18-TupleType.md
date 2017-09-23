---
layout: post
title: "스위프트 Tuple"
author: "younari"
---

# iOS and Swift Terminology - Tuple
##### Sources from [The Swift Programming Language ](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Types.html#//apple_ref/swift/grammar/tuple-type)

## What is a Tuple?
- A tuple type is a comma-separated list of types, enclosed in parentheses.

## Functions with Multiple Return Values
- You can use a tuple type **as the return type of a function to enable the function to return a single tuple containing multiple values.** You can also name the elements of a tuple type and use those names to refer to the values of the individual elements. An element name consists of an identifier followed immediately by a colon (:). 

### Sample code - Find MinMax

{% highlight swift %}

func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}

{% endhighlight %}


### Optional Tuple Return Types

{% highlight swift %}
func minMax(array: [Int]) -> (min: Int, max: Int)? {
    if array.isEmpty { return nil }
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
{% endhighlight %}
