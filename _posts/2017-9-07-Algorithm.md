---
layout: post
title: "알고리즘 공부하기"
author: "younari"
---

# Reading Note - 알고리즘

> [그림으로 개념을 이해하는 알고리즘](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968483547&orderClick=JAj), 저자: 아디트야 바르가바, 한빛 미디어

### 참고 자료
- [The Euclidean Algorithm](https://www.khanacademy.org/computing/computer-science/cryptography/modarithmetic/a/the-euclidean-algorithm)
- [Journey into cryptography](https://www.khanacademy.org/computing/computer-science/cryptography)
- [Algorithms, Khan Academy](https://www.khanacademy.org/computing/computer-science/algorithms)

# 이진 탐색


# 배열과 연결리스트
- 컴퓨터의 메모리는 거대한 서랍장과 같다.
- 여러개의 항목을 저장하고 싶을 때는 배열이나 리스트를 사용할 수 있다.
- 배열을 쓰면 모든 항목은 이웃하는 위치에 저장된다.
- 리스트를 쓰면 모든 항목이 흩어지지만, 각 항목은 다음 항목의 주소를 저장하고 있다.
- 배열은 읽기가 빠르다.
- 연결리스트는 삽입과 삭제가 빠르다.
- 배열의 모든 원소는 같은 자료형이어야 한다.

# Stack 자료구조
- 푸시(삽입)와 팝(떼어내고 읽기) 두가지 일만 할 수 있는 것

# Call stack
- 여러개의 함수를 호출하면서 함수에 사용되는 변수를 저장하는 스택을 호출스택이라고 한다.

# 재귀 함수
- 재귀함수에서의 스택 사용
- 재귀는 함수가 스스로를 호출하는 것이다.
- 모든 재귀 함수는 기본 단계와 재귀 단계라는 두 부분으로 나뉘어져 있다.
- 스택에는 푸시와 팝이라는 두가지 연산이 있다.
- 모든 함수 호출은 호출 스택을 사용한다.
- 호출 스택은 너무 커져서 메모리를 엄청나게 소비할 수 있다.
- 재귀 대신 반복문을 써서 코드를 작성할 수 있다.
- 배열을 포함하는 재귀 함수를 만들 때, 기본 단계는 보통 빈 배열이나 원소가 하나뿐인 배열이 된다.