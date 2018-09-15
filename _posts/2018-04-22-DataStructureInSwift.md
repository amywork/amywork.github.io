---
layout: post
title: "Algorithms in Swift"
author: "Amy"
---

## Data Structures & Algorithms in Swift

> [RayWenderlich](https://store.raywenderlich.com/products/data-structures-and-algorithms-in-swift) 에서 "스위프트 자료구조와 알고리즘" 책이 새롭게 출간되었습니다. Swift Standard Library 부터 Queues, Linked List, Trees, Binary Search 등등  자료 구조와 알고리즘에 대한 전반적인 내용을 스위프트의 시선으로 탐구해볼 수 있습니다. 대부분의 자료구조와 알고리즘 책들은 주로 C나 자바로 쓰여있었기 때문에 선뜻 손이 가지 않았는데요. 저처럼 스위프트를 좋아하는 주니어 iOS 개발자분들이라면 이 책이 자료구조와 알고리즘을 공부하는 데 좋은 시작점이 될 수 있을 것 같아요! :) 


### Constant time O(1) operation
### linear time O(n) operation

# Array Performance

- `Sequence` → iterate
- `Collection Protocol` → subscript operator

### operation
- `Random access` → O(1) operation 
- `count` → O(1) operation 
- `capacity` → O(1) operation → 용량 부족으로 capacity 를 늘려야 할 경우, capacity를 2배로 늘린다.
- `Lookup` → O(n) operation 
- `Insertion` → O(n) operation

# Dictionary Performance

- `Collection Protocol` → subscript operator

### operation
- `Lookup` → O(1) operation 
- `Insertion` → O(1) operation


# Linked List Performance
- a chain of nodes
- 값을 저장하는 동시에, 다음 노드의 주소를 저장한다.
- `head`, `tail`, `insert(after: Int)`, `node(at: Int)`

### operation
- `Lookup` → O(1) operation 
- `Insertion` → O(1) operation


```
public class Node<Value> {
	var value: Value?
	var next: Node?
	
	init(value: Value, next: Node? = nil) {
		self.value = value
		self.next = next
	}
}
```