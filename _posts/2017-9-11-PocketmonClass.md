---
layout: post
title: "포켓몬 클래스 만들기"
author: "younari"
---

{% highlight swift %}
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
{% endhighlight %}
