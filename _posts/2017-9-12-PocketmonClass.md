---
layout: post
title: 포켓몬 Class 만들기
---

> 객체 지향 프로그래밍은 컴퓨터 프로그램을 명령어의 목록으로 보는 시각에서 벗어나 여러 개의 독립된 단위, 즉 "객체"들의 모임으로 파악하고자 하는 것이다. 각각의 객체는 메시지를 주고받고, 데이터를 처리할 수 있다.


```swift
class Pocketmon {
    
    var name: String = ""
    var level: Int = 1
    var isFly: Bool = false
    var lifeGage: Int = 100
    var item: [String] = []
    
    func flyToSky() {
        if isFly {
            print(name + "가 하늘을 날았습니다.")
        }else {
            print(name + " 는 하늘을 날 수 없습니다.")
        }
    }
    
    func levelUP(grade: Int) {
        self.level = level + grade
        print("레벨이 \(grade)단계 Upgrade 되었습니다.")
    }
    
    func attack(victim: String, count: Int) {
        print(name + "가 " + victim + "을 \(count)대 때렸습니다.")
    }
    
    func gageDown(gage: Int) {
        print("gage가 \(gage)만큼 줄었습니다.")
        self.lifeGage = lifeGage - gage
        print(name + "에게 남아있는 life Gage는 " + "\(lifeGage)입니다.")
    }
}
```
