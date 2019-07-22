---
layout: post
title: "Thread Programming"
author: "Amy"
---

<br>
# 관련 아티클
- [Concurrency Programming Guide](https://www.objc.io/issues/2-concurrency/concurrency-apis-and-pitfalls/)
- [Low Level Concurrency APIs](https://www.objc.io/issues/2-concurrency/low-level-concurrency-apis/)
- [instruments / CPU Usage](https://help.apple.com/instruments/mac/current/)
- [Common Background Practices](https://www.objc.io/issues/2-concurrency/common-background-practices/)
- [Apple Document](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/AboutThreads/AboutThreads.html#//apple_ref/doc/uid/10000057i-CH6-SW3)


<br>
<br>

# Concurrency API 
- `pthread` and `NSThread` // 스레드를 직접 만드는 것은 비권장 (problem of too many threads being created)
- `Grand Central Dispatch`, `NSOperationQueue` // queue-based concurrency APIs
- `NSRunLoop`

<br>
<br>

# Threading Programming

✓ Threads are subunits of processes, which can be scheduled independently by the operating system scheduler.
- 스레드는 프로세스의 서브 유닛이고, OS 스케줄러에 의해 독립적으로 스케줄 될 수 있다.

✓ Virtually all concurrency APIs are built on top of threads under the hood – that’s true for both Grand Central Dispatch and operation queues.
- 모든 concurrency API(GCD, Operation Queues) 들은 스레드 위에서 지어진다.

✓ The term thread is used to refer to a separate path of execution for code.
- 스레드란, 실행하는 코드들의 분리된 길 (1앱 N스레드.. 실타래 같은 것.)

✓ The term process is used to refer to a running executable, which can encompass multiple threads.
- 프로세스란, 멀티 스레드를 아울러서 실행되는 코드. (1앱 1프로세스)

✓ The term task is used to refer to the abstract concept of work that needs to be performed.
- 테스크란, 실행해야 하는 일 묶음애 대한 추상적인 단위

✓ From a technical standpoint, a thread is a combination of the kernel-level and application-level data structures needed to manage the execution of code. 
- 코드 실행을 관리하기 위한 **커널 레벨**과 **어플리케이션 레벨**의 데이터 구조 조합

✓ The kernel-level structures coordinate the dispatching of events to the thread and the preemptive scheduling of the thread on one of the available cores.
- **커널 레벨**에서는 **이벤트를 스레드로 디스패치** 하는 것을 조정하고, **사용 가능한 코어(CPU)에다가 스케줄링**을 하게 된다.

✓ The application-level structures include the call stack for storing function calls and the structures the application needs to manage and manipulate the thread’s attributes and state.
- **어플리케이션 레벨**에서는 **함수 콜을 위한 stack 및 스레드의 속성과 상태를 관리**한다.

✓ After starting a thread, the thread runs in one of three main states: running, ready, or blocked. If a thread is not currently running, it is either blocked and waiting for input or it is ready to run but not scheduled to do so yet. The thread continues moving back and forth among these states until it finally exits and moves to the terminated state.
- 스레드의 상태는 **running, ready, blocked 3개 중에 하나**이다. 이 상태를 왔다 갔다 하면서 계속되다가 종료된다.

✓ When you create a new thread, you must specify an entry-point function. Because threads are relatively expensive to create in terms of memory and time, it is therefore recommended that your entry point function do a significant amount of work or set up a run loop to allow for recurring work to be performed.
- 스레드를 만들 때 `entry-point function` 을 지정해주어야 한다. 그런데 스레드를 만드는 작업은 꾀나 비싼 작업이기 때문에, 시작점에서 많은 양의 일을 하기를 권장한다.

✓ Because threads in a single application share the same memory space, they have access to all of the same data structures. If two threads try to manipulate the same data structure at the same time, one thread might overwrite another’s changes in a way that corrupts the resulting data structure.
- 메모리는 쉐어되기 때문에, 여러 스레드에서 동시에 같은 데이터를 get 하거나 set 하려고 하는 **Racing Condition을 주의**하자.

✓ Multiple threads can be executed at the same time on a single CPU core (or at least perceived as at the same time). The operating system assigns small slices of computing time to each thread.
- 여러개의 스레드가 동시에 하나의 CPU에서 작동 가능 (OS가 각 스레드마다 연산 시간을 쪼개 줌)

## Cost
✓ Threads don’t come for free. Each thread ties up memory and kernel resources.
✓ The minimum allowed **stack size for secondary threads is 16 KB** and the stack size must be a multiple of 4 KB. The space for this memory is set aside in your process space at thread creation time, but the actual pages associated with that memory are not created until they are needed.
- 스레드 생성은 공짜가 아니다. 메모리와 커널 리소스라는 비용이 든다. 

<br>
<br>

# Run Loops

✓ A run loop is a piece of infrastructure used to manage events arriving asynchronously on a thread
- 런러푸는 스레드에 도착하는 이벤트들을 비동기로 처리하기 위한 하부 구조이다.

✓ As events arrive, the system wakes up the thread and dispatches the events to the run loop, which then dispatches them to the handlers you specify. If no events are present and ready to be handled, the run loop puts the thread to sleep.
- 이벤트가 도착하면 스레드를 깨워서 이벤트를 런루프에다가 dispatch 해주고, 최종적으로 핸들러에 dispatch 된다. 아무런 이벤트가 없다면 런루프는 스레드를 재운다.

✓ Because a run loop puts its thread to sleep when there is nothing to do, it eliminates the need for polling, which wastes CPU cycles and prevents the processor itself from sleeping and saving power.
- 런루프는 스레드를 잠재웠다 깨웠다 하기 때문에, CPU 사이클을 소모하는 polling 의 비효율성을 없애준다.

🙉 Polling 방식?
- 친구를 기다린다고 치자. Polling 방식이라면, 친구가 올 때까지 아무것도 하지 않고 기다리는 것. Interrupt 방식 이라면, 친구가 올 떄 까지 영화보고 밥먹고 하다가 전화가 오면 그 떄 마중 나가는 것.

✓ To configure a run loop, all you have to do is launch your thread, get a reference to the run loop object, install your event handlers, and tell the run loop to run. The infrastructure provided by OS X handles the configuration of the main thread’s run loop for you automatically. If you plan to create long-lived secondary threads, however, you must configure the run loop for those threads yourself.
- 런루프를 만들려면, 스레드를 실행 시킨 뒤 런루프 객체에 레퍼런스 해주고 런루프를 동작시키면 된다. 


### RunLoop Mode


✓ A run loop mode is a collection of input sources and timers to be monitored and a collection of run loop observers to be notified. Each time you run your run loop, you specify (either explicitly or implicitly) a particular “mode” in which to run. During that pass of the run loop, only sources associated with that mode are monitored and allowed to deliver their events.
- 모드에 지정해둔 인풋 소스 / 타이머 / 옵저버만 모니터링 하고 이벤트를 딜리버 하겠다는 의미


### 스크롤 될 떄의 런루프 모드 (타이머 관련 이슈)
✓ A typical example of this is scrolling on iOS. While you’re scrolling, the run loop is not running in its default mode, and therefore, it’s not going to react to, for example, a timer you have scheduled before. Once scrolling stops, the run loop returns to the default mode and the events which have been queued up are executed. If you want a timer to fire during scrolling, you need to add it to the run loop in the NSRunLoopCommonModes mode.
- 스크롤 할 때 런루프가 default mode에 있지 않다. 떄문에 타이머에 반응하지 않을 것이다.
- 스크롤이 멈추면, 런루프는 default mode로 돌아오고, 큐잉된 이벤트들을 처리할 것이다.
- 스크롤 될 때 타이머가 fire되게 하고 싶다면, 타이머를 `NSRunLoopCommonModes`의 런루푸에 넣어야 한다.



<br>
<br>

## Queue-based concurrency APIs
- `Grand Central Dispatch`, `NSOperationQueue`
- centrally managing a thread pool that everybody uses collaboratively. // 모두가 협력해서 사용할 수 있는 스레드 풀을 중앙에서 관리해준다.


## GCD

✓ GCD decides on which particular thread your code blocks are going to be executed on, and it manages these threads according to the available system resources.
- 스레드 관리는 시스템 리소스에서 알아서 처리해줄테니,

✓ Think about work items in a queue rather than threads. 
- 스레드 보다는 실행해야 할 작업 단위에 개발자들이 집중할 수 있도록!


<br>


## Operation Queues

✓ The NSOperationQueue class has two different types of queues: the main queue and custom queues. The main queue runs on the main thread, and custom queues are processed in the background. In any case, the tasks which are processed by these queues are represented as subclasses of NSOperation.
- 메인 큐 (메인 스레드에서 돔)와 커스텀 큐 (백그라운드 스레드에서 돔)가 있다. 

✓ `maxConcurrentOperationCount` property. Setting it to one gives you a serial queue, which is great for isolation purposes.
- 최대로 concurrent 하게 돌아갈 수 있는 operation count를 지정할 수도 있다.


<br>
<br>

# 😫 여러가지 좋지 않은 상황들과 솔루션들

### 이유: Sharing of Resources
- Anything you share between multiple threads is a potential point of conflict, and you have to take safety measures to prevent these kind of conflicts.
- 멀티 스레드간에 공유되는 자원은 충돌의 요소이다.

## Race Conditions
## Lock
## language level support (`atomic`)
## Dead Lock
## Explosion