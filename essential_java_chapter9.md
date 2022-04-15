# Chapter 09 - java.lang 패키지와 유용한 클래스

## 1. java.lang 패키지

- 자바 프로그래밍에 가장 기본이 되는 클래스들 포함
- import문 없이도 사용 가능

### 1.1 Object 클래스

Object 클래스의 메서드

- **protected** Object clone() : 객체 자신의 복사본을 반환
- public boolean equals(Object obj) : 객체 자신과 객체 obj가 같은 객체 여부 반환
- **protected** void finalize() : 객체가 소멸될 때 가비지 컬랙터에서 자동 호출되며 수행해야 할 코드가 있을 시 오버라이딩 (거의 사용 X)
- public Class getClass() : 객체 자신의 클래스 정보를 담고 있는 Class 인스턴스 반환
- public int hashCode() : 객체 자신의 해시코드 반환
- public String toString() : 객체 자신의 정보를 문자열로 반환
- public void notify() : 객체 자신을 사용하려고 기다리는 쓰레드를 하나만 깨움
- public void notifyAll() : 객체 자신을 사용하려고 기다리는 모든 쓰레드를 깨움
- public void wait(), public void wait(long timeout), public void wait(long timeout, int nanos) : 다른 쓰레드가 notify()나 notifyAll()을 호출할 때까지 현재 쓰레드를 무한히 또는 지정된 시간 동안 기다리게 함

eqauls(Object obj)

- 매개변수로 객체의 참조변수를 받아서 비교 후 그 결과를 boolean 값으로 알려줌
- 서로 다른 두 객체는 false
- 참조변수에 저장된 값(주솟값)으로 비교
- equals 메서드는 오버라이딩으로 주소가 아는 객체에 저장된 내용을 비교하도록 변경 가능

hashCode()

- 해싱 기법에 사용되는 `해시함수(hash function)`을 구현한 것
- Object 클래스에 정의된 hashCode() 메서드는 객체의 주솟값으로 해시코드를 만들어서 반환
  - 해시코드는 4Byte이므로 32bit JVM에서는 중복될 수 없지만 64bit JVM에서는 중복될 수 있다.
- 객체의 같고 다름을 판단하기 위해선 eqauls() 메서드뿐만 아니라 hashCode() 메서드도 오버라이딩 해야 함
  - 같은 객체라면 hashCode() 메서드를 호출했을 때의 결과 값인 해시코드도 같아야 하므로
- String 클래스는 문자열의 내용이 같으면 동일한 해시코드를 반환하도록 오버라이드 되어 있음
  - 반면 System.identityHashCode(Object x) 메서드는 모든 객체에 대해 항상 다른 값을 반환하는 것을 보장

toString()

- 인스턴스에 대한 정보를 문자열로 제공
- 오버라이딩 하지 않으면 클래스 이름과 16진수의 해시 코드를 얻을 수 있음
- Object 클래스의 toString() 메서드의 접근 제어자가 public이므로 모든 자손 클래스는 접근 제어자를 public으로 해야 함

clone()

- 자신을 복제하여 새로운 인스턴스를 생성
- 인스턴스의 값만 복사하기 때문에 참조타입의 인스턴스 변수가 있는 클래스는 완전한 인스턴스 복제가 이루어지지 않음
  - clone() 메서드를 오버라이딩 해서 해결해야 함
- clone()을 사용하기 위해서 Cloneable 인터페이스를 구현한 뒤 접근자를 public으로 오버라이딩(다른 클래스에서 clone() 메서드를 호출할 수 있도록)
  - 인스턴스의 데이터를 보호하기 위해 이 같은 제약을 둠

공변반환타입

- JDK 1.5부터 추가
- 오버라이딩할 때 조상 메서드의 반환타입을 자손 클래스의 타입으로 변경을 허용
- 번거로운 형변환을 줄임

얕은 복사와 깊은 복사

