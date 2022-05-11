# Chapter 14 - 람다와 스트림

## 1. 람다식

- JDK 1.8부터 추가
- 함수형 언어의 기능까지 갖추게 함
  - 함수형 언어의 장점들을 자바에서도 사용

### 1.1 람다식이란?

- `메서드를 하나의 식(expression)으로 표현한 것`
  - 간략 & 명확한 식으로 함수를 표현
  - 메서드의 이름과 반환값이 없어지므로 `익명함수`라고도 부름
  - 메서드는 객체지향에서의 함수 (객체의 행위나 동작을 의미)

### 1.2 람다식 작성하기

- 이름과 반환 타입을 제거하고 매개변수 선언부와 몸통 사이에 `->` 추가
  - 반환값이 있다면 return문을 식으로 대신할 수 있음(끝에 세미콜론을 붙이지 않음)
  - 매개변수 타입은 추론이 가능한 경우 생략 (매개변수 중 어느 하나의 매개변수만 생략은 불가능)
  - 매개변수가 하나뿐이고 매개 변수의 타입이 없을 시 경우엔 괄호 생략 가능
  - 괄호 {}안의 문장이 하나일 때는 괄호 생략 가능, 이때도 끝에 세미콜론을 붙이지 않아야 한다.

### 1.3 함수형 인터페이스(Functional Interface)

- 람다식은 익명 클래스의 객체와 동등 (new Object() {~})
- 람다로 정의된 메서드를 호출할 때 필요한 것들
  - 참조 변수가 필요
  - 참조 변수를 만들기 위해서는 참조 변수의 타입이 필요
  - 람다식과 동등한 메서드를 정의한 인터페이스를 만든 뒤 참조 변수의 타입으로 지정
  - 인터페이스를 구현한 클래스를 `'new 타입'`과 같은 방식으로 구현할 수 있지만 람다식으로도 구현 가능
  - (참조 변수).(인터페이스에 정의된 메서드명)으로 익명 객체의 메서드 호출
- 위와 같은 과정으로 인해 인터페이스로 람다식을 다루기로 결정
- 람다식을 다루기 위한 인터페이스를 함수형 인터페이스라고 정의
- 함수형 인터페이스는 하나의 추상 메서드만 정의해야 됨
  - 람다식과 인터페이스의 메서드를 1:1로 연결해야 하기 때문
  - static 메서드와 default 메서드는 제약 X
- @FunctionalInteface 애너테이션을 붙이면 컴파일러가 정확한 함수형 인터페이스인지 확인해 줌

함수형 인터페이스 타입의 매개변수와 반환 타입

- 메서드의 매개변수가 함수형 인터페이스 타입일 시 해당 인자로 람다식을 참조하는 참조 변수를 매개변수로 지정해야 함
- 참조 변수 없이 직접 람다식을 매개변수로 지정하는 것도 가능
- 변수처럼 메서드를 주고받는 것이 가능해짐
  - 사실상 메서드가 아니라 객체를 주고받는 것이지만 코드가 더 간결해짐

람다식의 타입과 형변환

- 람다식의 타입이 함수형 인터페이스의 타입과 정확히 일치하는 것은 아님
  - 람다식의 타입 출력 시 `외부클래스이름&&Lambda&&번호`
  - 함수형 인터페이스와 일치하기 위한 형 변환이 필요하지만 형변환은 생략 가능
- Object로의 형변환은 불가능
  - 람다식은 오직 함수형 인터페이스로만 형변환 가능

외부 변수를 참조하는 람다식

- 익명 클래스와 동일한 방식으로 람다식도 외부에 선언된 변수에 접근 가능
- 람다식 내에서 참조하는 지역변수는 final이 붙지 않았어도 상수로 간주
- 외부 지역변수와 같은 이름의 람다식 매개변수는 허용 X

### 1.4 java.util.function 패키지

- 대부분의 메서드는 타입이 비슷하고 지네릭 메서드로 정의할 수 있음
  - 가능하면 미리 정의해 둔 인터페이스를 활용
  - 메서드 이름 통일, 재사용성, 유지 보수 측면에서 좋음
- 가장 기본적인 함수형 인터페이스
  - run() : 매개변수 X, 반환값 X
  - Supplier\<T> : 매개변수 X, 반환값 O
  - Consumer\<T> : 매개변수 O, 반환값 X
  - Function\<T,R> : 하나의 매개변수를 받아서 결과를 반환하는 일반적인 함수
  - Predicate\<T> : 매개변수 하나, boolean 타입 반환, 조건식을 표현하는 데 사용

