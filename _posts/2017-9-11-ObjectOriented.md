---
layout: post
title: "객체 지향 프로그래밍"
author: "younari"
---


> 객체 지향 프로그래밍은 컴퓨터 프로그램을 명령어의 목록으로 보는 시각에서 벗어나 여러 개의 독립된 단위, 즉 "객체"들의 모임으로 파악하고자 하는 것이다. 각각의 객체는 메시지를 주고받고, 데이터를 처리할 수 있다.

## 👌🏻 Class

- 같은 종류의 집단에 속하는 속성(attribute = variable)과 행위(behavior = function)를 정의한 것


## 👌🏻 Object

- Class의 Instance이다. 객체는 자신의 고유한 속성(attribute)을 가질 수 있으며, 클래스에서 정한 행위를 수행할 수 있다. 객체의 행위는 클래스에 정의된 행l위에 대한 정의를 공유함으로써 메모리를 경제적으로 사용한다. 

### ✔️ 객체의 속성

- **추상화(Abstraciton)**
	- 클래스를 정의하는 것 : 공통의 속성이나 기능을 묶어 이름을 붙이는 것
	- ex. 평균을 구하는 프로그램을 만들기 위해, 공통적으로 쓰이게 될 "과목", "학생", "연산" 이라는 각각의 Class를 만들어야겠다.

- **캡슐화(Encapsulation)**
	- class안에 관련된 변수와 메소드를 묶어 넣는 것
	- 계층적으로 분류한 기능과 특성의 모음을 클래스(Class)라는 캡슐(capsule)에 분류된 집단 별로 각각 집어 넣는다. 
	- 클래스를 실체화(Instance) 하면 객체(Object)를 만들수 있다.
	- 코드의 수정 없는 재활용을 위한 작업이다.

	{% highlight swift %}
	class Subject {
    var name: String
    var score: Int = 0
    var grade: String = "F"
    var gradePoint: Double = 0.0
    var credit: Int = 1
    
    init(name: String) {
        self.name = name
    } // instance를 생성할 때, 무조건 name이 초기화 됨

    func setScore(score: Int) {
        self.score = score
    } // method 함수 구현
    }
	{% endhighlight %}
	
- **은닉화**
	- 메소드의 로직이나 멤버 변수들에 대해 외부로 보이지 않게 하는 것 
	- 접근지정자: 클래스의 멤버에 접근할 수 있는 권한을 통제 (private, public, protected)
	- 데이터에 대한 읽기 쓰기 권한 구분 가능
	- 입력데이터의 유효성 검증 부분을 로직에 포함 시킬 수 있음 (ex. 음수는 입력 안되게끔 처리)

- **상속성(Inheritance)**
	- 상위 개념의 특징을 하위 개념이 물려받는 것
	- 상속은 새로운 클래스가 기존의 클래스의 자료와 연산을 이용할 수 있게 하는 기능
	- 상속을 통해서 기존의 클래스를 상속받은 하위 클래스를 이용해 프로그램의 요구에 맞추어 클래스를 수정할 수 있고 클래스 간의 종속 관계를 형성함으로써 객체를 조직화할 수 있음
	- 상속을 사용해 부모 클래스의 특성과 기능을 그대로 이어받고 기능의 일부분을 변경해야 할 경우 상속 받은 자식 클래스에서 그 기능만을 다시 정의하여 수정
	- Super class란 상속을 위해 존재한다. 

- **다형성(Polymorphism)**
	- 하나의 변수명, 함수명 등이 상황에 따라 다른 의미로 해석될 수 있는 것


### ✔️ Class + Instance 선언법

{% highlight swift %}
01. Subject 클래스 생성
Class Subject {
	var name: String = ""
	var score: Int = 0
}

02. instance 생성
var newSubject: Subject = Subject()
// Subject() == Subject.init(){} == 초기화 method
{% endhighlight %}



## 👌🏻 Method
- 클래스로부터 생성된 객체를 사용하는 방법으로서 객체에 명령을 내리는 메시지라 할 수 있다.
- Method는 Class내에 선언된 함수를 말하며, 이는 객체에 의해 발현된다.



## 🤔 Question
- 아래 코드에서, Student 클래스가 Subject 클래스를 재사용 하고 있는데, 이게 상속이라고 볼 수 있나요?

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
    
    // 질문: 위에서, Student 클래스에서, Subjct 클래스를 상속받은 것인가요?
    // 수강 과목 하나를 받아서 넣어주는 Method
    func addSubject(subject: Subject) {
        self.subjects.append(subject)
    }
}
{% endhighlight %}