- clone()은 단순히 객체에 저장된 값을 그대로 복제하는 것이지 객체가 참조하고 있는 객체까지 복사 X
  - 이러한 복제를 `얕은 복사`라고 부름
  - 얕은 복사는 원본을 변경하면 복사본도 영향을 받음
- 원본이 참조하고 있는 객체까지 복제하는 것이 `깊은 복사`
  - 원본의 변경이 복사본에 영향을 미치지 않음

getClass()

- Class 객체를 반환하는 메서드
- Class 객체는 클래스의 모든 정보를 담고 있고 클래스 당 1개가 존재
- 클래스 로더가 실행 시 필요한 클래스를 동적으로 메모리에 로드하는 역할
  - 기존에 생성된 클래스 객체가 메모리에 존재하는지 확인 후 있으면 객체의 참조, 없으면 클래스 패스에 지정된 경로를 따라서 클래스 파일을 찾음
- 클래스 파일을 읽어서 사용하기 편한 형태로 저장해 놓은 것이 클래스 객체

Class 객체를 얻는 방법

- new 생성자.getClass();
- 클래스명.class;
- Class.forName("클래스명");
  - forName()은 특정 클래스 파일을 올릴 때 주로 사용 (ex. 데이터베이스 드라이버)
- Class 객체를 이용하면 클래스에 정의된 멤버의 이름, 개수 등 클래스에 대한 모든 정보를 얻을 수 있음
  - 보다 동적인 코드 생성 가능

### 1.2 String 클래스

자바에서 제공하는 문자열을 위한 클래스

변경 불가능한 클래스

- 배열 참조변수 (char[]) value를 인스턴스 변수로 정의
- 인스턴스가 갖고 있는 문자를 읽어올 순 있지만 변경 불가능
- '+' 연산자는 문자열이 바뀌는 것이 아니라 새로운 문자열이 담긴 String 인스턴스가 생성
- 문자열 간의 변경이 많이 필요할 경우에는 StringBuffer 클래스를 사용할 것

문자열의 비교

- 문자열 리터럴을 저장하는 방법, String 클래스의 생성자를 사용하는 방법
- 리터럴은 이미 존재하는 것을 재사용
- String 클래스의 생성자는 new 연산자에 의해 메모리 할당이 이루어지고 항상 새로운 인스턴스 생성

문자열 리터럴

- 자바 소스파일의 모든 문자열 리터럴은 컴파일 시 클래스 파일에 저장
- 그러므로 하나의 인스턴스를 공유
- 클래스 파일이 클래스 로더에 의해 메모리에 올라갈 때 리터럴의 목록에 있는 리터럴들이 JVM 내에 있는 `상수 저장소(constant pool)`에 저장됨

빈 문자열

- 길이가 0인 배열 존재 가능
  - C언어에선 불가능
- `String s = "";`는 내부에 길이가 0인 char 배열을 저장하고 있는 것
- `char c ='';`라는 표현은 불가능
  - char형 변수에는 반드시 하나의 문자를 지정해야 함
- String형의 기본값은 null이 아닌 빈 문자열
- char형의 기본값은 공백으로 초기화

String 클래스의 생성자와 메서드

- String(String s) : 문자열 s를 갖는 String 인스턴스 생성
- String(char[] value) : 주어진 문자열 value를 갖는 String 인스턴스 생성
- String(StringBuffer buf) : StringBuffer 인스턴스가 갖고 있는 문자열과 같은 내용의 String 인스턴스를 생성
- char charAt(int index) : 지정된 위치에 있는 문자를 리턴
- int compareTo(String str) : 문자열과 사전 순서로 비교
  - 같으면 0, 사전 순 이전은 음수, 이후는 양수
