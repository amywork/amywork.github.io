---
layout: post
title: "Thread & GCD"
author: "Amy"
---

## Thread?
> 어떠한 프로그램 내에서, 특히 **프로세스** 내에서 실행되는 흐름의 단위를 말한다. 프로그램 환경에 따라 둘 이상 의 스레드를 동시에 실행할 수 있다. 이러한 실행 방식을 멀티스레드(multithread)라고 한다.

# iOS Thread

### 모든 app은 기본적으로 main thread를 가지고 있다.

### UIKit은 오직 app의 main thread에서만 실행된다.
- 사용자의 인터렉션을 최우선으로 놓기 때문에, UI를 메인 스레드에서 처리하도록 강제한다. 유저의 터치에 즉시 반응할 수 있도록 하기 위해서.

### Multi Thread
- **동시 작업**이 필요한 경우
- 상태를 계속 감시해야 할 경우
- **네트워크 요청**을 해야 할 경우
- 이미지, 동영상, 음악 재생 및 다운로드
- 장시간 소요되는 액션일 경우
- **Reachability** : 디바이스의 네트워크 상태를 체크하는 것도 멀티스레드에서 동작이 된다.(WIFI 인지 Cellular 인지 체크해서 특정 액션을 취할 때)
- **Multithread를 사용할 경우 CPU의 멀티 코어 프로세스를 이용할 수 있다**
- cf. **GPU** : 그래픽 연산 프로세싱만 담당하는 프로세서
- cf. [멀티코어 프로세스 자료](https://namu.wiki/w/%EB%A9%80%ED%8B%B0%EC%BD%94%EC%96%B4%20%ED%94%84%EB%A1%9C%EC%84%B8%EC%84%9C)

### 교착상태(Dead lock)
- [데드락, 교착상태 해결 (Dead lock)에 대하여](http://goo.gl/jTXUXO)
- 멀티 쓰레드 환경에서, 한정된 자원을 사용하려고 서로 경쟁하는 상황을 교착상태라고 한다.
- 두 개 이상의 작업이 서로 상대방의 작업이 끝나기 만을 기다리고 있기 때문에 결과 적으로 아무것도 완료되지 못하는 상태!
- ex. 파일 업로드와 파일 오픈이 동시에 요청되었을 때 (업로드 후 오픈을 하게하는 등 선후관계를 명확히 하는 것이 좋다.)
- **atomically**: 자원에 대하여 교착상태를 피하기 위해 설정하는 값


### iOS에서 MultiThread를 사용하는 방법
1. **Thread** : 직접 thread를 만들어서 제어, 멀티 코어 지원 안함2. ⭐️ **GCD** : 클로저 기반, Operation에 비해 저수준 API, 멀티 코어 지원3. **Operation Queue** : GCD 기반의 wrapper class. 고수준의 API를 제공. (GCD보다는 한 단계 더 거치므로 성능이 조금 느린 편)4. ⭐️ **Perform Selector** : Selector를 이용한 방식, GCD 이후엔 많이 사용되진 않는다.5. ⭐️ **Timer** : 간단한 interval Notification(특정 시간 간격마다 체크)를 제공해 주는 Class. 단, Timer는 main Loop에서 실행된다.


### Timer 샘플코드

```
class ViewController: UIViewController {

    @IBOutlet weak var mainLB: UILabel!
    @IBAction func action(_ sender: UIButton) {
        longTimeAction2()
        mainLB.text = "Thread Test"
    }
    
    // Timer는 메인 스레드에서 돌지만, UI도 같이 돌리는게 가능하다.
    func longTimeAction1() {
        var count = 0
        Timer.scheduledTimer(withTimeInterval: 1, repeats: true) { (t) in
            self.mainLB.text = String(count)
            count += 1
        }
    }
    
    // MainLoop
    func longTimeAction2() {
        var total = 0
        Timer.scheduledTimer(withTimeInterval: 1, repeats: false) { (t) in
            for n in 1...1000000 {
                total += n
                print(total)
            }
        }
    }

}
```

<br>
<hr>
<br>


# GCD(Grand Central Dispatch)
- **비동기로 작업을 수행**하기 위한 가장 강력하고 쉬운 방법
- **System에서 Thread 관리**를 자동으로 해준다. (Multi Core Processing)
- **DispatchQueue**를 이용해 작업들을 컨트롤 한다. (GCD로 실행할 작업들을 관리하는 queue, 큐 하나가 스레드 하나!)
- **Work Item** : 클로저를 활용하며, 큐에 넣는 작업을 의미한다.
- **Grand Central Dispatch**에는 **Serial Dispatch Queue**와 **Concurrent Dispatch Queue**가 있다. 
- **Concurrent Dispatch Queue** : 큐에 있는 작업들을 동시에 별도의 스레드에 수행시킨다. 

## → DispatchQueue
- **3가지 큐의 종류**: main / global / custom queue

#### main: Main Thread, 기본 UI를 제어하는 Serial queue
- `DispatchQueue.main.async`

#### global: app 전역에 사용, Concurrent queue
- 큐에 우선순위 부여 가능
- let globalQueue = DispatchQueue.global(qos: .userInitiated)

#### custom: init으로 label 및 attribute, qos 등 옵션 설정 가능
- let customQueue = DispatchQueue(label: "com.amywork.queue1",attributes: [.concurrent, .initiallyInactive])

#### Attributes
- **Default**: 직렬
- **concurrent**: 병렬
- **initiallyInactive**: activate() 메소드를 통한 태스크 수동 실행

#### Quality Of Service? (QoS) : 우선순위
- userInteractive > userInitiated > default > utility > background > unspecified


## Dispatch Work Item
- 클로저로 만들어서 DispatchQueue에서 실행함.
- 아이템마다 QoS를 넣을 수도 있음.
- 중간에 cancel 가능.

### 실행하는 방법
- `.perform()` : DispatchQueue.global에서 실행됨
- `.async(execute: workItem)` : 내가 설정한 큐에서 실행됨
- 샘플코드 (이하)

```
func useWorkItem() {   var value = 10   let workItem = DispatchWorkItem {value += 5 }  
   workItem.perform()  
   let queue = DispatchQueue.global(qos: .utility)   queue.async(execute: workItem)  
   workItem.notify(queue: DispatchQueue.main) {       print("value = ", value)   }}```


<br>
<hr>
<br>


# 참고지식

### 참고지식: 동기/비동기
- **동기(synchronous)** : 프로그래밍 적으로 어떤 일이 끝난후 다음 행동을 하는 것- **비동기(Asynchronous)** : 어떤 일이 끝난 것과는 상관 없이 다음 행동을 하는 것
- iOS의 디자인패턴에 의한 비동기처리 방식의 예제 : **델리게이트(delegate)**, **셀렉터(@selector)**, **클로저(closure)**, **노티피케이션(Notification)**

### 참고지식: 네트워크를 통해 데이터를 주고 받는 속도
- 1G(텍스트전송),2G(음성통화),3G(영상통화),4G(100MB bps, 데이터 업로드)
- **LongTermRevolution**: LTE (3G를 오랜 기간 동안 빠르게 발전시킨 것)
- **네트워크는 Hand-Shaking 방식**이기 때문에 request와 response가 쌍방향으로 있어야 한다.