# Chapter 11 - 컬렉션 프레임웍

## 1. 컬렉션 프레임웍(Collections Framework)

- 컬렉션 프레임웍 : `데이터 군을 저장하는 클래스들을 표준화한 설계`
- JDK 1.2 이전까지는 Vector, Hashtable, Properties와 같은 컬렉션 클래스 존재
  - 각자 다른 방식으로 처리
- JDK 1.2부터 컬렉션 프레임웍이 등장
  - 모든 클래스들을 표준화된 방식으로 다룰 수 있도록 체계화
  - 인터페이스 & 다형성을 이용한 객체지향적 설계

### 1.1 컬렉션 프레임웍의 핵심 인터페이스

- 3가지 타입으로 구분
  - List, Set, Map
  - List와 Set의 공통된 부분을 다시 뽑아서 인터페이스인 Collection을 추가로 정의
  - List -> Collection <- Set, Map
- List
  - 순서가 있는 데이터의 집합 & 데이터의 중복 허용
  - 예) 대기자 명단
  - 구현 클래스 : ArrayList, LinkedList, Stack, Vector 등
- Set
  - 순서를 유지하지 않는 데이터의 집합 & 데이터의 중복 허용 X
  - 예) 양의 정수집합, 소수의 집합
  - 구현 클래스 : HashSet, TreeSet 등
- Map
  - 키와 값의 쌍으로 이루어진 집합, 순서는 유지 X, 키는 중복 X, 값은 중복 허용
  - 예) 우편번호, 지역번호
  - 구현 클래스 : HashMap, TreeMap, Hashtable, Properties 등
- Vector나 Hashtable과 같은 기존의 컬렉션 클래스들은 프레임웍 명명법을 따르지 않으므로 사용하지 않는 것이 좋음

Collection 인터페이스

- List와 Set의 조상
- 저장된 데이터를 읽고, 추가하고 삭제하는 등 컬렉션을 다루는데 가장 기본적인 메서드들을 정의
- 스트림, 제네릭스 등도 사용됨

List 인터페이스

- `중복을 허용`하면서 `저장순서가 유지`되는 컬렉션을 구현하는데 사용
- Vector, ArrayList, LinkedList 등이 자손 클래스

Set 인터페이스

- `중복을 허용하지 않고 저장순서가 유지되지 않는` 컬렉션 클래스를 구현하는데 사용
- Set을 구현한 클래스로는 HashSet, TreeSet 등이 있음
- HashSet -> Set <- SortedSet <- TreeSet

Map 인터페이스

- `키와 값을 하나의 쌍으로 묶어서 저장하는` 컬렉션 클래스를 구현하는데 사용
- 키는 중복 X
- 값은 중복을 허용
  - 중복된 키와 값을 저장할 시 기존의 값은 없어지고 마지막에 저장된 값만 남김
- 구현 클래스로는 Hashtable, HashMap, LinkedHashMap, SortedMap, TreeMap 등이 있음
- value()(저장된 모든 값 반환) 에서는 반환 타입이 Collection, keySet()(저장된 모든 키 반환)에서는 반환타입이 Set인 것에 주의
  - 값은 중복을 허용하고 키는 중복을 허용하지 않는 속성 때문

Map.Entry 인터페이스

- Map 인터페이스의 내부 인터페이스
- Map에 저장되는 key-value 쌍을 다루기 위해 내부적으로 정의 된 인터페이스
  - equals(), getKey(), getValue(), hashCode(), setValue() 등의 메서드가 존재

### 1.2 ArrayList

- List 인터페이스를 구현
  - 저장순서 유지, 중복 허용
- 기존의 Vector를 개선한 것
  - 구현원리 & 기능적인 측면은 동일
  - 소스의 호환성을 위해 만들어짐
- Object 데이터를 순차적으로 저장
  - 선언된 배열의 타입이 모든 객체의 최고 조상인 Object
- Collections 클래스의 sort 메서드를 이용해서 저장된 객체들을 정렬 가능
- 요소들을 찾아서 삭제 시 거꾸로 순회하면서 삭제할 것
  - 빈 공간이 생기면 채우기 위해 나머지 요소들이 자리이동을 하므로
- 실제 저장할 개수보다 약간 여유 있는 크기로 선언할 것
  - 자동적으로 저장 크기를 늘려주는 과정이 처리시간이 많이 소요되므로
- vector의 용량과 크기
  - v.trimToSize()는 size와 capacity를 같게 만듬
  - v.ensureCapacity(n)에서 n이 기존의 capacity보다 크기가 크다면 새로운 인스턴스를 생성 후 할당
  - v.setSize(n)에서 capacity가 충분하지 않다면 새로운 인스턴스를 생성 후 할당 (자동적으로 기존의 크기보다 2배로 capacity를 증가시킨 객체 생성)
  - v.clear()는 모든 요소를 제거(capacity는 유지, 모든 공간은 다 null)