조건식의 표현에 사용되는 Predicate

- Predicate는 Function에서 반환 타입이 boolean이라는 차이
- 조건식을 람다식으로 표현하는 데 사용

매개변수가 두 개인 함수형 인터페이스

- 이름 앞에 접두사 `'Bi'`가 붙음
- 종류
  - BiConsumer\<T,U> : 두 개의 매개변수, 반환값 X
  - BiPredicate\<T,U> : 조건식을 표현하는 데 사용, 매개변수는 둘, 반환값은 boolean
  - BiFunction\<T,U,R> : 두 개의 매개변수를 받아서 하나의 결과 반환
- 3개 이상의 매개변수를 갖는 함수형 인터페이스는 직접 구현해서 사용 가능

UnaryOperator와 BinaryOperator

- 매개변수의 타입과 반환 타입의 타입이 모두 일치하는 점만 제외하고는 Function과 같음
- 종류
  - UnaryOperator\<T> : Function의 자손, 매개변수와 결괏값의 타입이 동일
  - BinaryOperator\<T> : Function의 자손, 두 매개변수와 결괏값의 타입이 동일

컬렉션 프레임웍과 함수형 인터페이스

- 컬렉션 프레임웍의 인터페이스의 다수의 디폴트 메서드 추가
- 그중 일부는 함수형 인터페이스를 사용
- Collection 인터페이스
  - boolean removeif(Predicate\<E> filter) : 조건에 맞는 요소를 삭제
- List 인터페이스
  - void replaceAll(UnaryOperator\<E> operator) : 모든 요소를 변환하여 대체
- Iterable 인터페이스
  - void forEach(Consumer\<T> action) : 모든 요소에 작업 action을 수행
- Map 인터페이스
  - V compute(K key, BiFunction\<K,V,V> f) : 지정된 키의 값에 작업 f를 수행
  - V computeIfAbsent(K key, Function\<K,V> f) : 키가 없으면 작업 f 수행 후 추가
  - V conputeIfPresent(K key, BiFunction\<K,V,V> f) : 지정된 키가 있을 때 작업 f 수행
  - V merge(K key, V value, BiFunction\<K,V,V> f) : 모든 요소에 병합작업 f를 수행
  - void forEach(BiConsumer\<K,V> action) : 모든 요소에 작업 action을 수행
  - void replaceAll(BiFunction\<K,V,V> f) : 모든 요소에 치환작업 f를 수행

기본형을 사용하는 함수형 인터페이스

- 기본형 대신 래퍼 클래스로 사용하는 것은 비효율적
- 보다 효율적인 처리를 위해 기본형을 사용하는 함수형 인터페이스들 제공
- 종류
  - DoubleToIntFunction : 입력이 double 타입, 출력이 int 타입
  - ToIntFunction\<T> : 입력은 지네릭, 출력은 int 타입
  - IntFunction\<R> : 입력이 int 타입, 출력은 지네릭 타입
  - ObjIntConsumer\<T> : 입력이 지네릭과 int, 출력은 없음

### 1.5 Function의 합성과 Predicate 결합

- static 메서드로 정의되어 있음

Function의 합성

- 두 람다식을 합성해서 새로운 람다식을 만들 수 있음
- f.andThen(g)
  - f의 결과값을 g로 받아서 값을 반환
- f.compose(g)
  - andThen과 반대
  - g의 결과값을 f로 받아서 값을 반환
- identity()
  - 항등 함수가 필요할 때 사용
  - 람다식으로 표현 시 `x->x`
  - map()으로 변환작업할 때 변환 없이 그대로 처리하고자 할 때 사용

Predicate의 결합

- 조건식들을 논리 연산자로 하나의 식으로 구성할 수 있듯이 여러 Predicate도 결합 가능
- and(), or(), negeate()로 연결
- negate는 `!`의 의미

### 1.6 메서드 참조

- 람다식을 더욱 간결하게 할 수 있는 방법
- 람다식이 하나의 메서드만 호출하는 경우에만 사용
- 우변 메서드의 선언 부분이나 좌변 인터페이스에 지정된 지네릭 타입으로부터 알아낼 수 있는 내용을 생략
- 표현
  - static 메서드, 인스턴스 메서드 참조는 `클래스이름::메서드이름`로 표현
  - 특정 객체 인스턴스메서드 참조는 `참조변수::메서드이름`와 같은 방식으로 표현

