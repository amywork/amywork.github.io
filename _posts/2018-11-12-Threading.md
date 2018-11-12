---
layout: post
title: "Book Marks"
author: "Amy"
---

<br>

# Threading Programming
- [Apple Document](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/AboutThreads/AboutThreads.html#//apple_ref/doc/uid/10000057i-CH6-SW3)

✓ The term thread is used to refer to a separate path of execution for code.
- 스레드란, 실행하는 코드들의 분리된 길 (1앱 N스레드)

✓ The term process is used to refer to a running executable, which can encompass multiple threads.
- 프로세스란, 멀티 스레드를 아울러서 코드를 실행하는 것. (1앱 1프로세스)

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