- String concat(String str) : 문자열 뒤에 덧붙임
- boolean contains(CharSequence s) : 지정된 문자열을 포함하고 있는지 검사
- boolean endsWith(String suffix) : 지정된 문자열로 끝나는지 검사
- boolean equals(Object obj) : 문자열의 내용을 비교 후 결과 리턴
- boolean equalsIgnoreCase(String str) : 문자열을 대소문 구별 없이 내용 비교
- int indexOf(int ch) : 주어진 문자가 문자열에 존재하는지 확인하여 위치 반환
  - 없으면 -1 반환
  - 인덱스는 0부터 시작
- int indexOf(int ch, int pos) : 주어진 문자가 지정된 위치부터 확인하여 문자열에 존재하는지 확인하여 위치 반환
  - 없으면 -1 반환
  - 인덱스는 0부터 시작
- int indexOf(String str) : 주어진 문자열이 존재하는지 확인 후 그 위치를 반환
  - 없으면 -1 반환
  - 인덱스는 0부터 시작
- String intern() : 문자열을 상수풀에 등록
  - 이미 같은 내용이 상수풀에 등록되어 있을 시 그 문자열의 주솟값을 반환
- int lastIndexOf(int ch) : 지정된 문자 또는 문자 코드를 문자열의 오른쪽 끝에서 찾아서 위치 반환
  - 없으면 -1 반환
- int lastIndexOf(String str) : 지정된 문자열을 인스턴스 문자열의 오른쪽 끝에서 찾아서 위치 반환
  - 없으면 -1 반환
- int length() : 문자열의 길이 return
- String replace(char old, char nw), String replace(CharSequence old, CharSequence nw) : 문자열 중의 첫 번째 파라미터와 일치하는 문자열을 두 번째 파라미터로 바꾼 문자열 반환
- String replaceAll(String regex, String replacement) : 문자열 중에서 지정된 문자열과 일치하는 것을 새로운 문자열로 변경
- String[] split(String regex) : 문자열을 지정된 분리자로 나누어 문자열 배열 반환
- String[] split(String regex, int limit) : 문자열을 지정된 분리자로 나누어 문자열 배열을 지정된 수로 잘라서 반환
- boolean startWith(String prefix) : 주어진 문자열로 시작하는지 검사
- String substring(int begin), String substring(int begin, int end) : 주어진 시작 위치부터 끝 범위에 포함된 문자열 return
  - 시작 위치는 문자의 범위에 포함, 끝 위치는 포함 X
- String toLowerCase() : 모든 문자열을 소문자로 변환하여 반환
- String toString() : String 인스턴스에 저장되어 있는 문자열을 반환
- String toUpperCase() : String 인스턴스에 저장되어 있는 모든 문자열을 대문자로 변환하여 반환
- String trim() : 문자열의 왼쪽 끝, 오른쪽 끝의 공백을 없앤 결과 return
  - 문자열 중간에 있는 공백은 제거 X
- static String valueOf(매개변수) : 지정된 값을 문자열로 변환하여 반환
  - 참조변수의 경우엔 toString()을 호출한 결과 반환
  - 매개변수로 boolean, char, int, long, float, double, Object 가능

join()과 StringJoiner

- join()은 여러 문자열 사이에 구분자를 넣어서 결합
- split()과 반대의 작업
- StringJoiner 클래스로 문자열 결합도 가능
  - ex) new StringJoiner(",", "[", "]");
  - 생성자 이후 문자열을 add

유니코드의 보충 문자

- 유니코드는 원래 2byte 였지만 `보충 문자`들은 20bit로 확정하게 되었음
  - 하나의 문자를 char 타입으로 다룰 수 없고 int로 다룰 수밖에 없게됨
- String의 메서드 중 매개변수가 int인 메서드들은 보충 문자를 지원

문자 인코딩 변환

- getBytes(String charsertName)를 사용하면 문자열의 문자 인코딩을 다른 인코딩으로 변경 가능

String.format()

- format은 형식화된 문자열을 만들어내는 간단한 방법
- printf()와 사용법이 완전히 동일

기본형 값을 String으로 변환

