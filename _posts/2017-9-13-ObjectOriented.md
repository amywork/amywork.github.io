---
layout: post
title: "객체 지향 프로그래밍"
author: "younari"
---

> 객체 지향 프로그래밍은 컴퓨터 프로그램을 여러 개의 독립된 단위, 즉 "객체"들의 모임으로 파악하고자 하는 것이다. 각각의 객체는 메시지를 주고받고, 데이터를 처리할 수 있다.

## ✔️ Class

- 같은 종류의 집단에 속하는 속성(attribute = variable)과 행위(behavior = function)를 정의한 것


## ✔️ Instance

- Class의 실체화된 인스턴스(객체)는 자신만의 고유한 속성(변수, 함수)을 가질 수 있으며, 클래스에서 정한 행위(함수, 메소드)를 수행할 수 있다. 객체의 행위는 클래스에 정의된 행위에 대한 정의를 공유함으로써 메모리를 경제적으로 사용한다. 
- 클래스를 실체화(Instance) 하면 객체(Object)를 만들수 있다.


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

## ✔️ Method
- 클래스로부터 생성된 객체를 사용하는 방법으로서 객체에 명령을 내리는 메시지라 할 수 있다.
- Method는 Class내에 선언된 함수를 말하며, 이는 객체에 의해 발현된다.

{% highlight swift %}

var s1 = Student(name: "Younari", id: 2011111050)
s1.setSubjects(subjects: [math, english, computer])
    
var test = cal.scoreAvgCal(student: s1)

{% endhighlight %}

<hr>

# 👀 객체의 속성

## **추상화(Abstraciton)**

- 클래스를 정의하는 것 : 공통의 속성이나 기능을 묶어 이름을 붙이는 것으로, 예를 들면 평균을 구하는 프로그램을 만들기 위해, 공통적으로 쓰이게 될 "과목", "학생", "연산" 이라는 각각의 Class를 만들어야겠다.

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

## **캡슐화(Encapsulation)**

- class안에 관련된 변수와 메소드를 묶어 넣는 것
- 코드의 수정 없는 재활용을 위한 작업이다.

	
## **은닉화**

- 메소드의 로직이나 멤버 변수들에 대해 외부로 보이지 않게 하는 것 
- 접근지정자: 클래스의 멤버에 접근할 수 있는 권한을 통제 (ex. private, public)
- 데이터에 대한 읽기 or 쓰기 권한 구분 가능
- 입력데이터의 유효성 검증 부분을 로직에 포함 시킬 수 있음 (ex. 음수는 입력 안되게끔 처리)

{% highlight swift %}

class Subject {
    private var name: String
    private var score: Int = 0
    // 외부에서 score에 setscore로 값을 할당은 할 수 있으나, 값을 직접 읽을 수는 없음

    func setScore(score: Int) {
        self.score = score
    } // method 함수 구현
    
    init(name: String) {
        self.name = name
    } // instance를 생성할 때, 무조건 name이 초기화 되고, 추후에 바꿀 수도 없음
}

{% endhighlight %}


## **상속성(Inheritance)**

- 상위 개념의 특징을 하위 개념이 물려받는 것
- 부모 클래스(super class, parent class)와 자식 클래스(sub class, child class)로 관계를 표현하며, 상속 할수록 더 확장되는 구조이다. 

{% highlight swift %}
class ViewController: UIViewController { }
{% endhighlight %}

- 상속은 새로운 클래스가 기존의 클래스의 자료와 연산을 이용할 수 있게 하는 기능
- *cf. Protocol은 채택한다, Class는 상속한다.*

{% highlight swift %}
class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        // super = 부모의 속성
}
{% endhighlight %}
        

## **다형성(Polymorphism)**

- 하나의 변수명, 함수명 등이 상황에 따라 다른 의미로 해석될 수 있는 것
- **재정의(Override)** : 부모 클래스에게서 물려받은 성질을 그대로 사용하지 않고 자식 클래스에게 맞는 형태 또는 행위로 변경하여 사용할 수 있는 기능
- **상속을 받은 클래스에 한해서** 부모 클래스의 속성(메소드)을 재정의 가능하다.

{% highlight swift %}
class ViewController: UIViewController {
    override func viewDidLoad() {
    // func viewdDidLoad는 원래 UIViewController 부모의 것을 재정의하겠다.
        super.viewDidLoad()
        // 부모꺼를 일단 호출해주고 아래에 내가  viewDidLoad() 재정의하겠다.
}
{% endhighlight %}
