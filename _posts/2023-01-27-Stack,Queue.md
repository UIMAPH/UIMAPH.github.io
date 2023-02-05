---
title: Stack & Queue
tags: [Data_Structure, Stack, Queue]
style: fill
color: success
comments: true
description: what's about Stack & Queue
---
# Stack, Queue
## Stack
> Stack은 LIFO(Last In First Out)의 원칙을 따르는 선형 자료구조이다
> [출처](https://www.geeksforgeeks.org/stack-data-structure/)

이때 stack은 두가지 main operation을 가지고 있는데
1. Push - 데이터를 추가한다.
2. Pop - 가장 최근에 추가한 데이터를 삭제한다.
~~~ c++
stack<int> numbers;

numbers.push(1);
numbers.push(2);
numbers.push(3);

while(!numbers.empty()) {
	cout<<numbers.top()<<", ";
	numbers.pop();
}
// 3, 2, 1, 
~~~
### stack의 활용
stack은 추가된 가장 최근의 데이터를 방출하는 데 큰 의미를 가지고 있다.
1. 인터넷 뒤로가기
	단순한 Stack은 아니지만 큰 틀은 stack을 활용한다.
	방문한 가장 page를 넣으면 뒤로가기를 누를 시 page stack에서 pop이 발생하고 top의 페이지를 반환한다.
2. 함수를 부를 때
	함수는 리턴값을 가지고 실행 줄로 돌아와야한다. 그렇기 때문에 메모리에서 최근 Line을 stack에 담고, 함수 연산 후 다시 돌아온다.
3. 재귀 알고리즘
	재귀 알고리즘은 stack을 활용하면 손쉽게 구현할 수 있다.
	실제로 return 에서 자기 함수를 부르는 것도 OS는 stack을 활용한다. (2번)

## Queue
>Queue는 FIFO(First In First Out)의 원칙을 따르는 선형 자료구조이다
>[출처](https://www.geeksforgeeks.org/queue-data-structure/)

Queue는 두가지 main operation을 가진다.
1. Enqueue - 데이터를 추가한다.
2. Dequeue - 데이터의 추가순서를 보장하며 데이터를 삭제한다.

~~~ c++
queue<int> numbers;

numbers.push(1);
numbers.push(2);
numbers.push(3);

while(!numbers.empty()) {
	cout<<numbers.front()<<", ";
	numbers.pop();
}
// 3, 2, 1, 
~~~
### Queue의 사용
1. 프로세스 관리
	process가 들어온 순서대로 process를 실행할 때, 필요하다(자세한 사항은 OS)

## Stack, Queue in iOS, Swift
### Stack
1. Navigation Stack
	Navigation Stack란 위에서 설명한 뒤로가기처럼 ViewController 간에 root를 지정하고 Stack에 푸쉬하면 가장 위에 있는 즉 최근에 보여주려고한 View가 window에 contain됩니다.

### Queue
1. Grand Dispatch Queue
	쓰레드를 관리하는 방법으로써 UI를 작업하는 Main큐와 Global 등으로 나뉘어 우선순위에 맞게 해당 동작을 수행합니다.
	- **Userinteractive** - UI적인 작업 즉 1순위의 작업(꼭 ui는 아니여도 됨)
	- **Userintiated** - 2번째로 중여한 것으로 유저가 기다리고 있는 행위들
	- **Default** - 중요도가 없는 것으로 이러한 것이 2개 있다면 들어온 순서대로
	- **Utility** - 시간이 좀 걸리는 작업
	- **background** - 당장 필요하지 않은 것들
