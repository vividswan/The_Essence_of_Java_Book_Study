# Chapter 10 - 날짜와 시간 & 형식화

## 1. 날짜와 시간

### 1.1 Calendar와 Date

- Date는 JDK 1.0부터 제공
- Calendar는 JDK 1.1부터 제공
- Calendar와 Date의 단점은 JDK 1.8의 java.time 패키지에서 개선한 클래스 제공

Calendear와 GregorianCalendar

- Calendar는 추상 클래스
- `Calendar.getInstance()`로 구현된 클래스의 인스턴스를 얻을 수 있음
  - 직접 생성자를 쓰기보단 getInstance를 사용
  - 태국을 제외한 나머지 국가는 GregorianCalendar

Date와 Calendar 간의 변환

- Calendar to Date
  - Calendar cal = Calendar.getInstance();
  - Date d = new Date(cal.getTimeInMillis());
- Date to Calendar
  - Date d = new Date();
  - Calendar cal = Calendar.getInstance();
  - cal.setTime(d);
- Calendar 인스턴스의 get 매개변수로 사용되는 int 값들은 Calendar에 정의된 static 상수
- Calendar.MONTH의 범위는 0~11임을 주의
- Calendar 인스턴스의 날짜와 시간을 원하는 값으로 변경하려면 set 메서드 사용
- 두 날짜 간의 차이는 두 날짜를 초 단위로 변경 후 차이를 구하면 됨
  - getTimeInMillis()는 1/1000초 단위로 값을 반환
- Calendar 인스턴스의 add()를 사용하면 지정한 필드의 값을 원하는 만큼 증가나 감소 가능
  - roll() 메서드는 다른 월이나 일 필드 등에 영향을 미치지 않고 증가나 감소

## 2. 형식화 클래스

- java.text 패키지에 포함
- 숫자, 날짜, 텍스트 데이터를 일정한 형식에 맞게 표현할 수 있는 방법을 객체지향으로 설계 후 표준화한 것
- 형식화 및 역으로 원래의 데이터를 얻을 수도 있음

### 2.1 DecimalFormat

- 숫자를 형식화하는데 사용
- 사용법
  1. 원하는 출력 형식의 패턴을 작성 후 DecimalFormat 인스턴스 생성
  2. 출력하고자 하는 문자열로 format 메서드를 호출
- parse() 메서드를 이용하면 기호와 문자가 포함된 문자열을 숫자로 변환 가능

### 2.2 SimpleDateFormat

- SimpleDateFormat으로 날짜 데이터(Date 객체)를 원하는 형태로 다양하게 출력
- 사용법
  1. 원하는 출력 형식의 패턴을 작성하여 SimpleDateFormat 인스턴스 생성
  2. 출력하고자 하는 Date 인스턴스를 가지고 format(Date d)를 호출
- Date 인스턴스만 format 메서드에 사용될 수 있기 때문에 Calendar 인스턴스는 Date 인스턴스로 변환 후 사용
- parse() 메서드로 날짜 데이터의 출력 형식을 변환 가능
  - 문자열 source를 날짜 source로

### 2.3 ChoiceFormat

- 특정 범위에 속하는 값을 문자열로 변환
- 연속적 또는 불연속적인 범위의 값을 처리하는 데 도움
- 경곗값은 반드시 오름차순으로 정렬
- 치환될 문자열의 개수는 경곗값에 의해 정의된 범위의 개수와 일치해야 함
  - 그렇지 않을 시 IllegalArgumentException 발생
- 패턴은 구분자로 '#'와 '<'
  - '#'는 경곗값을 범위에 포함
  - '<'는 포함 X

### 2.4 MessageFormat

- 데이터를 정해진 양식에 맞게 출력할 수 있도록 도와줌
- parse()를 이용하면 지정된 양식에서 필요한 데이터만을 손쉽게 추출
- 양식 문자열을 작성할 때 '{숫자}'로 표시된 부분이 데이터가 출력될 자리
- 양식에 들어갈 데이터는 객체 배열로 지정
- Database에 저장할 Insert 문으로 변환하는 경우에 사용하기 편리함

## 3. java.time 패키지

