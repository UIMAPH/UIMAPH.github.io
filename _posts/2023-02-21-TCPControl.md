---
title: TCP Control
tags: [Network, TCP]
style: border
color: warning
comments: true
description: what's about TCP Control
---
# Flow Control(흐름 제어)
> -   송신측과 수신측의 데이터 처리 속도 차이를 해결하기 위한 기법
 >-   Flow Control은 receiver가 packet을 지나치게 많이 받지 않도록 조절하는 것
> -   기본 개념은 receiver가 sender에게 현재 자신의 상태를 feedback 한다는 점

## 패킷 전송 과정
1.  **Application** Layer : 송신측 Application Layer 가 소켓에 데이터를 입력
2.  **Transport** Layer : 데이터를 세그먼트로 감싸고 Network Layer 에 전달
3.  **수신측 노드**로 세그먼트가 **전송**됨. 동시에 **송신측의 Send Buffer 와 수신측의 Receive Buffer 각각에 데이터가 저장**됨.
4.  **수신측 Application Layer** 에서 준비가 되면, R**eceive Buffer 에 있는 데이터를 읽기** 시작함
5.  **따라서, Receive Buffer 가 넘쳐나지 않도록 하는 것이 흐름 제어의 핵심!**
    → 이를 위해 RWND(Receive Window, Receive Buffer 의 남은 공간) 을 송신측에 계속하여 피드백함

## 해결법
### Stop-And-Wait
> **매번 전송한 패킷에 대한 확인 응답을 받아야만 그 다음 패킷을 전송**하는 기법

