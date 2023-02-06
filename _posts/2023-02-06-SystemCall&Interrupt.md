---
title: System Call & Interrupt 
tags: [OS, System_Call, Interrupt]
style: fill, border
color: secondary
comments: true
description: what's about system call & interrupt
---
# System Call & Interrupt
## System call 
>**정의**\
>시스템 호출(system call)은 운영 체제의 커널이 제공하는 서비스에 대해, 응용 프로그램의 요청에 따라 커널에 접근하기 위한 인터페이스이다. 보통 C나 C++과 같은 고급 언어로 작성된 프로그램들은 직접 시스템 호출을 사용할 수 없기 때문에 고급 API를 통해 시스템 호출에 접근하게 하는 방법이다.

즉 user-level의 다양한 프로그램들은 하드웨어를 완벽히 이용하기 위해 kernel의 도움을 받아야하고 이는  
system call을 통해서 이뤄진다.
![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FJXwNG%2Fbtqw787Kgfe%2FvmrkitiEEjDI8G9w2mFzUk%2Fimg.png)
### 종류
- 프로세스 제어 : end, abort, malloc, etc
- 파일 조작 : open, read, etc
- 장치 관리 :  request devices, attach, etc
- 정보 유지 : time, date, etc
- 통신 : send message, etc

### kernel mode
- 이 모드에서 운영 체제의 커널은 메모리, I/O 장치 및 기타 시스템 구성 요소를 포함하여 시스템의 하드웨어 리소스에 대한 전체 액세스 권한으로 실행된다.
- 커널 모드에서 실행되는 코드는 가장 높은 수준의 권한을 가지며 하드웨어 수정 또는 기타 낮은 수준의 시스템 작업을 포함하여 시스템에서 모든 작업을 수행할 수 있다.
- 커널 모드는 일반적으로 장치 드라이버, 시스템 호출 및 기타 저수준 시스템 작업을 실행하는 데 사용된다.

### user mode
- 이 모드에서 응용 프로그램은 시스템의 하드웨어 리소스에 대한 제한된 권한 및 액세스 권한으로 실행된다.
- 사용자 모드에서 실행되는 코드는 커널 및 기타 시스템 구성 요소와 격리되며 시스템의 안정성을 방해할 수 있는 작업을 수행할 수 없다.
- 사용자 모드에서 실행되는 응용 프로그램은 시스템 호출을 사용하여 운영 체제의 커널에서 서비스를 요청한다.

이렇게 두가지 모드로 나눈 이유는 모드의 전환을 통해 낮은 수준의 시스템에 방해가 되지 않고 안정성과 보안을 유지하는데 도움이 되기때문이다.  

### Interrupt & Trap(Exception)
**Interrupt**  
우리가 흔히 말하는 Interrupt는 하드웨어 인터럽트, 비동기식 인터럽트라고 한다.  
정해진 기준없이 예측불허하게 이벤트가 발생한다. ex ) I/O interrupt, ketboard event, etc

**Trap**  
트랩은 0으로 나누기 오류 또는 종료요청 과 같은 특정 조건에 대한 응답으로 프로그램의 정상적인 실행을 중단하는데 사용되는 메커니즈이다. 흔히 소프트웨어 인터럽트라고 한다.  
또 CPU 내부 인터럽트라고 불리는만큼 동기적으로 작동하며, 어느정도 예상이 가능하다.

**Exception**  
정상적인 제어흐름을 방해하는 실행에 발생하는 이벤트입니다.

> **Trap vs Exception**\
> Trap은 Exception과 비교해서 저수준 메커니즘이다. 즉 더 low level로 구현되어 속도와 성능에 중요성을 둔다. Exception은 high level로 구현되어 구조화가 되고 관리가능한 오류처리메커니즘을 허용합니다.

#### Interrupt Service Routine(ISR)
> **정의**\
> 인터럽트 서비스 루틴(Interrupt Service Routine)는 인터럽트 핸들러 라고도 하며 실제 인터럽트를 처리하는 루틴으로 실행 중이던 레지스터와 PC를 저장하여 실행중이던 CPU의 상태를 보종하고 인터럽트 처리가 끝나면 원래 상태로 복귀합니다. 운영체제 코드 부분에는 각종 인터럽트 별로 처리해야 할 내용이 이미 프로그램이 되어 있습니다.

![이미지](https://mblogthumb-phinf.pstatic.net/20160310_124/scw0531_14575366291105WjS7_PNG/ERTRTETRE.png?type=w2)
위와 같은 과정을 가지고 문제가 해결된다.  

>**Polling vs Interrupt**\
>Interrupt : 하드웨어의 변화를 감지하여 외부로부터의 입력을 CPU가 알아채는 방법이다. 인터럽트는 이벤트를 수행하라는 IRQ를 받으면, 인터럽트 핸들러를 통해 ISR이 동작하게 된다.\
>Polling : 하드웨어의 변화를 지속적으로 읽어 들이며 이벤트의 수행 여부를 주기적으로 검사하여 해당 신호를 받았을때 이벤트를 실행하는 방식이다.\
>즉, polling은 하드웨어에게 응답이 오기를 계속 기다리고 묻는것이라면, interrupt는 직접 하드웨어가 보내주는 것이다.


