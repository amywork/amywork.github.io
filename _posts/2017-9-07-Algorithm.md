---
layout: post
title: "자료구조와 알고리즘"
author: "younari"
---

# ✔️ Reading Note - 알고리즘
> 도서 - [그림으로 개념을 이해하는 알고리즘 / 아디트야 바르가바](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968483547&orderClick=JAj)

- 시작 기간: 2017년 9월 초
- 목표 기간: 2017년 11월 말
- **목표 01**: 주어진 문제를 어떠한 방식으로 해결할 수 있을까?
- **목표 02**: 코드의 속도를 어떻게 하면 빠르게 할 수 있을까?
- **목표 03**: 코드를 어떻게 더 정리해서 알아보기 쉽게 만들 수 있을까?


### 참고 자료
- [The Euclidean Algorithm](https://www.khanacademy.org/computing/computer-science/cryptography/modarithmetic/a/the-euclidean-algorithm)
- [Journey into cryptography](https://www.khanacademy.org/computing/computer-science/cryptography)
- [Algorithms, Khan Academy](https://www.khanacademy.org/computing/computer-science/algorithms)

# 배열과 연결리스트
- 컴퓨터의 메모리는 거대한 서랍장과 같다.
- 여러개의 항목을 저장하고 싶을 때는 배열이나 리스트를 사용할 수 있다.
- 배열을 쓰면 모든 항목은 이웃하는 위치에 저장된다.
- 리스트를 쓰면 모든 항목이 흩어지지만, 각 항목은 다음 항목의 주소를 저장하고 있다.
- 배열은 읽기가 빠르다.
- 연결리스트는 삽입과 삭제가 빠르다.
- 배열의 모든 원소는 같은 자료형이어야 한다.
- 연결리스트는 데이터 덩어리(노드 Node)를 저장할 때 그 다음 순서의 자료가 있는 위치를 데이터에 포함시키는 방식으로 자료를 저장한다.
- 배열에 비해 데이터의 추가/삽입 및 삭제가 용이하나 순차적으로 탐색하지 않으면 특정 위치의 요소에 접근할 수 없어 일반적으로 탐색 속도가 떨어진다.
- 탐색 또는 정렬을 자주 하면 배열을, 추가/삭제가 많으면 연결 리스트를 사용하는 것이 유리하다. 
- 배열은 쓰지않는 공간까지 전부 예약해두고 있어야 하기 때문에 공간 낭비가 생긴다.

# 선택 정렬
- [정렬 알고리즘 개요](https://namu.wiki/w/%EC%A0%95%EB%A0%AC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98?from=%EC%84%A0%ED%83%9D%20%EC%A0%95%EB%A0%AC#s-2.1.2)
- [스위프트 알고리즘 클럽 예제 코드](https://github.com/raywenderlich/swift-algorithm-club/tree/master/Selection%20Sort)

# 재귀 함수
- 재귀함수에서의 스택 사용 
- 재귀는 함수가 스스로를 호출하는 것이다.
- 모든 재귀 함수는 기본 단계와 재귀 단계라는 두 부분으로 나뉘어져 있다.
- 스택에는 푸시와 팝이라는 두가지 연산이 있다.
- 모든 함수 호출은 호출 스택을 사용한다.
- 호출 스택은 너무 커져서 메모리를 엄청나게 소비할 수 있다.
- 재귀 대신 반복문을 써서 코드를 작성할 수 있다.
- 배열을 포함하는 재귀 함수를 만들 때, 기본 단계는 보통 빈 배열이나 원소가 하나뿐인 배열이 된다.

# Stack
- 후입선출(後入先出, Last In First Out; LIFO)
- 입력은 push, 출력은 pop
- 푸시(삽입)와 팝(떼어내고 읽기) 두가지 일만 할 수 있는 것

# Call stack - 호출스택
- 여러개의 함수를 호출하면서 함수에 사용되는 변수를 저장하는 스택

# Queue
- 선입선출(先入先出, First In First Out; FIFO), 대기열
- 입력 동작은 Enqueue, 출력 동작은 Dequeue
- 어떠한 작업/데이터를 순서대로 실행/사용하기 위해 대기시킬 때 사용
- [스위프트 알고리즘 클럽](https://github.com/raywenderlich/swift-algorithm-club/tree/master/Queue)

# Binary Search - 이진탐색
- [이진 탐색(Binary Search)의 탐색 과정](http://blog.eairship.kr/246)
- [raywenderlich - Binary Search](https://github.com/raywenderlich/swift-algorithm-club/tree/master/Binary%20Search)
- 탐색 대상이 절반씩 줄어들기 때문에 시간 복잡도는 O(logn)

{% highlight swift %}

func binarySearch<T:Comparable>(inputArr:Array<T>, searchItem: T) -> Int? {
    var lowerIndex = 0;
    var upperIndex = inputArr.count - 1

    while (true) {
        let currentIndex = (lowerIndex + upperIndex)/2
        if(inputArr[currentIndex] == searchItem) {
            return currentIndex
        } else if (lowerIndex > upperIndex) {
            return nil
        } else {
            if (inputArr[currentIndex] > searchItem) {
                upperIndex = currentIndex - 1
            } else {
                lowerIndex = currentIndex + 1
            }
        }
    }
}

var myArray = [1,2,3,4,5,6,7,9,10];
if let searchIndex = binarySearch(myArray,5){
    println("Element found on index: \(searchIndex)");
}
{% endhighlight %}

# Insertion Sort
- [스위프트 알고리즘 클럽](https://github.com/raywenderlich/swift-algorithm-club/tree/master/Insertion%20Sort)

# Quick Sort
- [스위프트 알고리즘 클럽](https://github.com/raywenderlich/swift-algorithm-club/tree/master/Quicksort)
- **partitioning** : pivot을 중심으로 pivot보다 작은 수들을 sort 하고, 큰 수들을 sort 해서 합치는 것

{% highlight swift %}
func quicksort<T: Comparable>(_ a: [T]) -> [T] {
  guard a.count > 1 else { return a }

  let pivot = a[a.count/2]
  let less = a.filter { $0 < pivot }
  let equal = a.filter { $0 == pivot }
  let greater = a.filter { $0 > pivot }

  return quicksort(less) + equal + quicksort(greater)
}
{% endhighlight %}

# Hash Table
- [스위프트 알고리즘 클럽](https://github.com/raywenderlich/swift-algorithm-club/tree/master/Hash%20Table)
- **[ Key : Value ]**
- 해시테이블은 해시함수와 배열을 결합해서 만든다.
- 대부분의 프로그래밍 언어는 해시테이블을 지원한다.
- 딕셔너리, 연관 배열, maps
- 어떤 것과 다른 것 사이의 관계를 모형화하는 것
- 중복을 막을 수 있다.
- 서버에게 작업을 시키지 않고, 자료를 캐싱할 수 있다.
- 충돌을 줄이는 해시 함수가 좋은 함수이다.
- 사용률이 0.7보다 커지면 해시 테이블을 리사이징 한다.
- 평균적인 경우 해시 테이블의 성능은 상수 시간이다. O(1)
- It is difficult to predict where in the array your objects end up. Hence, dictionaries **do not guarantee any particular order of the elements in the hash table.**


# Graph, 너비 우선 탐색
- [스위프트 알고리즘 클럽](https://github.com/raywenderlich/swift-algorithm-club/tree/master/Graph)
- 최단 경로를 찾는 방법
- 탐색 목록은 큐가 되어야 한다.
- 누군가 확인한 후 두번 다시 확인하지 않도록 해야한다.

# 다익스트라 알고리즘

# 탐욕 알고리즘
- 정확한 답을 구할 수 없을 때, 가장 가까운 답을 구하는 방법

# 동적 프로그래밍
- 어려운 문제를 여러개의 하위 문제로 쪼개고 하위 문제들을 먼저 푸는 방법

# KNN 알고리즘
- 유사도를 통해 분류나 반응을 예측하는 회귀에 주로 사용되는 머신러닝의 한 종류

# Tree, 트리

# 역 인덱스

# 퓨리에 변환

# 병렬 알고리즘

# 맵 리듀스

