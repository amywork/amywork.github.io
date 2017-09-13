---
layout: post
title: "클래스 구현 시나리오"
author: "younari"
---
## 평균을 구하기 위해서는 어떤 CLASS가 있어야 할까?
🤔 과목 클래스에 과목 이름, 성적 등을 담자. <br>
🤔 학생 클래스에 학생 이름, 수강 과목, 총합 점수 등을 담자. <br>
🤔 연산 클래스에서는 학생이름을 입력하면 평균 점수를 반환하는 메소드를 담자. <br>


**- 과목 클래스**

{% highlight swift %}
class Subject {
    var name: String
    var score: Int = 0
    var grade: String = "F"
    var gradePoint: Double = 0.0
    var credit: Int = 1
    
    func setScore(score: Int) {
        self.score = score
    } // method 함수 구현
    
    init(name: String) {
        self.name = name
    } // instance를 생성할 때, 무조건 name이 초기화 됨
}
{% endhighlight %}

**- 학생 클래스**

{% highlight swift %}
class Student {
    var name: String
    var studentID: Int
    
    var subjects: [Subject] = []
    var totalScore: Int = 0
    
    var totalGrade: String = "F"
    var totalGradePoint: Int = 0
  
    init(name: String, id: Int) {
        self.name = name
        self.studentID = id
    }
    
    // 수강 과목들을 모두 받아서 넣어주는 Method
    func setSubjects(subjects:[Subject]) {
        self.subjects = subjects
    }

    // 수강 과목 하나를 받아서 넣어주는 Method
    func addSubject(subject: Subject) {
        self.subjects.append(subject)
    }
}
{% endhighlight %}

**- 연산 클래스**

{% highlight swift %}

class CreditCalculation {
    
    func scoreAvgCal(student: Student) -> Int {
        let avgScore = avgSubjectScores(subjects: student.subjects)
        student.totalScore = avgScore
        return student.totalScore
    }
    
    func avgSubjectScores(subjects:[Subject]) -> Int {
        
        var totalScore: Int = 0
        for sbj in subjects {
            totalScore += sbj.score
        }

        let avgScore: Int = totalScore / subjects.count
        return avgScore
    }
}
{% endhighlight %}

<br>

## 접근 지정자 : Private

- 만약 Subject의 Score를 Write만 할 수 있고, Read는 할 수 없게 구현하고 싶다면?
- Class 내에서 변수 score를 선언할 때 ***private***으로 선언한다.

{% highlight swift %}
class Subject {
    private var name: String
    private var score: Int = 0
    -> 외부에서 score에 setscore로 값을 할당은 할 수 있으나, 값을 직접 읽을 수는 없음

    func setScore(score: Int) {
        self.score = score
    } // method 함수 구현
    
    init(name: String) {
        self.name = name
    } // instance를 생성할 때, 무조건 name이 초기화 되고, 추후에 바꿀 수도 없음
}
{% endhighlight %}