- 숫자의 빈 문자열 ""을 더하는 방법
- `String.valueOf()`를 사용하는 방법
  - 성능은 빈 문자열을 더하는 방법보다 좋음

String을 기본형 값으로 변환

- valueOf()를 사용
- parseInt()와 같은 메서드를 사용
  - parseInt()에서 반환 타입은 Integer이지만 오토박싱에 의해 int로 자동 변환
- parseInt()나 parseFloat() 같은 메서드는 문자열에 공백 또는 문자가 포함되어 있는 경우 변환 시 NumberFormatException 발생
  - trim()을 사용해 주면 공백 제거 가능
- float형 값을 의미하는 f와 같은 자료형 접미사는 알맞은 변환 시 허용

### 1.3 StringBuffer 클래스와 StringBuilder 클래스

- StringBuffer 클래스는 지정된 문자열을 변경 가능
  - 내부에 편집을 위한 buffer를 가지고 있음
  - 인스턴스 생성 시 크기 지정 가능
- 편집할 문자열의 길이를 고려하여 버퍼의 길이를 충분히 잡아줄 것

StringBuffer의 생성자

- 생성 시 적절한 길이의 char형 배열 생성 (buffer로 이용)
- 생성자 `StringBuffer(int length)`를 사용해서 문자열의 길이를 충분히 여유 있는 크기로 지정할 것
- 버퍼 크기를 지정하지 않을 시 16개의 문자를 저장할 수 있는 버퍼 생성
- 배열의 길이가 넘어가면 내부에서 이전의 배열의 값을 복사한 새로운 배열 생성 후 교체

StringBuffer의 변경

- appned()로 추가 가능
  - 반환타입이 StringBuffer -> 자신의 주소를 반환
  - append로 받은 참조변수는 모두 같은 StringBuffer 인스턴스를 가리킴

StringBuffer의 비교

- StringBuffer는 equals 메서드를 오버라이딩 하지 않음
- 등가비교연산자(==)와 같음
- toString()은 오버라이딩 되어있음 -> 담고 있는 문자열을 String으로 반환
  - toString() 후 eqauls로 비교 가능

StringBuffer 클래스의 생성자와 메서드

- StringBuffer() : 16문자의 버퍼를 가진 인스턴스 생성
- StringBuffer(int length) : 지정된 길이의 문자를 담을 수 있는 버퍼를 가진 인스턴스 생성
- StringBuffer(String str) : 지정된 문자열 값을 갖는 인스턴스 생성
  - 지정된 문자열보다 16자리 더 큰 버퍼로 생성
- StringBuffer append(매개변수) : 매개 변수로 입력된 값을 문자열로 반환하여 인스턴스의 문자열 뒤에 덧붙임
  - 매개변수로 boolean, char, char[], double, float, int, long, Object, String 가능
- int capacity() : 인스턴스의 버퍼 크기를 반환
- char charAt(int idndex) : 지정된 인덱스에 있는 문자를 반환
- StringBuffer delete(int start, int end) : 시작 위치부터 끝 위치 사이에 있는 문자를 제거
  - 끝 위치의 문자는 제외
- StringBuffer deleteCharAt(int index) : 지정된 위치의 문자를 제거
- StringBuffer insert(int pos, 매개변수) : 두 번째 매개변수로 받은 값을 문자열로 변환 후 지정된 위치에 추가
  - pos는 0부터 시작
  - 두 번째 매개변수는 boolean, char, char[], double, float, int, long, Object, String이 들어올 수 있음
- int length() : 인스턴스에 저장되어 있는 문자열의 길이 반환
- StringBuffer replace(int start, int end, String str) : 지정된 범위의 문자들을 주어진 문자열로 바꿈
  - end 위치의 문자는 범위에 포함 X