- ArrayList, Vector와 같은 자료구조는 데이터를 읽어오고 저장하는 데는 효율이 좋지만 용량을 변경하는 과정의 효율이 떨어짐
- Object remove(int index)
  - 지정된 위치에 있는 객체를 삭제하고 삭제한 객체를 반환
  - 삭제한 객체의 바로 아래 있는 데이터를 한 칸씩 위로 복사후 삭제할 객체를 덮어쓰는 방식 (System.arraycopy를 호출)
- 인덱스가 n인 데이터의 주소 = 배열의 주소 + n \* 데이터 타입의 크기
- ArrayList는 LinkedList에 비해 읽기(접근시간)는 빠르고 추가/삭제는 느림 (순차적인 추가삭제는 더 빠름), 비효율적인 메모리 사용

### 1.3 LinkedList

- 배열은 다음과 같은 장점을 보안하기 위해 링크드 리스트 고안
  1. 크기를 변경할 수 없으므로 데이터를 복사해야함
  2. 비순차적인 데이터의 추가 또는 삭제에 많은 시간이 걸림
- 각 요소들은 자신과 연결된 다음 요소에 대한 참조와 데이터로 구성
  - 데이터 이동을 위한 복사 과정이 없으므로 처리속도가 빠름
  - 데이터 삭제는 이전 데이터가 삭제하고자 하는 요소의 다음 요소를 참조하도록
  - 데이터 추가는 추가하고자 하는 위치의 이전 요소의 참조를 새로운 요소에, 새로운 요소가 그 다음 요소를 참조하도록
- 이전 요소 또한 접근할 수 있도록 구현된것이 더블 링크드 리스트(이중 연결리스트)
  - 다음 요소의 주소, 이전 요소의 주소, 데이터로 구성
- 더블 링크드 리스트의 접근성을 향상시킨 것이 더블 씨큘러 링크드 리스트(이중 원형 연결리스트)
  - 첫 번째 요소와 마지막 요소를 서로 연결시킨 것
- 자바의 LinkedList는 더블 링크드 리스트로 구현되어 있음
  - 낮은 접근성을 높이기 위해
- 순차적으로 추가/삭제하는 경우엔 ArrayList가 LinkedList보다 빠름
  - ArrayList의 각 요소들의 데이터 재배치가 필요하지 않기 때문
- 중간 데이터를 추가/삭제하는 경우에는 LinkedList가 ArrayList보다 빠름
  - ArrayList는 각 요소들을 재배치 후 추가할 공간을 할당해야하지만 LinkedList는 각 요소의 연결만 변경해주면 가능
- LinkedList는 ArrayLIst에 비해 읽기(접근시간)는 느리고 추가/삭제는 느리며 데이터가 많을 수록 접근성이 떨어지는 단점이 있다.

### 1.4 Stack과 Queue

- 스택은 마지막에 저장한 데이터를 가장 먼저 꺼내게 되는 LIFO(Last In First Out) 구조
  - 순차적으로 데이터를 추가하고 삭제하기 때문에 ArrayList와 같은 배열기반의 컬랙션이 적합
  - 자바에서 Stack 클래스 구현하여 제공
- 큐는 처음에 저장한 데이터를 가장 먼저 꺼내게 되는 FIFO(First In First Out) 구조
  - 데이터를 꺼낼 때 항상 첫 번째 저장된 데이터를 삭제하므로 LinkedList로 구현이 적합(ArrayList는 첫 번째 원소 삭제시 재배치를 해야하므로)
  - 자바에선 Queue 인터페이스만 존재, 구현한 클래스를 선택해서 사용

스택과 큐의 활용

- 스텍의 활용 예
  - 수식 계산, 수식 괄호 검사, 워드프로세서의 undo/redo, 웹브라우저의 뒤로/앞으로
- 큐의 활용 예
  - 최근 사용 문서, 인쇄작업 대기목록, 버퍼

PriorityQueue

- Queue의 인터페이스 구현체 중 하나
- 저장한 순서에 관계없이 우선순위가 높은 것부터 꺼낸다.
- null을 저장 할 수 없다. (NullPointException 발생)
- 저장공간으로 배열을 사용하고 각 요소를 heap이라는 자료구조 형태로 저장
  - 힙은 이진트리의 종류로써 가장 큰 값이나 가장 작은 값을 빠르게 찾아줌
- 숫자뿐 아니라 객체도 저장 가능, 각 크기를 비교할 수 있는 방법을 제공해야함
  - 숫자들의 경우 오토박싱되며 Number의 자손들은 자체적으로 정의되어있음

Deque(Double-Ended Queue)

- Queue의 변형, 양쪽 끝에 추가/삭제가 가능
- Dequq의 조상은 Queue
- 구현체는 ArrayDeque, LinkedList 등이 있음
- 스택과 큐를 합쳐놓은 것과 같으므로 스택으로도, 큐로도 사용 가능

### 1.5 Iterator, ListIterator, Enumeration