- Date와 Calendar가 가지고 있는 단점들을 해소하기 위해 JDK1.8부터 java.time 패키지 추가
- 이 패키지에 속한 클래스들은 `불변(immutable)`

### 3.1 java.time 패키지의 핵심 클래스

- LocalDate(날짜) + LocalTime(시간) -> LocalDateTime (날짜 & 시간)
- LocalDateTime + 시간대 -> ZonedDateTime

Period와 Duration

- 날짜 - 날짜 = Period
- 시간 - 시간 = Duration

객체 생성하기 - now(), of()

- now()는 현재 날짜와 시간을 저장하는 객체 생성
- of()는 해당 필드의 값을 순서대로 지정해 주면 그에 맞는 시간의 객체 생성

Temporal과 TemporalAmount

- Temporal, TemporalAccessor, TemporalAdjuster를 구현한 클래스
  - LocalDate, LocalTime, LocalDateTime, ZonedDateTime, Instant 등
- TemporalAmount를 구현한 클래스
  - Period, Duration

TemporalUnit과 TemporalField

- TemporalUnit : 날짜와 시간의 단위를 정의해 놓은 인터페이스
  - 구현체 : 열거형 ChronoUnit
- TemporalField : 년, 월, 일 등 날짜와 시간의 필드를 정의해 놓은 것
  - 구현체 : 열거형 ChronoField
- 날짜와 시간에서 특정 필드의 값만을 얻을 때는 get(), 혹은 get으로 시작하는 이름의 메서드 사용
- 특정 날짜와 시간에서 저장된 단위의 값을 더하거나 뺄 때는 plus() 또는 minus()에 값과 함께 열거형 ChronoUnit 사용

### 3.2 LocalDate와 LocalTime

- java.time 패키지의 가장 기본이 되는 클래스
- now(), of() static 메서드로 인스턴스 생성
  - now()는 현재의 날짜나 시간
  - of()는 지정된 날짜나 시간
- parse()로 문자열을 날짜와 시간으로 변환도 가능

특정 필드의 값 가져오기 - get(), getXXX()

- 주의사항
  - Calendar와 달리 월의 범위가 1~12
  - 요일은 월요일부터 1
- get(), getLong()으로 원하는 필드 직접 지정 or getXXX() 메소드로 특정 값 가져오기 가능
- get(), getLong()의 매개변수는 ChronoField에 정의된 상수들
  - 사용할 수 있는 필드는 클래스마다 다름
- chronoField.XXX.range()로 특정 필드가 가질 수 있는 값의 범위 조회 가능

필드의 값 변경하기 - with(), plus(), minus()

- 날짜와 시간에서 특정 필드 값을 변경하려믄 with로 시작하는 메서드 사용
  - 원하는 필드를 직접 지정 가능
  - LocalDate, LocalTime은 불변이므로 대입 연산자 사용해서 할당할 것
- 특정 필드에 값을 더하거나 빼는 plus()와 minus()도 존재
- LocalTime의 truncatedTo()는 지정된 것보다 작은 단위의 필드를 0으로 만듦
  - 년, 월, 일은 0이 될 수 없기 때문에 LocalDate는 truncatedTo()가 없음

날짜와 시간의 비교 - isAfter(), isBefore(), isEqual()

- compareTo() 보다 편하게 비교할 수 있는 메서드들을 제공
- isAfter(), isBefore(), isEqual()
- 연표가 다른 날짜는 isEqual()로 비교

### 3.3 Instant

- Instant는 에포크 타임부터 경과된 시간을 나노초 단위로 표현
- now(), ofEpochSecond()로 생성 가능
- now.getEpochSecond(), now.getNano() 메서드로 값을 가져올 수 있음
  - 시간을 초 단위와 나노초 단위로 나누어 저장

Instant와 Date간의 변환

- Date로 변환 : from(Instant instant)
- Instant로 변환 : toInstant()

### 3.4 LocalDateTime과 ZonedDateTime

- LocalDate + LocalTime -> LocalDateTime
- LocalDateTime + 시간대 -> ZonedDateTime

LocalDate와 LocalTime으로 LocalDateTime 만들기

- LocalDate와 LocalTime을 합쳐서 하나의 LocalDateTime을 만들 수 있음
- 날짜와 시간을 직접 정하는 of()나 now()를 통해서도 LocalDateTime 생성 가능