- StringBuffer reverse() : 인스턴스에 저장되어 있는 문자열의 순서를 거꾸로 **나열**
- void setCharAt() : 지정된 위치의 문자를 주어진 문자로 교체
- void setLength(int newLength) : 지정된 길이로 문자열의 길이를 변경, 길이를 늘리는 경우 빈 공간은 널문자('\u0000')
- String toString() : StringBuffer 인스턴스의 문자열을 String으로 반환
- String subString(int String), String subString(int start, int end) : 지정된 범위 내의 문자열을 String으로 뽑아서 반환
  - 시작 위치만 지정 시 시작 위치부터 문자열 끝까지

StringBuilder란?

- StringBuffer는 멀티쓰레드에 안전하도록 동기화
  - 멀티쓰레드로 작성된 프로그램이 아니라면 동기화는 성능을 떨어뜨림
- StringBuffer에서 쓰레드의 동기화만 뺀 것이 StringBuilder

### Math 클래스

- 기본적인 수학계산에 유용한 메서드로 구성
- Math 클래스의 모든 메서드는 static

올림, 버림, 반올림

- 반올림한 값을 얻기 위해선 `round()`를 사용
  - 10, 100등을 곱한 뒤 결과에 round() 사용 후 다시 10.0, 100.0으로 나누기
- `rint()`는 두 정수의 정가운데 있는 가장 가까운 짝수 정수를 반환
  - 음수에서 주의
- `ceil()`은 올림, `floor()`는 버림

예외를 발생시키는 메서드

- 정수형 간의 연산에서 발생할 수 있는 오버플로우를 감지하기 위한 것
- int addExact(int x, int y), int subtractExact(int x, int y) 등등..
- 연산자는 단지 결과를 반환하지만 메서드들은 예외를 발생
  - `ArithmeticException`

삼각함수와 지수, 로그

- sqrt, pow, sin, cos, log 등등 ..
- 삼각함수는 라디안이 매개변수 (toRadians를 사용해 double로 바꿀 수 있음)

StrictMath 클래스

- Math 클래스는 JVM이 설치된 OS의 메서드를 호출해서 사용
  - OS에 의존적인 계산
- 성능을 다소 포기하고 어떤 OS에서든지 같은 결과를 얻기 위한 것이 StrictMath 클래스

### 1.5 래퍼(wrapper) 클래스

- 자바는 8개의 기본형이 있고 기본형을 객체로 다뤄야 할 때 사용되는 것이 래퍼(wrapper) 클래스
- 생성자의 매개변수로 문자열을 제공할 때 각 자료형에 알맞은 문자열을 사용할 것
  - `NumberFormatException` 등의 예외주의
- wrapper 클래스들은 모두 equals()가 오버라이딩 되어 있음

Number 클래스

- 기본형 중에서 숫자와 관련된 래퍼 클래스들은 모두 Number 클래스가 조상
- 그 외엔 BigInteger, BigDecimal -> 큰 범위의 정수 및 부동 소수점수를 처리

문자열을 숫자로 변환하기

- 타입.parse타입(String s)
  - 반환값이 기본형
- 타입.valueOf()
  - 반환값이 래퍼 클래스 타입, parse 타입 보단 성능이 조금 느리다.
- JDK 1.5부터 도입된 `오토박싱` 기능으로 인해 반환값이 기본형일 때와 래퍼 클래스일 때의 성능 차이가 없어짐
- parseInt, Integer.valueOf에 두 번째 매개변수로 진법의 숫자를 넣을 수 있음

오토박싱 & 언박싱

- 기본형과 참조형 간의 연산이 가능
  - 컴파일러가 변환하는 코드를 자동으로 넣어줌
- 내부적으로 객체 배열을 가지고 있는 클래스들도 컴파일러가 자동으로 형변환 코드를 추가
- 래퍼 클래스의 객체로 변환해주 는 것이 오토박싱
- 반대로 변환해 주는 것이 언박싱

## 2 유용한 클래스

### 2.1 java.util.Objects 클래스

