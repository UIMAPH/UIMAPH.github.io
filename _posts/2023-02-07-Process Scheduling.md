---
title: Process Scheduling 
tags: [OS, Process_Scheduling]
style: border
color: secondary
comments: true
description: what's about Process Scheduling
---
# Process Scheduling
>**정의**\
>CPU 시간 및 메모리와 같은 사용 가능한 리소스를 관리하고 시스템에서 실행 중인 프로세스에 할당하기 위해 운영 체제에서 사용하는 기술이다. 스케줄러는 다음에 실행할 프로세스를 결정하고 그에 따라 리소스를 할당하여 모든 프로세스가 시스템 리소스를 공정하게 공유하고 적시에 실행할 수 있도록 한다.

**3가지 scheduling 단위**  
- Long-term scheduling
	ready 큐로 어떤 process를 보낼것인가?  
	수행빈도가 적고, 느립니다.  
	**즉, 어떤 프로세스를 커널에 등록할 것인가를 정하는 것입니다.**
- Middle-term scheduling
	어떤 process가 CPU를 할당받을 것인가?  
	**즉, 어떤 프로세스에게 메모리를 할당할 것인가**
- Short-term scheduling
	CPU를 할당받는 시기와, 특정 프로세스를 지정하는 작업  
	자주 수행되고 빠릅니다.  
	**어떤 프로세스에게 CPU를 할당할 것인가?**

## Non-Preemptive Scheduling
> **정의**\
> 어떤 프로세스가 CPU를 할당받으면 그 프로세스가 종료되거나 입출력 요구가 발생하여 자발적으로 중지될 때까지 실행되도록 보장합니다.

**장담점**  
- 순서대로 처리되는 공정성이 있고 다움에 처리해야 할 프로세스와 관계없이 응답 시간을 예상할 수 있으면 선점 방식보다 스케줄러 호출빈도가 낮고 context switching에 의한 overhead가 적다.
- 일괄 처리 시스템에 적합하며, CPU 사용 시간이 긴 하나의 프로세스가 CPU 사용 시간이 짧은 여러 프로세스를 오랫동안 대기시킬 수 있으므로 처리율이 떨어질 수 있다는 단점이 있습니다.

### FCFS (First Come First Serve Scheduling)
![img](https://www.tutorialspoint.com/operating_system/images/fcfs.jpg)

 **CPU를 먼저 요청한 process가 먼저 배정받는 방식이다.**  
 운이 좋다면 상당히 좋은 방법이지만, 평균대기 시간을 계산해본다면 비효율적인 경우가 많다.
 또 **Convoy Effect**가 발생하는데 이는 CPU와 장치들의 사용률을 낮추기 때문에 되도록 지양해야한다.
 > **Convoy Effect**\
 > CPU 사용시간이 긴 프로세스에 의해 사용시간이 짧은 프로세스들이 오래 기다리는 현상으로\
 > "호위 효과 " 라고도 하며, 이로 인해 평균 대기시간이 길어지게 된다.

### SJF(Shortest Job First), SPN(Shortest Process Next)
>**정의**\
>Ready queue에서 CPU 요구량이 적은 것을 먼저 실행시켜 주는 방식

**평균 응답 시간을 줄일 수 있다**는 장점이 있지만, 만약 실행시간이 긴 프로세스가 있다면 CPU할당을 받지 못하고 무한 대기 현상이 발생할 수 있습니다.  -> No Starvation을 어긴다.
> **CPU 스케줄링 알고리즘의 목적**\
> No Starvation : 각각의 프로세스들이 오랜시간동안 CPU를 할당받지 못하는 상황이 없도록 한다.\
> Fairness : 각각의 프로세스에 공평하게 CPU를 할당해준다.\
> Balance : 항상 시스템은 바쁘게 유지한다.

## Preemptive scheduling
>**정의**\
>어떤 프로세스가 CPU를 할당받아 실행 중에 있어도 다른 프로세스가 실행 중인 프로세스를 중지하고 CPU를 강제로 점유할 수 있습니다.

**몇가지 규칙**  
1. 높은 우선순위를 가지는 프로세스는 항상 먼저 스케줄되어야 한다.
2. I/O-bound process는 CPU-bound process 보다 반드시 높은 우선순위에 있어야 한다.
3. Time slice 의 양은 CPU burst time 보다 조금만 더 많아야 한다.
4. Real-time 프로세스는 다른 프로세스에 비해 매우 높은 우선순위를 갖는다.

### SRT(Shortest remaining Time Sheduling)
>**정의**\
>최단 잔여시간을 우선으로 하는 스케줄링\
>진행 중인 프로세스가 있어도, 최단 잔여시간인 프로세스를 위해 sleep시키고 짧은 프로세스를 먼저 할당한다.

![이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F47cG3%2FbtqF63ctwKv%2F99wazY0GKJGkY5jUxCMoRk%2Fimg.png)



**장담점**
- 평균대기시간이 짧다. 하지만 잦은 Context switching이 일어나고 overheadr가 커진다.
- starvation 현상이 심각하게 발생할 수 있다.
- CPU의 잔여시간을 예측하는 것은 불가능에 가깝기 때문에 실제로 사용되기 어렵다.


### Round Robin
>**정의**\
>시분할 시스템을 위해 설계된 선점형 스케줄링의 하나로서, 프로세스들 사이에 우선순위를 두지 않고, 순서대로 시간단위로 CPU를 할당하는 방식의 CPU 스케줄링 알고리즘이다.

![img](https://velog.velcdn.com/images%2Fchappi%2Fpost%2Fffeefa26-2f1d-4ba6-af24-912b8e3d5911%2F3.png)

**장단점**  
- Time Quantum의 적절한 설정이 중요하다
	- 너무 클 경우, FCFS와 다른 점이 없다
	- 너무 작을 경우, context switching이 잦게 발생해 over header가 커진다.
- 모든 프로세스가 최초 응답 시간을 빠르게 보장받을 수 있다는 큰 장점이 있다.  
    자연스럽게 콘보이 효과 역시 줄어든다.

### Priority Scheduling
>**정의**\
>우선순위가 높은 프로세스에 CPU를 우선 할당하는 방식의 스케줄링\
>우선 순위는 시간 제한, 메모리 요구량, 프로세스의 중요성, 자원사용 비용 등에 따라 달라질 수 있다.\
>우선 순위가 같을 경우, FCFS와 다를게 없다.