LocalDateTime의 변환

- toLocalDate()으로 LocalDate으로 변환 가능
- toLocalTime()으로 LocalTime으로 변환 가능

LocalDateTime으로 ZonedDateTime 만들기

- LocalDateTime에 시간대를 추가해서 만듦
- ZoneId를 따름 (일광 절약시간을 자동으로 처리)
- atZone()으로 시간대 정보를 추가

ZoneOffset

- UTC로부터 얼마만큼 떨어져 있는지를 ZoneOffset로 표현
- 서울은 +9 (9시간 빠름)

OffsetDateTime

- ZoneId가 아닌 ZoneOffset을 사용하는 것이 OffsetDateTime
- ZoneOffset은 단지 시간대를 시간의 차이로만 구분

ZonedDateTime의 변환

- ZoneDateTime도 날짜와 시간에 관련된 다른 클래스로 변환하는 메서드들을 가지고 있음
- ZonedDateTime을 GregorianCalendar로 바꾸기 위해선 `from(ZonedDateTime)` 메서드 사용
- GregorianCalendar을 ZonedDateTime로 바꾸기 위해선 `toZonedDateTime()` 메서드 사용

### 3.5 TemporalAdjusters

- java.time 패키지의 클래스들에서 자주 쓰일만한 날짜 계산들을 대신해주는 메서드를 정의해놓은 것이 TemporalAdjusters 클래스

TemporalAdjusters 직접 구현하기

- LocalDate의 with() 메서드는 TemporalAdjusters 인터페이스를 구현한 객체를 매개변수로 받음
- FunctionalInterface인 adjustInto()를 오버라이딩 후 구현

### 3.6 Period와 Duration

- 날짜 - 날짜 = Period
- 시간 - 시간 = Duration

between()

- 두 날짜와 시간의 차이를 나타내는 period를 구할 수 있음
- 앞의 파라미터가 날짜 상으로 이전이면 양수, 반대면 음수

between()과 until()

- 거의 같은 일을 함
- between()은 static 메서드, until은 인스턴스 메서드라는 게 차이

of(), with()

- ofXXX() 등의 메서드 존재
- with()는 특정 필드의 값을 변경

사칙연산, 비교연산, 기타 메서드

- 곱셈과 나눗셈을 위한 메서드도 존재
  - multipliedBy()
  - dividedBy()
- 부호 관련 메서드도 존재
  - 반대로 변경 : negate()
  - 부호를 없앰 : abs()
- normalized() : 월의 값이 12를 넘지 않게 변경해 줌
  - 일의 길이는 그대로 놔둠 (일정하지 않기 때문에)

다른 단위로 변환 -toTotalMonths(), toDays(), toHours(), toMinutes()

- Period와 Duration을 다른 단위의 값으로 변환하는 데 사용
- get()은 특정 필드의 값을 그대로 가져옴
- 특정 단위로 변환한 결과를 반환하는 메서드들도 존재
- LocalDate의 toEpochDay() 메서드는 Epoch Day(1970.01.01)부터 날짜를 세어 반환
  - Epoch Day 이후의 두 날짜 간의 일수를 편리하게 계산

### 3.7 파싱과 포맷

- 파싱 : 날짜와 시간을 원하는 형식으로 출력 & 해석
- 형식화와 관련된 클래스들은 `java.time.format` 패키지
  - 다양한 형식 제공 & 그 외의 형식은 직접 정의해서 사용
  - LocalDate나 LocalTime 같은 클래스에도 존재 format() 메서드 존재

로케일에 종속된 형식화

- `ofLocalizedDate()`, `ofLocalizedTime()`, `ofLocalized DateTime()`은 로케일에 종속적인 포맷터를 생성

출력 형식 직접 정의하기

- DateTimeFormatter의 ofPattern()으로 원하는 출력 형식을 직접 작성할 수도 있음

문자열을 날짜와 시간으로 파싱하기

- 문자열을 날짜 또는 시간으로 변환하려면 static 메서드인 parse()를 사용
  - 날짜와 시간을 표현하는 데 사용되는 클래스에는 이 메서드가 거의 다 포함