- Iterator, ListIterator, Enumeration 모두 컬렉션에 저장된 요소를 접근하는데 사용되는 인터페이스
  - Enumeration은 Iterator의 구버전
  - ListIterator는 Iterator의 기능을 향상 시킨 것

Iterator

- 컬렉션 프레임웍의 저장된 요소들을 읽어오는 방법의 표준화
- Iterator는 정의된 인터페이스, Collection에선 Iterator를 반환하는 iterator() 정의
  - 코드의 일관성을 높혀줌
- 메서드
  - boolean hasNext() : 읽어 올 요소가 남아있는지 여부를 return
  - Object next() : 다음 요소를 읽어옴, hasNext()를 먼저 호출해야 안전
  - void remove() : next()로 읽어 온 요소를 삭제, enxt()를 호출 후 사용해야함
- Map을 구현한 클래스는 키와 값을 쌍으로 저장하기 때문에 keySet()이나 entrySet()과 같은 메서드로 Set을 얻어오고 iterator()를 호출해야함

ListIterator와 Enumeration

- Enumeration : Iterator의 구버전
  - boolean hasMoreElements(), Object nextElement()
- ListIterator : Iterator에 양방향 조회 기능 추가 (List를 구현한 경우에만 사용 가능)
- Object PreviousIndex(), Object Previous(), boolean hasPrevious() 등등 추가

### 1.6 Arrays

- 배열을 다루는데 유용한 메서드가 정의

배열의 복사 - copyOf(), copyOfRange()

- copyOf()는 배열 전체를 복사해서 새로운 배열을 만든 뒤 반환
- copyOfRange()는 배열의 일부를 복사해서 새로운 배열을 만들어 반환
  - 범위의 끝은 포함되지 않음

배열 채우기 - fill(), setAll()

- fill()은 배열의 모든 요소를 지정된 값으로 채움
- setAll()은 함수형 인터페이스를 매개변수로 받아서 배열을 채움(구현 객체 or 람다식 지정)

배열의 정렬과 검색 - sort(), binarySearch()

- sort()는 배열을 정렬할 때 사용
- binarySearch()는 배열에 저장된 요소를 검색할 때 사용
  - 지정된 값이 저장된 위치를 찾아서 반환
  - 배열이 정렬된 상태어이야 함
  - 검색한 값과 일치하는 요소들이 여러 개 있다면 어떤 것이 반환될지 알 수 없음

배열의 비교와 출력 - equals(), toString()

- toString()은 배열의 모든 요소를 문자열로 편하게 출력
  - 일차원 배열에만 사용 가능
  - 다차원 배열은 deepToString() 사용(모든 요소를 재귀적으로 접근해서 문자열을 구성)
- equals()는 두 배열에 저장된 모든 요소를 비교 후 결과 return
  - 일차원 배열에만 사용 가능
  - 다차원 배열은 deepEqauls() 사용(모든 요소를 재귀적으로 접근해서 비교)

배열을 List로 변환 - asList(Object... a)

- asList는 배열을 List에 담아서 반환
  - 매개변수의 타입이 가변인수 -> 배열 생성 없이 저장할 요소들만 나열도 가능
- asList()가 반환한 List의 크기를 변경할 수 없음
  - 추가 또는 삭제가 불가능
  - 저장된 내용의 변경은 가능
  - 크기를 변경할 수 있는 List는 `new ArrayList(Arrays.asList(1,2,3,4,5))`와 같은 형식으로 호출

parallelXXX(), spliterator(), stream()

- parallelXXX() : 보다 빠른 결과를 얻기 위해 여러 쓰레드가 작업을 나누어 처리
- spliterator() : 여러 쓰레드가 처리할 수 있게 하나의 작업을 여러 작업으로 나누는 Spliterator 반환
- stream() : 컬렉션을 스트림으로 변환

### 1.7 Comparator와 Comparable

- 컬렉션을 정렬하는데 필요한 메서드를 정의하는 인터페이스
  - Comparable : 기본 정렬기준을 구현하는데 사용
  - Comparator : 기본 정렬기준 외에 다른 기준으로 정렬하고자할 때 사용
- 비교하는 값이 크면 -1, 같으면 0, 작으면 1을 반환하도록 구현
- Arrays.sort()
  - Comparator를 지정해주지 않으면 저장하는 객체에 구현된 내용에 따라 정렬
  - static void sort(Object[] a) : 객체 배열에 저장한 객체가 구현한 Comparable에 의한 정렬
  - static void sort(Object[] a, Comparator c) : 지정한 Comparator에 의한 정렬
- String에선 대소문자를 구분하지 않고 비교하는 Comparator를 상수의 형태로 제공
  - public static final Comparator CASE_INSENSITIVE_ORDER
- 구현된 compareTo() 결과에 -1을 곱하는 식으로 반대로 정렬하는 것도 가능
- compare()의 매개변수가 Object임으로 형변환 후 compareTo()를 호출할 것
