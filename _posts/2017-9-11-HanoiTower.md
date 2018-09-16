---
layout: post
title: "재귀, 하노이의 탑"
author: "Amy"
---

### 재귀를 활용한 하노이 탑 알고리즘 해결하기

> A,B,C 세 개의 기둥과 기둥에 꽂을 수 있는 크기가 다양한 원판들이 있고, 퍼즐을 시작하기 전에는 A기둥에 원판들이 작은 것부터 큰 순서대로 쌓여 있다. 아래 두 가지 조건을 만족시키면서, 한 기둥에 꽂힌 원판들을 그 순서 그대로 다른 기둥으로 옮겨서 다시 쌓아야 한다.

- 한 번에 하나의 원판만 옮길 수 있다.
- 큰 원판이 작은 원판 위에 있어서는 안 된다.

{% highlight swift %}

// To move N disks from a peg A to peg B, you must first move N-1 disks from peg A to peg C, then move the last disk from A onto B and finally move N-1 disks from C to B. In other words to solve the Hanoi(N) problem you must first solve the Hanoi(N-1) problem two times. When you only have to move one disk you can just make the move.

func hanoi(_ N: Int, 
           from firstPeg: String = "A", 
           to secondPeg: String = "B", 
           using thirdPeg: String = "C") {
    if N == 1 {
        moveDisk(from: firstPeg, to: secondPeg)
    } else {
        hanoi(N - 1, from: firstPeg, to: thirdPeg, using: secondPeg)
        moveDisk(from: firstPeg, to: secondPeg)
        hanoi(N - 1, from: thirdPeg, to: secondPeg, using: firstPeg)
    }
}

hanoi(N)

{% endhighlight %}



[위키백과, 재밌는 지식](https://ko.wikipedia.org/wiki/%ED%95%98%EB%85%B8%EC%9D%B4%EC%9D%98_%ED%83%91) 64개의 원판을 옮기는 데 약 18446744073709551615번을 움직여야 하고, 한번 옮길 때 시간을 1초로 가정했을 때 64개의 원판을 옮기는 데 5849억 4241만 7355년 걸린다.