![img](https://velog.velcdn.com/images%2Fhaero_kim%2Fpost%2Fd0d48e5e-55df-4936-8328-796fa8b53f56%2F263B7D4E5715ECEB32.png)

### Sliding Window
> 수신측에서 설정한 **윈도우 크기만큼 송신측에서 패킷 각각에 대한 확인 응답없이 세그먼트를 전송**하게 하고, 데이터 **흐름을 동적으로 조절**하는 기법

> 전송은 되었으나, **ACK 을 받지 못한 Byte 크기를 파악하기 위해 사용** \
> LastByteSent - LastByteAcked ≤ ReceiveWindowAvertised \
> → 마지막에 보낸 바이트 수 - 마지막에 확인된 바이트 수 ≤ 버퍼 남은 공간  \
> (현재 이도저도 못하고 있는 패킷 수 ≤ 슬라이딩 윈도우 크기)

TCP/IP 를 사용하는 모든 호스트들은 **송신 그리고 수신을 위한 2개의 Window** 를 가지고 있다. 호스트들은 실제 데이터를 보내기 전에 3-Way Handshake 를 통해 연결 설정을 해줄 때 **수신 호스트의 Receive Window 크기에 자신의 Send Window 크기를 맞춰 설정**한다.

# Congestion Control(혼잡제어)
> **송신측의 데이터 전달과 네트워크의 처리속도  차이를 해결하기 위한 기법**이다.

**특징**
-   네트워크 혼잡을 피하기 위해 송신 측에서 보내는 데이터의 전송 속도를 제어하는 것
-   송신 측 데이터 전달과 네트워크 데이터 처리 속도 차이를 해결하기 위한 기법
-   한 라우터에 데이터가 몰려서 모든 데이터를 처리할 수 없는 상황에 이르렀을 때, 호스트들은 계속해서 재전송을 하고 결과적으로 혼잡만 가중시켜 오버플로우나 데이터 손실이 발생한다.

## 해결법
### AIMD(Addictive Increase Multiplicative Decrease)
![이미지](https://velog.velcdn.com/images%2Fchullll%2Fpost%2F7f6f3777-a305-411b-be15-c3caf9a782be%2Fimage.png)

**특징**
-   합 증가/ 곱 감소 알고리즘 이라고도 한다.
-   처음 패킷 하나를 보내 패킷이 문제없이 도착하면 CWND(Congestion Window, 혼잡 윈도우)를 1씩 증가시키면서 전송하는 방법
-   패킷 전송이 실패하거나 타임아웃이 발생하면 CWND를 1/2로 감소시킨다.
-   호스트가 하나의 네트워크를 공유하고 있으면 나중에 진입하는 쪽이 불리하지만 시간이 지날수록 평형 상태로 수렴한다
-   단점은, 초기 넓은 대역폭을 활용하지 못하고 미리 혼잡 상태를 감지하지 못하고 혼잡해지고 나서야 대역폭을 줄이는 방식이라는 점이다.

### Slow Start
#### 1. 기본 개념

![이미지](https://velog.velcdn.com/images%2Fchullll%2Fpost%2Fb1c1e748-29cf-4970-bcac-0422df0fd0c5%2Fimage.png)
-   전송 성공 시 ACK 패킷마다 cwnd를 1씩 증가시킨다. 즉, 한 주기가 끝나고 나면 cwnd는 2배배가 된다.
-   혼잡 현상이 발생하면 cwnd 를 1로 떨어뜨린다.
-   한번 혼잡 현상이 발생하고 나면 혼잡 현상이 발생했던 cwnd 의 절반까지는 이전처럼 증가시키다가 그 이후부터는 완만하게 1씩 증가시킨다.
-   즉, 임계 값(ssthresh, slow start threshold)을 절반으로 줄인다.
-   매 전송마다 2배씩 증가하기 때문에 데이터의 크기가 지수함수적으로 증가한다.
-   파악한 임계 값에 도달하기 전까지 2배씩 증가, 임계값 도달 시 혼잡 회피 단계로 넘어간다.

#### 2. 혼잡 회피

-   윈도우 크기가 임계 값에 도달한 이후에는 데이터의 손실이 발생할 크기가 올라감
-   이를 회피하기 위해 cwnd 크기를 전송 시마다 1 씩 증가시킨다.
-   수신 측으로부터 일정 시간동안 ACK 를 받지 못하면, 타임 아웃이 발생해 cwnd 크기를 1로 줄인다.
-   동시에 임계 값을 패킷 손실이 발생했을 때의 cwnd 크기의 1/2로 줄인다.

#### 3. 빠른 회복

-   혼잡한 상태가 되면 cwnd 를 1이 아니라 절반으로 줄이고 선형 증가 시키는 방법
-   빠른 회복 정책이 적용되면 이후엔 순수한 AIMD 방식으로 동작

#### 4. 빠른 재전송

![이미지](https://velog.velcdn.com/images%2Fchullll%2Fpost%2F0525c9e9-9686-4588-8c83-136e8624549e%2Fimage.png)

-   패킷을 받는 수신자 입장에서 세그먼트로 분할된 내용이 순서대로 도착하지 않는 경우가 있다.
-   수신 측에서 패 킷을 받을 때 먼저 올 패킷보다 다음 패킷이 먼저 도착해도 ACK 를 보냄
-   단, 이 때 순서대로 잘 도착한 패킷의 마지막 순번을 ACK 에 실어서 보냄
-   따라서 중간에 패킷이 손실되면 송신 측에서는 순번이 중복된 ACK 패킷을 받게 되는 것이다. 이것을 감지하면 문제가 되는 순번의 패킷을 재전송할 수 있다.
-   빠른 재전송은 중복된 순번의 패킷을 3개 받으면 재전송한다.
-   재전송하는 경우 혼잡 상태로 간주하고 혼잡 회피를 한다.

# 오류제어
## 오류인지
1.  **수신측에서 송신측으로 명시적으로 NAK 를 전송**
2.  **전송한 패킷에 대한 ACK 가 오지 않거나 중복된 ACK 가 전송될 때!**

## 해결법
### Stop And Wait
> 하나보내고 ACK을 기다리면, 안오면 다시 주면 되니까 기본적으로 오류를 제어할 수 있다.

### Go Back N
![이미지](https://velog.velcdn.com/images%2Fnnnyeong%2Fpost%2F2e5a8940-7d93-48bc-beed-0a5918fccc91%2Fimage.png)
이렇게 중간 패킷이 사라지면 사라진 부분 부터 다시 보낸다.
당연히 이렇게 되면 생각할 것도 없지만, 보냈던것다시 보내야하기 때문에 손해이다.

### Selective Repeat
![이미지](https://velog.velcdn.com/images%2Fnnnyeong%2Fpost%2F2def564a-111b-477e-a3ae-d182fdc0f477%2Fimage.png)
오류가 발생한 패킷만 다시 보내고, 수신측 버퍼에 정렬되지 않게 들어온다.
정렬이 필요하고 다른 버퍼가 또 필요하다는 단점이 있다.

# 흐름제어 vs 혼잡제어

### 흐름 제어
> 흐름 제어는 수신자가 처리할 수 있는 것보다 더 많은 데이터로 인해 부담을 느끼지 않도록 소스 장치에서 대상 장치로 데이터가 전송되는 속도를 관리하는 데 사용되는 메커니즘입니다. 일반적으로 프로토콜 스택의 전송 계층에서 구현되며 데이터가 소모될 수 있는 것보다 더 빨리 전송되어 버퍼 오버플로 및 데이터 손실을 방지하는 데 사용됩니다. 데이터 전송 속도를 동적으로 조정하기 위해 수신기의 피드백을 사용하는 슬라이딩 윈도우 프로토콜과 같은 메커니즘을 사용하여 흐름 제어를 구현할 수 있습니다.

### 혼잡 제어
> 혼잡 제어는 데이터가 네트워크를 통해 전송되는 속도를 조절하여 네트워크 혼잡을 방지하는 데 사용되는 메커니즘입니다. 일반적으로 프로토콜 스택의 네트워크 계층에서 구현되며 네트워크가 트래픽으로 과부하될 때 발생할 수 있는 패킷 손실을 방지하는 데 사용됩니다. 혼잡 제어 알고리즘은 네트워크에서 혼잡의 징후를 모니터링하고 그에 따라 전송 속도를 조정하여 혼잡을 유발하지 않고 최적의 네트워크 활용을 달성하는 것을 목표로 합니다

즉,  
흐름 제어와 혼잡 제어는 네트워크에서 데이터 흐름을 조절하는 데 사용되는 두 가지 메커니즘입니다. 흐름 제어는 데이터 손실을 방지하기 위해 소스 장치와 대상 장치 간의 데이터 전송 속도를 조절하는 반면 혼잡 제어는 네트워크 정체 및 패킷 손실을 방지하기 위해 네트워크에서 데이터 전송 속도를 조절합니다.

# + Syn Flooding
> 네트워크의 TCP(전송 제어 프로토콜) 프로토콜을 대상으로 하는 서비스 거부(DoS) 공격 유형입니다. 이 공격은 적법한 요청에 대한 서버의 응답 능력을 압도하기 위해 대상 서버나 네트워크에 대량의 SYN 패킷을 전송함으로써 시작됩니다.\
> \
> SYN 플러딩 공격은 필터링, 속도 제한 및 TCP SYN 쿠키와 같은 다양한 기술을 사용하여 완화할 수 있습니다. 방화벽과 같은 필터링 기술은 공격 트래픽을 차단할 수 있으며 속도 제한을 사용하여 단일 소스 IP 주소의 연결 요청 수를 제한할 수 있습니다. TCP SYN 쿠키는 클라이언트가 유효한 ACK 패킷을 보낼 때까지 메모리를 사용하지 않고 연결 요청을 처리하기 위해 일부 운영 체제에서 사용하는 기술입니다.

# + Silly Window Syndrome
> 송신 응용 프로그램이 TCP 수신 창 크기보다 작은 비효율적인 작은 세그먼트로 데이터를 보내는 상황을 설명하기 위해 만들어졌습니다. 이로 인해 불필요한 지연이 발생하고 네트워크 성능이 저하될 수 있습니다.
