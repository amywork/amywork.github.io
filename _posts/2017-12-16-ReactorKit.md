---
layout: post
title: "Reactor Kit"
author: "younari"
---

> **[Fastcampus RxSwift](http://www.fastcampus.co.kr/dev_camp_rxswift/)** 강좌 마지막 시간에 천재 수열님이 만들어주신 Reactor Kit을 잠깐 맛보았다. 뷰는 UI처리만 하게 하고, 비즈니스 로직들을 담은 모델은 Reactor가 감싸게끔 설계하는 방식. 스위프트를 객체 지향으로만 쓰는 것이 아니라 멀티 패러다임으로 사용하는 연습을 해보니 프로그래밍이 훨씬 재밌어진다. 🙃

# Reactor Kit
- **[ReactorKit](https://github.com/ReactorKit/ReactorKit)** :: A framework for reactive and unidirectional Swift application architecture

# Basic Concept
- UI와 비즈니스 로직을 분리하는 것
- View는 Action을 emit한다.
- Reactor는 State를 emit한다.

# Sample Project
- 리엑트킷 깃헙에도 예제로 나와있는 Counter를 직접 구현해보는 실습을 진행했다.

### import pods

{% highlight swift %}
import UIKit
import RxSwift
import RxCocoa
import ReactorKit
{% endhighlight %}

### ViewController
- `View` 프로토콜 채택
- `Reactor` 인스턴스 프로퍼티 구현
- `disposebag`
- `bind` 함수 구현

{% highlight swift %}
class ViewController: UIViewController, View {
    
    @IBOutlet weak var countLabel: UILabel!
    @IBOutlet weak var plusButton: UIButton!
    @IBOutlet weak var minusButton: UIButton!
    @IBOutlet weak var activityIndicator: UIActivityIndicatorView!
    
    var disposeBag: DisposeBag = DisposeBag()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        reactor = CounterViewReactor()
    }
    
    func bind(reactor: CounterViewReactor) {
        minusButton.rx.tap.map { CounterViewReactor.Action.decrease }
            .bind(to: reactor.action).disposed(by: disposeBag)
        
        plusButton.rx.tap.map { CounterViewReactor.Action.increase }
            .bind(to: reactor.action).disposed(by: disposeBag)
        
        reactor.state.map { $0.count }.map { "\($0)" }
            .bind(to: countLabel.rx.text).disposed(by: disposeBag)
        
        reactor.state.map { !$0.showIndicator }
            .distinctUntilChanged()
            .bind(to: activityIndicator.rx.isHidden).disposed(by: disposeBag)
        
        reactor.state.map { $0.showIndicator }
            .distinctUntilChanged()
            .bind(to: countLabel.rx.isHidden).disposed(by: disposeBag)
        
    }
    
}
{% endhighlight %}


### Reactor
- `Action`
- `Mutation`
- `State`
- `initialState`
- `func mutate` :: Return Observable Mutation
- `func reduce` :: Return Reactor.State

{% highlight swift %}

final class CounterViewReactor: Reactor {
    enum Action {
        case increase
        case decrease
    }
    
    enum Mutation {
        case increaseValue
        case decreaseValue
        case showIndicator
        case hideIndicator
    }
    
    struct State {
        var count: Int
        var showIndicator: Bool
    }
    
    let initialState: CounterViewReactor.State = State(count: 0, showIndicator: false)
    
    func mutate(action: CounterViewReactor.Action) ->
        Observable<CounterViewReactor.Mutation> {
            switch action {
            case .increase:
                return Observable.concat(
                    [Observable.just(Mutation.showIndicator),
                     Observable.just(Mutation.increaseValue)
                        .delay(0.5, scheduler: MainScheduler.instance),
                     Observable.just(Mutation.hideIndicator)])
                
            case .decrease:
                return Observable.concat(
                    [Observable.just(Mutation.showIndicator),
                     Observable.just(Mutation.decreaseValue)
                        .delay(0.5, scheduler: MainScheduler.instance),
                     Observable.just(Mutation.hideIndicator)])
            }
    }
    
    func reduce(state: CounterViewReactor.State,
                mutation: CounterViewReactor.Mutation) ->
        CounterViewReactor.State {
            switch mutation {
            case .increaseValue:
                return State(count: state.count + 1,
                             showIndicator: state.showIndicator)
            case .decreaseValue:
                return State(count: state.count - 1,
                             showIndicator: state.showIndicator)
            case .showIndicator:
                return State(count: state.count,
                             showIndicator: true)
            case .hideIndicator:
                return State(count: state.count,
                             showIndicator: false)
            }
    }
}



{% endhighlight %}
