---
title: OS & Process, Thread
tags: [OS, Process, Thread]
style: border
color: secondary
comments: true
description: what's about OS & Thread
---
# OS
## 
>**정의**\
>운영 체제(OS)는 컴퓨터의 하드웨어 및 소프트웨어 리소스를 관리하고 제어하는 ​​기본 소프트웨어입니다. 컴퓨터 사용자와 컴퓨터 하드웨어 사이의 중개자 역할을 하며 다른 소프트웨어를 실행하기 위한 플랫폼을 제공합니다.

![img](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter2/2_01_OS_Services.jpg)

os는 가장 primary한 software로써 하드웨어와 user and other system programs의 사이에 위치한다.

눈여겨 볼것은 os가 위와같은 계층의 형태를 띄고 있다.

## 현대 OS의 종류
1. Windows 
2. Linux
3. Unix
4. iOS
5. Android
6. etc

그리고 포스팅하는 블로그는 Linux를 위주로 알아 볼 것이다.

# Process & Thread
![image](https://velog.velcdn.com/images/curiosity806/post/06d141f2-ff5a-4855-a064-b90f218bc0e2/image.png)
### Process
> **정의**\
> 하나 혹은 그 이상의 Thread로 실행되는 컴퓨터 프로그램의 instance이다. Process는 Program code와 그 activity를 포함한다.\
> [출처](https://ko.wikipedia.org/wiki/프로세스)

즉, Task와 같은 의미를 띄고 있다.
#### Process의 구조

![이미지](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter3/3_01_Process_Memory.jpg)

대략 위와같은 구조를 각 Process별로 가지고 있다.
자세한 사항은 [[Process Memory Allocation]]에 포스팅 하겠다.
대략적으로
- stack
    - 매개변수, 지역변수, return 주소 등과 같은 데이터를 저장하는 영역이다.
    - 컴파일러에 의해 Run Time 도중 크기가 결정되며, 함수가 호출&종료 되는 시점에 생성&제거된다.
- heap
    - new, delete, malloc, free 등을 호출하여 데이터를 저장&관리하는 영역
    - Run time에 크기가 결정된다.
- Data Section
    - 사전에 선언된 데이터가 저장되는 영역 (global static variables, etc)
    - Compile time에 크기가 결정된다.
    - 내부에서 DATA & BBS 영역으로 구분된다.
    - initialized data section : 초기화된 전역 변수가 저장되는 영역(DATA)
    - uninitialized data section : 초기화되지 않은 전역 변수가 저장되는 영역(BBS)
- Text Section
    - 컴파일된 기계어가 저장되는 영역
    - Compile time에 크기가 결정된다.

#### Process의 상태
![img](https://webeduclick.com/wp-content/uploads/2019/02/process.png)
- New : process가 생성되는 상태
- Ready : process가 processor에 할당되기를 대기중인 상태
- Runnig : processor에 할당되어, 실행중인 상태
- Wait : process가 특정 event를 기다리는 상태 (ex. I/O) -> 이때 cpu clock을 낮추거나 해서 에너지 소비를 줄일 수 있음
- Terminated : 종료된 상태

### Thread
> **정의**\
> 스레드(thread)란 프로세스(process) 내에서 실제로 작업을 수행하는 주체를 의미합니다.\
> 모든 프로세스에는 한 개 이상의 스레드가 존재하여 작업을 수행합니다.\
> [출처](http://www.tcpschool.com/java/java_thread_concept)

현대의 OS는 Process의 1개 이상의 Thread를 가지고 실제로 Thread 단위로 Scheduling한다.\
![img](https://velog.velcdn.com/images/yun8565/post/b791bf35-aee1-4f10-9fef-49c382578015/image.png)
실제로 process내부에서 왼쪽과같은 형태로 memory를 점유한다.

## Multi Process & Multi Thread
![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbQK2ht%2FbtrgC6LxZ2H%2F9pPh5WgFTLpRZ9bgPKdoq0%2Fimg.png)
> **Multi Process의 정의**\
> 다중 처리는 여러 독립 프로세서가 단일 컴퓨터 시스템에 통합되어 여러 작업을 동시에 병렬 처리할 수 있는 컴퓨터 시스템 아키텍처의 한 유형입니다.

> **Multi Thread의 정의**\
> 멀티스레딩은 단일 프로세스가 여러 실행 스레드를 가질 수 있도록 하는 일종의 컴퓨터 아키텍처입니다. 각 스레드는 병렬로 실행되며 별도의 작업을 처리할 수 있습니다.

**Multi Process**\
Processor(이하 CPU)는 process단위로 일을 처리한다. OS도 그러한 Process를 변화해가며 Task를 수행한다.\
하지만 컴퓨터 기술이 발전하면서 컴퓨터에 병렬 Processor를 탑재하게 됨으로써, 다양한 process를 실행한다. 그러므로 동시에 process를 병렬적으로 실행하는데 다양한 장담점을 품는다.
**장점**
- 독립된 구조로 안전성이 높다. process끼리 공유하는 부분이 독립적이라 서로에게 영향을 미칠 가능성이 적다.
- 이는 하나의 프로세스가 잘못되어도 다른 프로세스에는 영향을 안끼친다는 장점 또한 파생한다.
**단점**
- 프로세스를 변경하는데 overhead가 크다.
    - user-level에서 kernel-level로 가서 다음으로 실행될 process로 올리는 것은 많은 시간이 필요하다
    - 또 이 과정에서 cache memory init와 같은 무거운 작업은 overhead가 발생한다.

**Multi Thread**\
이는 한 Process안에서의 작업이므로 많은 부분을 Thread끼리 공유한다. 이는 그 만큼 바꿀것이 적다는 것을 의미한다.
**장점**
- 시스템 자원 소모 감소 (자원의 효율성 증대)
- 프로세스를 생성하여 자원을 할당하는 시스템 콜이 줄어 자원을 효율적으로 관리할 수 있다.
- 시스템 처리율 향상 (처리 비용 감소)
- 스레드 간 데이터를 주고받는 것이 간단해지고 시스템 자원 소모가 줄어든다.
- 스레드 사이 작업량이 적어 `Context Switching`이 빠르다. (캐시 메모리를 비울 필요가 없음)
- 간단한 통신 방법으로 프로그램 응답시간 단축
- 스레드는 프로세스 내 스택 영역을 제외한 메모리 영역을 공유하기에 통신 비용이 적다.
- 힙 영역을 공유하므로 데이터를 주고받을 수 있다.
**단점**
-   자원을 공유하기에 동기화 문제가 발생할 수 있다. (병목현상, 데드락 등)
-   주의 깊은 설계가 필요하고 디버깅이 어렵다. (불필요 부분까지 동기화하면, 대기시간으로 인해 성능 저하 발생)
-   하나의 스레드에 문제가 생기면 전체 프로세스가 영향을 받는다.
-   단일 프로세스 시스템의 경우 효과를 기대하기 어렵다.
    - Thread를 병렬적으로 돌릴 수 있는 힘은 core가 여러개 있어서 병력실행이 가능하다는 점인데, 단일코어에서 thread를 여러개로 나눠서 context switching을 하는 것은 괜한 시간낭비가 될 수 있다.
### Multi Process VS Multi Thread
-   멀티 스레드는 멀티 프로세스보다 작은 메모리 공간을 차지하고 `Context Switching`이 빠른 장점이 있지만, 동기화 문제와 하나의 스레드 장애로 전체 스레드가 종료될 위험을 갖고 있다.
-   멀티 프로세스는 하나의 프로세스가 죽더라도 다른 프로세스에 영향을 주지 않아 안정성이 높지만, 멀티 스레드보다 많은 메모리 공간과 CPU 시간을 차지하는 단점이 있다.
-   두 방법은 동시에 여러 작업을 수행하는 점에서 동일하지만, 각각의 장단이 있음으로 적용하는 시스템에 따라 적합한 동작 방식을 선택하고 적용해야 한다.

## Context Switching & PCB
### PCB
>**Process Control Block의 정의**\
>특정한 프로세스를 관리할 필요가 있는 정보를 포함하는, 운영체제 커널의 자료구조입니다. PCB는 운영 체제가 프로세스를 표현한 것이라 할 수 있습니다

![img](https://binaryterms.com/wp-content/uploads/2019/08/Process-Control-Block.jpg)
쉽게 말해 해당 프로세스의 정보모음이다.\
이는 OS가 PCB를 보고 해당 process의 우선순위를 정하거나 Ready상태의 Process를 Running하기 위해 메모리를 초기화 시킬 정보를 가지고 있다.\

OS는 빠른 process조회를 위해 process table(Linked list)을 가지고 있는데, 여기에 pid(process id)와 PCB pointer가 있다.
![img](https://media.geeksforgeeks.org/wp-content/uploads/process-control-block.jpg)

### Context Switching
>**정의**\
>여러개의 프로세스가 실행되고 있을 때, 기존에 실행되던 프로세스를 중단하고 다른 프로세스를 실행하는 것 즉, CPU에 실행되는 프로세스를 교체하는 것

![img](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F994590345BB1B4DB2F)


위와 같은 순서로 context switching은 이뤄진다.
1. 스케줄러가 P0 프로세스의 실행을 중단하고 P1 프로세스를 실행할 것을 요청함
2. P0프로세스에서 Stack의 데이터 위치를 가리키고 있는 SP(Stack pointer)의 값과 다음 실행해야하는 코드의 주소값을 가지고 있는  PC(Program Counter)의 값을 PCB에 저장함 (운영체제에서 관리)
3. P0프로세스는 ready 상태로 바뀌고, CPU에서 P1 프로세스를 실행함. 이 과정을 통해 P1 프로세스의 상태가 ready에서 running으로 바뀌는데, 이 작업을 디스패치(dispatch)라고 한다.
4. 반대로 다시 P1프로세스에서 P0프로세스로 컨텍스트 스위칭을 할 경우, P1프로세스의 SP값과 PC값을 PCB에 저장하고(이 때 PCB는 P0프로세스의 위치값을 저장하는 PCB와는 별도로 생성되는 메모리 공간임) P0프로세스의 PCB에서 SP값과 CP값을 찾아 SP와 PC에 덮어씌움.

> **Context switching은 assembler로 되어있다?**
> 초기에 context switcing은 잦은 수행으로 인해 속도 향상을 위해 어셈블리어로 작성되어 있는 경우가 많았다. 하지만 이는 새로운 CPU에 대한 이식성이 떨어져서 C코드로 작성하고 이를 컴파일러를 통해 어셈블리어로 바꾸어 어쩔수 없는 overhead가 있었는데, 현대는 컴파일러의 발전으로 많이 극복되었다고 한다.

## IPC(Inter Process Comunication)
> **정의**\
> 프로세스들끼리 서로 데이터를 주고받는 행위 또는 그에 대한 방법을 뜻한다.

![이미지](https://t1.daumcdn.net/cfile/tistory/99B096385C4C756932)

위해서 설명했던 것 처럼 process는 독립적인데 이는 서로간의 소통을 힘들게 한다는 단점을 지니고 있었다.\
그래서 프로세스간 소통을 하기 위해 다양한 방법이 연구되었다.

1. Message passing
    커널을 통해 두 프로세스가 소통하는 방식\
    이는 indirect와 direct방식이 있는데 위 그림은 message queue를 이용한 indirect방식이다.\
    커널이 관여하고, 메시지 처리에 많은 실행이 소모되기 때문에 느리다.\
    하지만 같은 메모리 동기화등의 과정은 필요하지 않다.
2. Shared Memory
    프로세스간에 공유하는 메모리 영역을 가지고 있는 방식\
    이는 빠르지만, 동기화가 보장되지 않는 만큼 구현에 있어 어렵고, 비동기적 실행으로 인해 생기는 다양한 문제점을 고려해야한다.
3. socket을 이용한 방식, memory map등 다양한 소통방식이 존재한다.



