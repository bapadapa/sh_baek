# Java Collection
## JCF(Java Collection FrameWork)
Java에서 데이터 관련 처리를 위한 인터페이스, 클래스의 집합(자료구조의 집합이라고 봐도 될 것 같다) (JDK 1.2부터 생겼다한다.)
- 정확히 말하면 데이터를 저장하는 클래스들을 표준화한 설계(Framework).
큰 덩어리로 보면 `Iterable`을 상속받은 것과 `Map`을 상속받은 것이 있다.
![img](/img/Java/JavaCollection/JavaCollection.svg)

JCF의 구성도
## Itorable
- 뜻 그대로 반복가능한 인터페이스이다.
- 위 사진에서 보이는 하위 객체가 추상 메소드인 `iterator()` , `hasNext()`, `next()`를 구현하도록 한다.
## Collection
Itorable을 상속받은 인터페이스
- `size()`, `add()` , `remove()` 와 같은 것들이 명세되어있다.
## List
순서가 존재하는 데이터의 집합
### ArrayList
![linkedList](/img/Java/JavaCollection/ArrayList.png)
- 가변길이를 가진 선형리스트

- 배열을 기반으로 사용하고, 만약 기본적으로 설정한 길이를 넘으면 내부적으로 정한 DEFAULT_CAPACITY 길이만큼 늘려준다. (그림은 너무 길어져서 1개만 추가함)
### LinkedList
![linkedList](/img/Java/JavaCollection/linkedList.png)
- 앞,뒤에있는 노드의 위치값을 기억하여 값을 찾음
- 삭제 및 제거시 위치값을 변경시켜줌
- 위치값을 기억함으로 `Random Access`가 어렵다
- 다음 값의 위치값을 아는 것 이지 특정 값의 위치값을 아는 것이 아니기 때문
- `sequential` 한 접근에 유용하다
- 구현부분을 구경해보니 양방향 likedlist로 구현되어 있다.
- 시작위치 , 끝위치도 기억하는 것으로 보인다.

- ArrayList와 비교

	- 장점
		- 삽입 삭제가 용이
		- 포인터로 가르키고 있으니 데이터 위치 이동 불필요
		- 기억장소 재사용 가능
		- 연속적인 기억 장소 할당이 불필요

	- 단점
		- 포인터 또한 메모리기 떄문에, 공간낭비가 생길 수 있다.
		- 데이터의 위치가 산재되어있다.

	- 결론 
		- 삽입 삭제가 빈번히 일어나는 상황이면 LinkedList
		- 조회만 빈번히 일어나는 상황이면 ArrayList를 권장
### Vector
- ArrayList와 동일한 구조
- 차이점
- Vector은 항상 동기화되어있다.
- 즉 1번에 Thread 1개만 접근 가능하다.
- ArrayList에 동기화 코드를 추가해서 사용하면 Vector처럼 동기화가 되기 때문에 Vector를 사용하는 것 보다는 ArrayList를 사용하는 것을 권장한다.
### Stack
- Vector를 상속받아 구현된 LIFO방식의 클래스
- Stack을 사용하는 것 보다는 DeQue를 사용하는 것을 권장한다
## Queue
FIFO(선입선출) 하는 자료구조
### Priority Queue
- Queue는 기본적으로 FIFO구조지만, 각 요소에 Priority(우선순위)를 지정하여 우선순위가 높은 것 먼저 사용하는 방식

- 우선순위 비교를 위해 `Comparator` 정의 혹은 `Comparable`을 상속하여 사용해야한다

- `Null` 불가
### Deque
Double Ended Queue 의 약자로 FIFO,LIFO가 가능한 자료구조 - FIFO,LIFO가 가능하니 Queue, Stack을 이용하듯 사용 가능하다.
### ArrayDeque
- Deque를 사용할떄 사용한다

- ArrayList와 유사한 구조를 가지고 있음.

- 단순히 생각하면 ArrayList에 Queue/Stack이 추가됬다 생각하면 편할듯 하다.
## Set
순서가 존재하지 않고, 데이터 중복이 불가능한 데이터 집합
 
| 인터페이스 | 중복 | 순서 | 정렬 |
| ------------ | ---- | ---- | ---- |
| HashSet| X| X| ---|
| LiknedSet| X| X| O|
| LinkedHasSet | X| O| ---|
### HashSet

- 중복을 허용하지 않은 집합을 생성할 때 사용

- 내부적으로는 HashMap을 이용하여 처리하여 O(1)의 시간복잡도를 갖는다

- 동기화를 지원하지 않는다. 만약 필요하다면 외부에서 동기화처리를 해줘야한다.

- 순서가 없기 때문에 HashSet을 순회하면 자체적인 저장 방식에 따라 순서가 정해진다.

### LinkedHashSet

- HashSet과 동일한 구조
- 차이점 : 삽입 순서를 기준으로 반복되는 HashSet이라고 보면 된다.
- 즉, HashSet을 사용하는데, 순서가 필요하면 사용

## SortedSet
이름 그대로 정렬이 된 Set이다
### TreeSet
- HashSet과 동일하지만, B-Tree 구조로 저장이된다.

- 맞는지 모르겠지만, B-Tree구조니 O(Log(n))의 시간복잡도를 갖게 될 것 같다.
## Map
- Key - Value로 묶여있는 집합

- Key : 중복 X

- Value : 중복 O

| 인터페이스 | 중복 | 순서 | 정렬 |
| ------------ | ---- | ---- | ---- |
| HashMap/HashTable | X | X | --- |
| TreeMap | X | X | O |
| LinkedHashMap | X | O | --- |

### HashMap
- 기본적으로 Map을 사용할때 사용하는 Map
### HashTable
- HashMap의 레거시

- 즉 HashTable을 사용하지말고 HashMap을 사용하는 것을 권장
### LinkedHashMap
- 순서가 존재하는 Map
## SortedMap
### TreeMap
- B-Tree구조로 정렬되어 있는 Map

- 정렬 기준 : Key값