생성자의 메서드 참조

- `클래스이름::new`와 같은 방식으로 표현
- 배열의 경우엔 `자료형[]::new`와 같은 방식으로 표현

## 2. 스트림(stream)

### 2.1 스트림이란?

- for문과 Iterator의 단점
  - 코드가 길다.
  - 알아보기 어려움
  - 재사용성 떨어짐
  - 데이터 소스마다 다른 방식으로 다뤄야 함
- 이러한 문제를 해결하기 위해 `스트림`을 만듦
  - 데이터 소스를 추상화
  - 컬렉션, 파일에 저장된 데이터를 모두 같은 방식으로 다룸

스트림은 데이터 소스를 변경하지 않는다.

- 데이터를 읽기만 할 뿐 변경 X
- 변경 필요시 결과를 컬렉션, 배열 등에 할당

스트림은 일회용이다.

- 스트림은 Iterator처럼 일회용
- 한번 사용하면 닫혀서 다시 사용 불가
- 재사용 필요시 재생성

스트림은 작업을 내부 반복으로 처리한다.

- 내부 반복이라는 것은 반복문을 메서드의 내부에 숨길 수 있다는 의미
- forEach()는 메서드 안으로 for문을 넣은 것

스트림의 연산

- 중간 연산
  - 연산 결과가 스트림
  - 연속해서 중간 연산 가능
- 최종 연산
  - 연산 결과가 스트림이 아닌 연산
  - 스트림의 요소를 소모하므로 단 한 번만 가능

지연된 연산

- 스트림은 최종 연산이 수행되기 전까지 중간 연산이 수행 X
- 최종 연산이 수행되어야 스트림의 요소들이 중간 연산을 거쳐 최종 연산에서 소모

Stream\<Integer>와 IntStream

- 오토박싱&언박싱으로 인한 비효율을 줄이기 위해 기본형으로 데이터 소스의 요소를 다루는 스트림도 제공
- IntStream, LongStream, DoubleStream 제공

병렬 스트림

- 내부적으로 fork&join 프레임웍을 이용해서 자동적으로 연산을 병렬로 수행
- parallel()을 호출하면 병렬로 연산을 수행하도록 지시할 수 있음
- 병렬로 처리되지 않게 하려면 sequential() 호출
- 모든 스트림은 기본적으로 병렬 스트림이 아님
- 병렬처리가 항상 더 빠른 결과를 얻게 해주는 것은 아님

### 2.2 스트림 만들기

컬렉션

- Stream\<T> Collection.stream() 사용

배열

- Stream\<T> Stream.of(~) 메서드로 스트림 생성
- int 등의 기본형은 `IntSTream IntStream.of(~)` 메서드로 생성

특정 범위의 정수

- IntStream, LongStream이 갖고 있는 메서드
- 연속된 정수의 스트림을 생성 후 반환
- IntStream.range(int begin, int end)
  - end 값 미포함
- IntStream.rangeClosed(int begin, int end)
  - end 값 포함

임의의 수

- Random 클래스에서 제공하는 난수를 생성하는 데 사용되는 메서드
- Intstream ints(), LongStream longs(), DoubleStream doubles()
- 크기가 정해지지 않은 무한 스트림을 반환
  - limit()을 같이 사용하여 크기를 제한해 줄 것
- 매개변수로 범위를 지정할 수 있음 (end 값 미포함)

람다식 - iterate(), generate()

- 람다식을 매개변수로 받아서 계산되는 값들을 요소로 하는 무한 스트림 생성
- iterate는 씨앗 값으로 지정된 값부터 시작, 람다식에 의해 계산된 결과를 다시 seed 값으로 해서 계산 반복
- generate는 매개변수가 없는 람다식(Supplier)만 허용하기 때문에, 이전 값으로 계산 X
- 둘에 의해 생성된 스트림을 기본형 타입의 참조 변수로 다루기 위해선 mapToInt()와 같은 변환 메서드 필요
  - 반대는 boxed() 메서드 사용

파일

- java.nio.file.Files에서 지정된 디렉터리에 있는 파일의 목록을 소스로 하는 스트림을 생성 후 반환하는 메서드 지원
- Stream\<Path> File.list(Path dir)
- 파일의 한 행을 요소로 하는 스트림 생성 메서드도 존재
  - Files.lines(~)