- Object 클래스의 보조 클래스
- 모든 메서드가 static
- `isNull()`은 해당 객체가 null 인지 유무를 return
- `requreNonNull()`은 해당 객체가 null이 아니어야 하는 경우에 사용
  - 해당 객체가 null일 시 NullPointException 발생
  - 두 번째 매개변수로 예외 메시지 지정 가능
- `compare()`은 두 비교 대상이 같으면 0, 크면 양수, 작으면 음수 반환
  - 기준의 역할은 `Comparator`
- Objects의 `equals()`는 null 검사를 하지 않아도 OK
- `deepEquals()`는 객체를 재귀적으로 비교하므로 다차원의 배열 비교도 가능
- `hashCode()`는 내부적으로 null 검사를 한 후 Object 클래스의 hashCode() 호출
  - null 일 때는 0 반환

### 2.2 java.util.Random 클래스

- Math.random()은 내부적으로 Random 클래스의 인스턴스를 생성해서 사용하는 것
  - Math.random()과 다르게 Random은 종자값(seed)를 설정 가능

Random 클래스의 생성자와 메서드

- 생성자 Random()은 종자값을 System.currentTimeMillis()로 함
- Random() : 현재 시간을 종자값으로 이용하는 Random 인스턴스 생성
- Random(long seed) : 매개변수 seed를 종자값으로 하는 Random 인스턴스를 생성
- nextBoolean(), nextBytes(Bytep[] bytes), nextDouble(), nextFloat() : next 뒤 타입의 난수를 반환
- void setSeed(long seed) : 종자값을 주어진 값(seed)으로 변경
- double nextGaussian() : 평균은 0.0이고 표준편차는 1.0인 가우시안 분포에 따른 double형의 난수를 반환

### 2.3 정규식(Regular Expression) - java.util.regex 패키지

- Perl의 강력한 기능을 지원
- 정규식 정의와 데이터 비교 과정
  1. Pattern 클래스의 compile 호출 후 pattern 인스턴스를 얻음
  2. Pattern 클래스의 Matcher 인스턴스를 호출
  3. Matcher 인스턴스의 matches() 메서드를 통해 정규식에 부합하는지 확인
- 정규식의 일부를 괄호로 묶어서 그루핑 가능
  - group(0)은 매칭된 문자열 전체
- Mather의 find()로 찾은 정규식과 일치하는 문자를 start(), end()로 위치를 알아낼 수 있고 appendReplacement()로 원하는 문자열 치환 가능

### 2.4 java.util.Scanner 클래스

- Scanner는 화면, 파일, 문자열과 같은 입력 소스로부터 문자 데이터를 읽어옴
- 정규식 표현을 이용한 라인 단위의 검색 지원, 구분자(delimiter)에도 정규식 표현을 사용할 수 있음
- JDK 1.5부터 Scanner 사용, JDK 1.6 이후엔 입출력만 전문적으로 담당하는 java.io.Console 추가(이클립스와 같은 IDE에서 동작 X)
- 입력받을 값에 따라 nextLine(), nextInt(), nextLong() 등등 존재

### 2.5 java.util.StringTokernizer 클래스

- StringTokernizer는 긴 문자열을 지정된 구분자(delimiter)를 기준으로 토큰이라는 여러 개의 문자열로 잘라내는 데 사용
- String의 split, Scanner의 useDelimiter로 분리할 수 있지만 이 두 가지 방식은 정규식을 사용해야 하므로 정규식에 익숙해야 함
  - 그러나 StringTokernizer는 구분자로 단 하나의 문자 밖에 사용하지 못함 (ex-"+-\*/"를 구분자 자리에 넣으면 각각의 문자가 구분자)
  - String의 split()은 빈 문자열도 토큰으로 인식, StringTokernizer은 빈 문자열은 토큰으로 인식 X
  - String의 split()은 결과를 배열에 담기 때문에 토큰으로 바로바로 잘라서 반환하는 StringTokernizer보다 느림
