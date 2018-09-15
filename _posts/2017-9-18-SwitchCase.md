---
layout: post
title: "스위프트 Switch"
author: "Amy"
---

> 패턴 비교문: Switch 와 Case 문법

# 01. Swith
- **패턴 비교문으로, 가장 첫번째 매칭되는 패턴의 구문이 실행된다.**

### ✌🏻 스위치문 속에 튜플 넣어보기
- 사용 가능한 모든 값에 대한 매칭은 **와일드 카드( _ )**를 통해서 매칭할 수도 있다.


{% highlight swift %}
func getPoint(somePoint:(Int,Int)) {
     switch somePoint {
     case (0, 0):
         print("\(somePoint) is at the origin")
     case (_, 0):
         print("\(somePoint) is on the x-axis")
     case (0, _):
         print("\(somePoint) is on the y-axis")
     case (-2...2, -2...2):
         print("\(somePoint) is inside the box")
     default:
         print("\(somePoint) is outside of the box")
     }
}
{% endhighlight %}


### ✌🏻 값 Binding & Where 문법 사용하기
- case 내부에서 사용되는 임시 값으로 매칭 시킬수 있다

{% highlight swift %}
func whereTopoint(point(Int,Int)) {
	switch point {
		case let (x,y) where x == y:
			print("(\(x),\(y)) is on the line x = y")
		case let (x,y) where x == -y:
			print("(\(x),\(y)) is on the line x = -y")
		case let (x,y):
			print("(\(x),\(y)) is just some arbitrary point")
	}
}
{% endhighlight %}


### ✌🏻 학점 계산기 ~ 과목 클래스를 스위치문으로 바꿔보기
#### 01. 범위 연산자, Interval Matching
- **범위 연산자**를 통해 해당 범위에 해당하는 value를 매칭할 수 있다.

#### 02. Score를 입력받아 Grade Point로 변환하고 Grade로 최종 변환한다.

{% highlight swift %}

class Subject {
    var name: String
    var score: Int = 0
    var grade: String = "F"
    var gradePoint: Double = 0.0
    
    init(name: String) {
        self.name = name
    } // instance를 생성할 때, 무조건 name 초기화
    
    
    func setScore(score: Int) {
        self.score = score
        scoreToGrade(score)
        gradeToGradepoint(grade: grade)
    } // method 함수 구현
    
    
    func scoreToGrade(_: Int) -> String {
        switch score {
        case 95...100:
        	grade = "A+"
        case 90..<95:
		grade = "A"
        case 85..<90:
		grade = "B+"
        case 80..<85:
		grade = "B"
        case 75..<80:
		grade = "C+"
        case 70..<75:
        	grade = "C"
        default:
		grade = "F"
	}
        return grade
    }
    
    
    func gradeToGradepoint(grade: String) -> Double {
        switch grade {
        case "A+":
            gradePoint = 4.0
        case "A":
            gradePoint = 3.5
        case "B+":
            gradePoint = 3.0
        case "B":
            gradePoint = 2.5
        case "C+":
            gradePoint = 2.0
        case "C":
            gradePoint = 1.5
        default:
            gradePoint = 0.0
        }
        return gradePoint
    }
    
}

{% endhighlight %}
