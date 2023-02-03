---
title: Data structure & Array
tags: [Data_Structure, Array, List, Vector, ArrayList]
style: fill
color: success
comments: true
description: aasdasd
---
# What is data Structure

> **자료구조**는 컴퓨터 과학에서 효율적인 접근 및 수정을 가능케 하는 자료의 조직, 관리, 저장을 의미한다. 더 정확히 말해, 자료 구조는 데이터 값의 모임, 또 데이터 간의 관계, 그리고 데이터에 적용할 수 있는 함수나 명령을 의미한다. 신중히 선택한 자료구조는 보다 효율적인 알고리즘을 사용할 수 있게 한다. 이러한 자료구조의 선택문제는 대개 **추상 자료형**의 선택으로부터 시작하는 경우가 많다. 효과적으로 설계된 자료구조는 실행시간 혹은 메모리 용량과 같은 자원을 최소한으로 사용하면서 연산을 수행하도록 해준다.
> - [출처](https://ko.wikipedia.org/wiki/자료_구조) 

쉽게 말해서 자료들이 모여있는 구조이다.

또 추상자료형이 등장하는데 추상자료형(Abstruct Data Type)

>추상적 자료형은 인터페이스와 구현을 분리하여 추상화 계층을 둔 것이다. 예를 들어, 전기 밥솥을 추상적 자료형에 비유한다면, 그 속에 들어가는 밥은 자료가 되고, 밥솥에 있는 취사, 예약취사 버튼들과 남은 시간을 표시하는 디스플레이에 어떤 내용들이 표시되어야 하는지를 명기한 것이다. 추상적 자료형에서는 이것들이 어떻게 구성되는지 관심이 없고, 몇 와트의 전기를 소모하는지에 대해서도 관심이 없다.
>- [출처](https://ko.wikipedia.org/wiki/추상_자료형)

쉽게 말하면 설명서같은 것이다.

## 자료구조의 종류
- 배열, 리스트
- 해쉬
- 스택, 큐
- 그래프, 트리
- 힙
- 셋, 맵
- etc

## Primitive Data Structure
![img](https://www.simplilearn.com/ice9/free_resources_article_thumb/Data_Structures_in_C_1.png)
> **Primitive Data Structure의 정의**
> C의 기본 데이터 구조는 C 언어로 이미 정의된 기본 데이터 구조입니다. 이러한 데이터 구조는 단일 값만 저장하는 데 사용할 수 있습니다. 그것들은 데이터 조작의 기초입니다. C의 기본 데이터 구조(기본 데이터 유형이라고도 함)에는 int, char, float, double 및 포인터가 포함됩니다.
> [출처](https://www.simplilearn.com/tutorials/c-tutorial/data-structures-in-c)

> **Non-Primitive Data Structures의 정의**
> C의 기본이 아닌 데이터 구조는 원시 데이터 구조에서 파생되었기 때문에 파생 데이터 구조라고도 합니다. 기본이 아닌 데이터 구조를 사용하여 많은 수의 값을 저장할 수 있습니다. 저장된 데이터는 삽입, 삭제, 검색, 정렬 등과 ​​같은 다양한 작업을 사용하여 조작할 수도 있습니다. 배열, 트리, 스택 등과 같은 데이터 구조가 이 범주에 속합니다.
> [출처](https://www.simplilearn.com/tutorials/c-tutorial/data-structures-in-c)

# Array & List & Vector
## Array

> 배열(array)은 연속적인 데이터의 집합이다.
> -[TCP SHCOOL](http://www.tcpschool.com/c/c_array_oneDimensional)-

하지만 뭔가 빠진듯한 이 정의는 c++에 한정지으면 다음과 같은 정의도 가능하다

> 임의접근이 가능하고, 메모리에 연속적으로 올라가있는 자료구조
> 
> > 이렇게 언어를 한정한 이유는 자바스크립에서 배열은 **희소배열**이라고 하는 형태의 메모리에 
> > 비연속으로 올라가고, 임의접근이 아닌 해쉬테이블 형식인데 이 또한 array라는 이름으로 불리고 있어 통용된 정의라고 보기엔 힘들것 같다.

임의접근과 연속적 이 두 개념은 서로 인과관계이다.
배열은 메모리에 특정 타입이 연속되있다고 보장된다. 그럼 10번째 값은 무엇인가?라고한다면
**배열의 시작주소 + 타입의 사이즈 x 9**
이렇게 한다면 바로 그 주소를 찾을 수 있다.
~~~c++
	int numbers[3] = {1,2,3};
	for(int number : numbers) cout<<number<<", ";
	// 1, 2, 3,
~~~

### 장단점
- 임의접근 즉 원하는 데이터를 O(1)에 조회할수 있다. 
- 대치 또한 O(1)이다.
- 삽입/삭제가 O(n)이다.

## List
> **연결 리스트**, **링크드 리스트**(linked list)는 각 노드가 데이터와 포인터를 가지고 한 줄로 연결되어 있는 방식으로 데이터를 저장하는 자료 구조이다. 이름에서 말하듯이 데이터를 담고 있는 노드들이 연결되어 있는데, 노드의 포인터가 다음이나 이전의 노드와의 연결을 담당하게 된다.
> - [출처](https://ko.wikipedia.org/wiki/연결_리스트)

설명을 할 때, 이중 연결 리스트로 설명할 예정이다.

배열과 가장 큰 차이점은 메모리에 순차적으로 올라가지 않아도 된다는 것이다.
즉 비연속적이다.
배열은 이전과 다음을 표시하는 pointer가 없어서 type의 사이즈만큼 넘어가서 다음 데이터를 나타낸다면,
리스트는 다음 노드를 가르키는 pointer가 있기 때문에 연속적일 필요가 없다.
~~~ c++
list<int> numbers;
numbers.push_back(1);
numbers.push_back(2);
numbers.push_back(3);

list<int>::iterator iter = a.begin();

for(auto number : numbers) {
	cout << *iter << ", ";
}

//1, 2, 3,
~~~

### 장단점
- 삽입 / 삭제, 대치 O(1)
- List 끼리의 연산 (List + List ...) O(1)
- 조회 O(n)

## Vector (Arraylist)

vector는 정적으로 배열의 크기를 정하는 것이아닌 동적으로 크기를 정하는 배열이다.
하지만 c++에선 몇가지 멤버변수를 추가해 사용성을 늘리고 있다.

~~~ c++
vector<int> numbers;

numbers.push_back(1);
numbers.push_back(2);
numbers.push_back(3);

for(int number : numbers) {
	cout<<number<<", ";
}
// 1, 2, 3, 
~~~
vetor는 동적으로 크기를 정하기 때문에 특정 갯수를 넘어가면 배열을 동적으로 확장한다.
그렇기 때문에 add last가 항상 O(1)은 아니다.

### 장단점
- 기본적으로 장점은 배열과 같다.
- 동적으로 배열을 확장할 수 있다.

>vector와 arraylist의 차이?
> c++ 에서는 arraylist란 자료구조는 명시적으로 존재하지 않는다.
> 이 두 자료구조는 Java에서 차이를 나타내는데 가장 큰 차이점은 **Tread safe** 하다는 점이다.
> vector는 Thread safe한 반면, arraylist Thread safe하지 않기 때문에 다양한 차이가 발생한다.
> 1. arraylist는 동기적으로 작동하지 않기 때문에, 기본적으로 빠르다.
> 2. arraylist는 명시적으로 동기화 시켜줘야 한다. (안그러면 생기는 문제는 OS 게시글에서..)

## Swift에서 Array, List, Vector

swift에서의 배열
```swift
	var array: [Any] = [1, "String", 2.9]
	print(array)
	// [1, "String", 2.9]
```
모든 배열이 같은 자료형을 가지고 있을 필요는 없다.
swift에서는 한 배열에 다양한 타입을 넣고, 임의접근이 가능하다.
이렇게 되기 위해 NSArray이라는 것이 존재한다.
instance라면 무엇이던 append할 수 있다. 
다른 타입이지만 임의접근이 가능한 이유는 값을 저장하는 것이 아닌 메모리에 올라간 주소를 올리기 때문이다.

또 list와 vector가 따로 존재하지 않는다.
기본적으로 vector처럼 동적할당을 지원한다.
그리고 List는 swift가 제공하지 않는다.