- StringTokernizer 생성자에 매개 변수가 두 개 일 땐 문자열과 구분자, 세 개 일 땐 마지막 매개 변수는 구분자도 토큰으로 하는지의 여부
  - 메서드는 countTokens(), hasMoreTokens(), nextToken() 존재

### 2.6 java.math.BigInteger 클래스

- long으로 표현할 수 있는 값보다 더 큰 값을 다룰 때 사용
- BigInteger는 내부적으로 int 배열을 사용해서 값을 다룸
  - 큰 값을 다룰 수 있지만 성능은 long보다 떨어짐
  - `final int signum`으로 부호, `final int[] mag`로 값
  - BigInteger는 불변

BigInteger의 생성

- 문자로 숫자열을 표현해서 생성
- 두 번째 파라미터로 진수를 주고 생성도 가능
- 숫자로 생성도 가능

다른 타입으로의 변환

- toSTring(), toString(int radix), byte[] toByteArray() 변환 메서드
- ~Value()로 기본형으로 변환하는 메서드 존재
- 끝에 `Exact`를 붙여서 변환한 타입의 범위에 속하지 않으면 예외를 발생시키는 메서드도 존재

BigInteger의 연산

- 정수형에 사용할 수 있는 모든 연산자 & 수학적인 계산을 지원해 주는 메서드들 정의
- BigInteger는 불변이므로, 반환 타입이 BigInteger

비트 연산 메서드

- 큰 숫자를 다룰 때 성능 향상을 위해 비트 단위로 연산을 수행하는 메서드들도 존재
- testBit(0)으로 가장 마지막 비트를 확인하여 짝수, 홀수 구분을 효율적으로 가능

### 2.7 java.math.BigDecimal 클래스

- BigDecimal은 정수를 이용해서 실수를 표현
  - 실수를 정수와 10의 제곱의 곱으로 표현
- 구성
  - private final BigInteger intVal; : 정수
  - private final int scale; : 지수
  - private transient int precision; : 정밀도(정수의 전체 자릿수)

BigDecimal의 생성

- 문자열로 숫자를 표현하는 것이 일반적
  - 기본적 리터럴로는 표현의 한계가 있음
  - double, int, long 타입으로도 가능
- BigDecimal.valueOf(~)로도 생성 가능
- double 타입의 값을 매개변수로 갖는 생성자는 오차가 발생할 수 있음

다른 타입으로의 변환

- String toPlainString() : 다른 기호 없이 숫자로만 표현
- String toString : 지수 형태로도 표현 가능
- ~Value() : 기본형으로 변환하는 메서드도 갖고 있음
- 끝에 `Exact`가 붙은 변환한 타입의 범위에 속하지 않으면 예외 처리를 발생시키는 메서드도 존재

BigDecimal의 연산

- 실수형에 사용할 수 있는 모든 연산자 & 수학적 계산을 쉽게 해주는 메서드들이 정의
- BigDecimal도 불변이므로 새로운 BigDecimal 타입의 인스턴스를 반환
- 연산 결과의 정수, 지수, 정밀도가 달라짐

반올림 모드 - divide()와 setScale()

- 나눗셈을 처리하기 위한 메서드는 다양한 버전이 존재
- 나눗셈의 결과를 어떻게 반올림할 것인지, 몇 번째 자리에서 반올림할 것인지를 결정
- `ROUND_` 상수를 매개 변수로 반올림 처리 방법을 결정
- divide()로 나눗셈한 결과가 무한 소수인 경우 반올림 모드를 지정해 주지 않으면 `ArithmeticException` 발생

java.math.MathContext

- 반올림 모드와 정밀도를 하나로 묶어 놓은 클래스
- divide()에서는 scale이 소수점 이하의 자릿수를 의미
  - MathContext에서는 precision이 정수와 소수점 이하를 모두 포함한 자릿수를 의미

scale의 변경

- BigDecimal을 10으로 곱하거나 나누는 대신 scale의 값을 변경함으로써 같은 결과를 얻을 수 있음
- seScale() 메서드로 변경 가능