빈 스트림

- 요소가 비어있는 스트림 생성 시 null 보단 Stream.empty()를 사용해서 만들 것
- null을 할당해도 오류는 아니다.

두 스트림의 연결

- 같은 타입의 두 스트림을 concat()으로 연결할 수 있다.

### 2.3 스트림의 중간 연산

스트림 자르기 - skip(), limit()

- skip()은 요소를 건너 뜀
- limit은 스트림의 요소를 제한(잘라냄)
- 기본형 스트림에도 정의되어 있음

스트림 요소 걸러내기 - filter(), distinct()

- filter()는 주어진 조건(Predicate)에 맞지 않는 요소를 걸러냄
  - 연산 결과가 boolean인 람다식을 사용해도 된다.
  - 필요하다면 filter()를 다른 조건으로 여러 번 사용 가능
- distinct()는 중복된 요소를 제거

정렬 - sorted()

- sort()는 지정된 Comparator로 스트림을 정렬
  - Comparator 대신 int 값을 반환하는 람다식 사용 가능
- Comparator가 지정되어 있지 않으면 스트림 요소의 기본 정렬 기준(Comparable)로 정렬
  - JDK 1.8부터 Comparator 인터페이스에 static 메서드와 디폴트 메서드가 많이 추가
  - 비교 대상이 기본형인 경우 thenComparing()이 효율적

변환 - map()

- 원하는 필드만 뽑아내거나 특정 형태로 변환해야 할 때 사용
- T 타입을 R 타입으로 변환해서 반환하는 함수를 지정해야 함
- 하나의 스트림에 여러 번 적용 가능

조회 - peek()

- 연산과 연산 사이에 올바르게 처리되었는지 확인할 때 사용
- forEach와는 달리 스트림 요소 소모 X

mapToInt(), mapToLong(), mapToDouble()

- Stream\<T> 타입의 스트림을 기본형 스트림으로 변환할 때 사용하는 메서드들
- 기본형만 다룰 땐 연산 시 따로 기본형으로 변환할 필요가 없으므로 효율적
- Stream\<T> 보다 숫자를 다루는데 편리한 메서드들을 더 제공함
  - sum(), average(), max(), min()
  - 이 메서드들은 최종 연산이기 때문에 호출 후 스트림이 닫힘
  - 스트림을 다시 생성해야 하는 불편함을 덜기 위해 `summaryStatistics()` 메서드 제공
- mapToObj() : 기본형 스트림을 Stream\<T>으로 변환
- boxed() : IntStream을 Stream\<Integer>로 변환

flatMap() - Stream\<T[]>를 Stream\<T>로 변환

- 스트림의 요소나 map()의 연산 결과가 배열일 경우 Stream\<T>로 변환하기 위해 flatMap() 사용
  - flatMap()은 스트림의 스트림이 아닌 스트림으로 만들어준다.
- 스트림의 스트림을 하나의 스트림으로 합칠 때도 flatMap() 사용

### 2.4 Optional\<T>과 OptionalInt

- Optional\<T>는 지네릭 클래스로 `T 타입의 객체를 감싸는 래퍼 클래스`
  - Optional 타입의 객체에는 모든 타입의 참조 변수를 담을 수 있음
  - private final T value 필드를 class 내부에서 가지고 있음
- 반환된 결과가 null 인지 if문으로 체크하는 대신 Optional에 정의된 메서드로 처리 가능

Optional 객체 생성하기

- of() 또는 ofNullable()을 사용
  - 참조 변수의 값이 null 일 가능성이 있으면 ofNullable() 사용
  - null로 초기화 하기보단 empty()로 초기화하는 것이 바람직

Optional 객체의 값 가져오기

- Optional 객체의 값은 get()으로 가져올 수 있음
  - null 일 때는 NoSuchElementException 발생
- 예외를 대비해서 orElse()로 대체할 값 지정 가능
  - orElseGet()으로 null 값을 대체할 람다식 지정 가능
  - orElseThrow()로 null 일 때 지정된 예외를 발생시킬 수도 있음
- 값의 존재를 확인할 수 있는 isPresent() 메서드와 값이 있을 때 사용할 수 있는 ifPresent() 메서드 존재
- Stream 클래스에 정의된 메서드 중에서 Optional\<T>을 반환하는 것들이 존재
  - findAny(), findFirst(), max(), min(), reduce()

