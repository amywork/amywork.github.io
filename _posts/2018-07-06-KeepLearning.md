---
layout: post
title: "Keep Learning"
author: "Amy"
---


# objc.io review note

> I'm subscribing **objc.io** online contents on monthly plan. It is an platform which provides **In-depth** guides on **iOS** and macOS development. This post is for keeping track of the Swift coding concepts that I encountered through objc.io.

<br>

# Delegates & Callbacks
✳︎ [Swift Talk #24 Delegates & Callbacks](https://talk.objc.io/episodes/S01E24-delegates-callbacks)

#### ↳ NOTE
- Another advantage of delegate protocols is that you can group multiple methods that belong together. Furthermore, it's relatively easy to implement an API where you can register and unregister multiple delegates. This is much more difficult with callback functions, e.g. you'd need to return a token when registering a callback, which you can use later on to unregister this specific callback.

<br>

# Sharing State between VCs
✳︎ [Swift Talk #86, #87](https://talk.objc.io/episodes/S01E86-sharing-state-between-view-controllers-in-mvc-part-1)

#### ↳ NOTE
- Tip. Embedding ViewControllers through Container
- Opinion. It seems container view's viewDidLoad is called earlier than owner.


<br>

# Collection protocol 
✳︎ [Swift Talk #32 Array, ArraySlice & Collection](https://talk.objc.io/episodes/S01E32-array-arrayslice-collection)

#### ↳ NOTE
- For me, Collection protocol is an amazing & interesting topic. :)

```
extension Collection {
    func split(batchSize: IndexDistance) -> [SubSequence] {
        var remainderIndex = startIndex
        var result: [SubSequence] = []
        while remainderIndex < endIndex {
            let batchEndIndex = index(remainderIndex, offsetBy: batchSize, limitedBy: endIndex) ?? endIndex
            result.append(self[remainderIndex..<batchEndIndex])
            remainderIndex = batchEndIndex
        }
        return result
    }
}
```

<br>

# Auto Layout with Key Paths
✳︎ [Swift Talk #75 Auto Layout with Key Paths](https://talk.objc.io/episodes/S01E75-auto-layout-with-key-paths)



<br>

# === : ==
- === is really asking: “Do both these variables **hold the same reference as their value?**” 
- Programming language literature, == is sometimes called **structural equality**, and === is called **pointer equality** or **reference equality**. 

<br>

# Reference

- A variable that holds a reference can be declared with let — that is, the reference is constant. This means that the variable can never be changed to refer to something else. 
- But **it doesn’t mean that the object it refers to can’t be changed.**
— **it’s only constant in what it points to. It doesn’t mean what it points to is constant. 
**

<br>

# Polymorphic features

- Subtyping and **method overriding** is one way of getting polymorphic behavior, - generics, where a function or method is written once to take any type that provides certain functions or methods, but the implementations of those functions can vary. 
- Unlike method overriding, the results of function overloading and generics are known statically at compile time. 