OptinalInt, OptionalLong, OptionalDouble

- 기본형을 값으로 하는 Optinal들
- IntStream 등에도 해당 클래스를 반환하는 메서드 존재
- 기존의 get()이 getAsInt() 등 메서드 이름이 조금씩 다름
- 기본형 int가 0 일 때와 아무런 값을 갖지 않는 상황을 구별하기 위해 클래스 내부에 isPresent라는 인스턴스 변수를 두고 구분

### 2.5 스트림의 최종 연산

- 최종 연산은 스트림의 요소를 소모 후 결과를 만들어냄
- 최종 연산 후엔 스트림이 닫히고 더 이상 사용 불가
- 최종 결과는 단일 값이거나 배열 또는 컬렉션

forEach()

- 반환 타입 void
- 스트림의 요소를 출력하는 등의 용도로 사용

조건 검사 - allMatch(), anyMatch(), noneMatch(), findFirst(), findAny()

- 스트림의 요소에 대해 지정된 조건으로 확인하는 데 사용할 수 있는 메서드들
- 연산 결과로 boolean 반환
- findAny()와 findFirst()의 반환 타입은 Optional\<T>
  - 요소가 없을 시 비어있는 Optional 객체를 반환

통계 - count(), sum(), average(), max(), min()

- 스트림의 요소들에 대한 통계 정보를 얻을 수 있는 메서드들
- 기본형이 아닐 시엔 count(), max(), min() 세 가지 메서드들만 사용 가능
  - 위의 메서드들을 사용하기보단 기본형 스트림으로 변환하거나 reduce()와 collect()를 사용해서 통계 정보를 얻음

리듀싱 - reduce()

- 스트림의 요소를 줄여나가면서 연산을 수행한 뒤 최종 결과를 반환
- 처음 두 요소를 가지고 연산한 결과를 그다음 요소와 연산
  - 매개변수의 타입 BinaryOperator\<T>
- 매개변수가 초기값을 갖는 reduce()는 초기값과 스트림의 첫 번째 요소로 연산을 시작
  - 스트림의 요소가 없을 시 초기값이 반환
  - 초기값이 있으므로 반환 타입이 Optional이 아닌 T (초기 값이 없는 reduce 메서드는 반환값 Optional)
- combiner 매개변수는 병렬 스트림에 의해 처리된 결과를 합칠 때 사용하는 매개변수

### 2.6 collect()

- 스트림의 최종 연산 중 가장 복잡하면서도 유용하게 활용
- 리듀싱과 유사한 방식
- collect()의 매개변수로 어떻게 수집할 것인가에 대한 방법이 정의되어야 하는데 그 방법을 정의한 것이 collector
- collect()를 사용하기 위해 알아야 하는 것
  - collect() : 스트림의 최종 연산, 매개변수로 컬렉터를 필요로 함
  - Collector : 인터페이스, 컬렉터는 이 인터페이스를 구현해야 함
  - Collectors : 클래스, static 메서드로 미리 작성된 컬렉터를 제공
- 매개변수로 Collector 구현체가 들어간 collect 메서드와 Supplier, BiConsumer 두 개가 들어간 메서드 존재
  - 두 번째 메서드는 잘 사용되지 않는다.

스트림을 컬렉션과 배열로 변환 - toList(), toSet(), toMap(), toCollection, toArray()

- List는 Collectors.toList() 사용
- 특정 컬렉션은 toCollection()에 해당 컬렉션의 생성자 참조를 매개변수로 넣기
  - ex) ArrayList::new
- Map의 경우엔 toMap 메서드의 파라미터로 키와 값을 정해줘야 함
- 스트림에서 저장된 요소들을 배열로 반환하려면 (stream 참조 변수).toArray() 사용
  - 원하는 타입의 생성자 참조를 매개변수로 지정
  - 매개변수를 지정하지 않을 시 배열의 타입은 Object[]

통계 - counting(), summingInt(), averageInt(), maxBy(), minBy()

- collect()가 아니어도 위의 연산들의 결과를 얻을 수 있지만 collect()를 사용하면 groupingBy()와 함께 사용 가능

리듀싱 - reducing()

- collect()에서 리듀싱도 사용 가능
- 매개변수에 따라 3가지 종류가 있음

문자열 결합 - joining()

- 문자열 스트림의 모든 요소를 하나의 문자열로 연결해서 반환
- 구분자와 접두사, 접미사도 지정 가능
