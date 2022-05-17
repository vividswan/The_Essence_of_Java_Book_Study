# Chapter 14 - 입출력 I/O

## 1. 자바에서의 입출력

### 1.1 입출력이란?

- Input & Output
- 컴퓨터 내부 또는 외부의 장치와 프로그램 간의 데이터를 주고받는 것

### 1.2 스트림(stream)

- 데이터를 운반하는 데 사용되는 연결통로
- 두 대상을 연결하고 데이터를 전송할 수 있는 것
- 단 반향 통신만 가능
- 큐와 같은 FIFO 구조

### 1.3 바이트 기반 스트림 - InputStream, OutputStream

- 바이트 단위로 데이트를 전송
- 대상에 맞는 스트림을 선택 후 사용
- 바이트 기반 스트림은 모두 InputStream, OutputStream의 추상 메서드를 구현한 자손들
  - read()와 write() 추상 메서드를 구현 후 해당 메서드를 기반으로 사용

### 1.4 보조 스트림

- 스트림의 기능을 향상시키거나 새로운 기능을 추가할 수 있음
  - 실제 데이터를 주고받는 스트림은 아니므로 입출력 불가능
- 보조 스트림은 모두 FilterInputStream의 자손
  - FilterInputStream은 InputStream의 자손

### 1.5 문자 기반 스트림 - Reader, Writer

- Java에서는 char형이 2byte
  - 2byte 단위로 처리하기 위한 문자 기반의 스트림이 Reader, Writer
- 바이트 기반 스트림과 이름만 조금 다를 뿐 활용방법은 거의 같음

## 2. 바이트 기반 스트림

### 2.1 InputStream과 OutptStream

- 모든 바이트 기반의 스트림의 조상
- InputStream엔 available(), close(), mark, markSupported(), read(), reset(), skip() 등의 메서드 존재
- OutputStream엔 close(), flush(), write() 등의 메서드 존재

### 2.2 ByteArrayInputStream과 ByteArrayOutputStream

- 바이트 배열에 데이터를 입출력하는데 사용되는 스트림
- 주로 다른 곳에 입출력하기 전에 데이터를 임시로 바이트 배열에 담아서 변환 등의 작업을 하는데 사용
- 바이트 배열은 사용하는 자원이 메모리므로 사용하지 않을 시 가비지 컬렉터에 의해 자동으로 자원 반환
  - close()로 닫을 필요는 없음
- 배열을 담을 수 있는 read()와 write()로 오버 로딩 된 메서드를 사용하면 더 효율적으로 입출력 가능
  - 한 번에 더 많은 물건을 옮기듯 배열의 크기만큼 읽거나 쓸 수 있음
- read()와 wirte()가 발생시킬 수 있는 IOException을 처리하기 위해 try-catch 문으로 감싸주어야 한다.

### 2.3 FileInputStream과 FileOutputStream

- 파일에 입출력하기 위한 스트림
  - 실제 프로그래밍에서 많이 사용되는 스트림 중 하나
- 텍스트 파일을 다루는 경우엔 문자 기반의 스트림인 FileReader/FileWriter를 사용하는 것이 더 좋음

## 3. 바이트 기반의 보조 스트림

### 3.1 FilterInputStream과 FilterOutputStream

- 자체적으로 입출력이 불가능한 보조 스트림
  - 생성자의 매개변수로 InputStream, OutputStream이 들어가야 한다.
- FilterInputStream의 생성자가 protected이기 때문에 인스턴스를 생성해서 사용할 순 없고 상속을 통해서 오버라이딩 되어야 함

### 3.2 BufferdInputStream과 BufferedOutputStream

- 스트림의 입출력 효율을 높이기 위해 버퍼를 사용하는 보조 스트림
  - 효율적이므로 대부분 입출력에 사용
- 버퍼의 크기는 입력 소스로부터 한 번에 가져올 수 있는 데이터 크기로 지정하면 좋음
  - 보통 입력 소스가 파일인 경우 8K의 크기로 하는 것이 보통
  - 버퍼의 크기를 변경해가면서 테스트 가능
- 외부의 입력 소스로 부터 읽는 것보다 내부의 버퍼로 읽는 것이 훨씬 빠르므로 효율이 높음
- BufferedOutputStream은 버퍼가 가득 차면 버퍼의 모든 내용을 출력 소스에 출력
  - 출력 후 버퍼를 비우고 출력을 저장할 준비
  - 마지막 출력 부분이 출력 소스에 쓰이지 못할 것을 대비해 close()나 flush()를 호출할 것
- 보조 스트림을 사용한 경우엔 기반 스트림의 flosh()나 close()를 호출할 필요가 없음
  - 보조 스트림 내부에 기반 스트림의 close()가 구현

### 3.3 DataInputStream과 DataOutputStream

- 데이터를 읽고 쓰는 데 있어서 byte 단위가 아닌 8가지 기본 자료형의 단위로 읽고 쓸 수 있음
- 출력 형식은 각 기본 자료형 값을 16진수로 표현하여 저장
- 출력한 데이터를 다시 읽어 올 때는 출력했을 때의 순서를 염두
- 출력한 값들은 이진 데이터로 저장된다.
  - 문자 데이터가 아니므로 파일을 16진수 코드로 볼 수 있는 프로그램이나 ByteArrayOutputStream을 사용해야 함
  - DataOutputStream의 wirte 메서드들로 기록한 데이터는 DataInputStream의 read 메서드로 읽으면 됨
- 데이터를 읽는 메서드가 더 이상 읽을 데이터가 없으면 EOFException 발생
  - 스트림을 닫아줘야 함

### 3.4 SequenceInputStream

- 여러 개의 입력 스트림을 연속으로 연결해서 하나의 스트림으로부터 데이터를 읽는 것과 같이 처리
  - 큰 파일을 여러 개의 작은 파일로 나누었다가 하나의 파일로 합치는 것과 같은 작업을 수행할 때 사용
- 생성자로 Enumeration이나 두 개의 스트림을 받음

### 3.5 PrintStream

- 데이터를 기반 스트림에 다양한 형태로 출력할 수 있는 print, println, printf와 같은 메서드를 오버 로딩하여 제공
- PrintStream과 PrintWriter는 거의 같은 기능이지만 PrintWriter가 다양한 언어의 문자를 처리하는데 적합하므로 가능하면 PrintWriter를 사용할 것
  - JDK1.1부터 추가되었음
- print(), println()은 매우 자주 사용되므로 예외를 던지지 않고 내부에서 처리하도록 정의
- printf()는 JDK1.5부터 추가되었고 C언어와 같이 편리한 형식화된 출력을 지원
  - 옵션의 개수와 매개변수의 개수가 일치해야 함
  - '숫자$'를 옵션 앞에 붙여 출력된 매개변수 지정 가능

## 4. 문자 기반 스트림

### 4.1 Reader와 Writer

- 바이트 기반 스트림의 조상인 InputStream, OutputStream과 같은 역할을 문자 기반의 스트림에서 수행
- 2 byte로 스트림을 처리하며 인코딩을 지원해 줌
  - 자바에서 사용하는 유니코드 간의 변환을 자동적으로 처리
  - Reader는 특정 인코딩을 읽어서 유니코드로 변환
  - Writer는 유니코드를 특정 인코딩으로 변환하여 저장

### 4.2 FileReader와 FileWriter

- 파일로부터 텍스트 데이터를 읽고 파일을 쓰는데 사용

### 4.3 PipedReader와 PipedWriter

- 스레드 간에 데이터를 주고받을 때 사용
- 입력과 출력 스트림을 하나의 스트림으로 연결해서 데이터를 주고받음
- 사용법
  - 스트림 생성 후 한쪽 스레드에서 connect()를 호출해서 입력 스트림과 출력 스트림을 연결
  - 입출력을 마친 후에는 어느 한쪽 스트림만 닫아도 나머지 스트림은 자동으로 닫힘

### 4.4 StringReader와 StringWriter

- 입출력 대상이 메모리인 스트림
- StringWriter에 출력되는 데이터는 내부의 StringBuffer에 저장
  - getBuffer(), toString() 등의 메서드를 이용해서 데이터를 얻어올 수 있음

## 5. 문자 기반의 보조 스트림

### 5.1 BufferedReader와 BufferdWriter

- 버퍼를 이용해서 입출력의 효율을 높일 수 있도록 해주는 역할
  - 버퍼를 이용하는 것이 비교할 수 없을 정도로 효율이 좋아진다.
- BufferedReader의 readLine()을 사용하면 데이터를 라인 단위로 읽을 수 있음
- BufferWriter는 newLine()이라는 줄바꿈을 해주는 메서드를 가지고 있음

### 5.2 InputStreamReader와 OutputStreamWriter

- 바이트 기반 스트림을 문자 기반 스트림으로 연결해 주는 역할
- 바이트 기반 스트림의 데이터를 지정된 인코딩의 문자 데이터로 변환하는 작업 수행
- 생성자로 스트림과 인코딩을 넣을 수 있음
  - 인코딩을 지정해 주지 않는다면 OS에서 사용하는 인코딩을 사용
  - 한글 윈도우에서 중국어로 작성된 파일을 읽는 상황 등에서 인코딩을 알맞게 지정해 주어야 함
- getEncoding()으로 사용 중인 OS의 인코딩 확인 가능

## 6. 표준 입출력과 File

### 6.1 표준 입출력 - System.in, System.out, System.err

- 표준 입출력이란?
  - 콘솔(console, 도스창)을 통한 데이터 입력과 콘솔로의 데이터 출력을 의미
- 자바에선 표준 입출력을 위해 3가지 입출력 스트림을 제공
  - System.in, System.out, System.err
  - 개발자가 별도로 스트림 생성하는 코드를 작성할 필요 X
  - 자바 어플리케이션의 실행과 동시에 자동적으로 생성
- System.in : 콘솔로부터 데이터를 입력받는데 사용
- System.out : 콘솔로 데이터를 출력하는 데 사용
- System.err : 콘솔로 데이터를 출력하는 데 사용
- 모두 System 클래스에 선언된 클래스 변수
- BufferdInputStream과 BufferdOutputStream을 사용
- 콘솔 입력
  - 버퍼를 가지고 있기 때문에 Backspace를 이용해서 편집 가능
  - Enter 키나 입력의 끝을 알리는 '^z'를 누르기 전까진 데이터를 입력 중인 것으로 간주
  - '^z'를 누를 시 read()가 -1을 반환
  - Enter를 누르면 캐리지리턴과 줄 바꿈 문자도 사용자키로 인식됨 -> BufferedReader의 newLine()을 통해 라인 단위로 데이터를 받는 것으로 해결

### 6.2 표준 입출력의 대상 변경 - setOut(), setErr(), setIn()

- setOut(), setErr(), setIn()을 사용해 입출력을 콘솔 이외에 다른 입출력 대상으로 변경하는 것이 가능
- 선언한 메서드의 매개변수에 들어 있는 스트림으로 입력 및 출력 변경 가능
- 메서드를 선언하는 방법 외에도 커맨드 라인에서 표준 입출력의 대상을 변경할 수 있음
  - '>' : System.out의 출력을 콘솔이 아닌 다른 곳으로 지정, 기존의 내용 삭제
  - '>>' : System.out의 출력을 콘솔이 아닌 다른 곳으로 지정, 기존의 내용에 새로운 내용 추가
  - '<' : 표준 입력을 콘솔이 아닌 다른 곳으로 지정

### 6.3 RandomAccessFile

- RandomAccessFile은 하나의 클래스로 파일에 대한 입력과 출력을 모두 할 수 있도록 되어있음
- 기본 자료형 단위로 데이터를 읽고 쓸 수 있음 (DataInputStream, DataOutputStream 구현)
- RandomAccessFile 클래스는 파일의 어느 위치에나 읽고 쓰기가 가능한 것이 가장 큰 장점
  - 위치에 제한이 X
  - 다른 입출력 클래스들은 입출력 소스에 순차적으로 읽기/쓰기를 함
  - 내부적으로 파일 포인터를 사용
- getFilePointer()를 통해 현재 포인트 위치를 찾을 수 있음
- write 이후엔 파일 포인터를 처음으로 이동시킨 다음에 read에 관한 메서드 사용
  - seek(0)로 이동

### 6.4 File

- 기본적이면서도 가장 많이 사용되는 입출력 대상
- 자바에서는 File 클래스를 통해 파일과 디렉토리를 다룰 수 있도록 지원
  - File 인스턴스는 파일 일 수도 있고 디렉토리일 수도 있다.
  - 생성자에 경로와 파일 이름을 넣을 수 있음 (경로가 없을 시 현재 실행되는 위치를 경로로)
- 경로의 구분자는 OS마다 다르므로 클래스 변수를 사용해야 함
- 절대 경로는 파일시스템의 루트로부터 시작하는 파일의 전체 경로를 의미
  - 기호나 링크 등을 포함하지 않는 유일한 경로를 정규 경로라고 함
- 파일을 생성하기 위해선 우선 File 인스턴스를 생성한 후에, 출력 스트림을 생성하거나 createNewFile()을 호출
- 디렉토리에 포함된 파일과 서브 디렉토리의 목록도 확인 가능
- accept 메서드를 구현하는 FilenameFilter를 사용해 특정 조건에 맞는 파일의 목록을 얻을 수도 있음
- delete() 메서드로 삭제, renameTo()를 이용해서 파일의 이름도 변경 가능

## 7. 직렬화(Serialization)

- 직렬화는 네트워크를 통해 컴퓨터 간에 서로 객체를 주고받는 것을 가능하게 해줌

### 7.1 직렬화란?

- 객체를 데이터 스트림으로 만드는 것
  - 객체에 저장된 데이터를 스트림에 쓰기 위해 연속적인 데이터로 변환하는 것
- 스트림으로부터 데이터를 읽어서 객체를 만드는 것은 역직렬화
- 객체의 모든 인스턴스 변수의 값을 저장 후 저장했던 객체를 다시 생성할 때는 객체를 생성 후에 저장한 값을 읽어오는 방식
  - 인스턴스 변수의 값이 참조형일 때는 복잡함

### 7.2 ObjectInputStream, ObjectOutputStream

- 직렬화엔 ObjectInputStream 사용
- 역직렬화엔 ObjectOutputStream 사용
- 두 개 모두 보조 스트림이므로 기반 스트림을 지정해 주어야 함
- readObject()의 반환 타입은 Object이므로 형변환이 필요
- 자동 직렬화가 편리하긴 하지만 직렬화 작업 시간을 단축시키기 위해서는 직렬화하고자 하는 객체의 클래스에 writeObject와 readObject를 구현해 주어야 함

### 7.3 직렬화가 가능한 클래스 만들기 - Serializable, transient

- 직렬화가 가능한 클래스를 만들기 위해선 Serializable 인터페이스를 구현
- Serializable은 아무런 내용이 없는 빈 인터페이스
  - 직렬화를 고려하여 작성한 클래스인지를 판단해 주는 기준
  - Serializable을 구현한 클래스의 자손은 Serializable을 구현할 필요 없음
  - 조상 클래스에서 Serializable를 구현하지 않으면 조상의 인스턴스 변수들이 직렬화 대상에 포함되지 않음 (코드를 직접 추가해야 함)
- 직렬화할 수 없는 클래스는 직렬화 시 java.io.NotSerializableException 발생
  - 모든 클래스의 최고 조상인 Object는 Serializable을 구현하지 않았기 때문에 직렬화할 수 없음
- 인스턴스 변수 타입이 아닌 실제로 연결된 객체의 종류에 의해 직렬화가 결정됨
- 직렬화하고자 하는 객체의 클래스에 직렬화가 안 되는 객체에 대한 참조를 포함하고 있을 시 제어자 transient를 붙여서 직렬화 대상에서 제외되도록 할 수 있음
- 역직렬화 시 직렬화했던 순서대로 처리해야 함
  - 이런 이유로 객체를 개별적으로 직렬화 하기보단 ArrayList와 같은 컬렉션에 저장해서 직렬화하는 게 좋음
- private void writeObject(ObjectOutputStream out), private void readObject(ObjectInputStream in)을 구현하여 조상으로부터 상속받은 인스턴스 변수에 대한 직렬화를 구현할 수 있음

### 7.4 직렬화 가능한 클래스의 버전 관리

- 클래스의 이름이 같더라도 클래스의 내용이 변경된 경우 역직렬화는 실패하며 예외가 발생함
  - 이를 해결하기 위해 객체가 직렬화될 때 클래스에 정의된 멤버들의 정보를 이용해서 serialVersionUID라는 클래스의 버전을 자동 생성해서 직렬화 내용에 포함
  - 역직렬화 시 클래스의 버전을 비교함으로써 직렬화할 때의 클래스 버전과 일치하는지 확인
- static 변수, 상수, transient는 직렬화에 영향을 끼치지 않기 때문에 클래스의 버전을 다르게 인식하도록 할 필요 X
- 클래스가 조금만 변경되어도 재배포하는 게 어려운 경우 클래스의 버전을 수동으로 관리할 수 있음
  - static final long serialVersionUID를 추가로 정의
  - 정의 시 버전이 자동 생성된 값으로 변경되지 않음
  - 보통 클래스끼리 다른 값을 갖지 않도록 serialver.exe를 사용해서 생성된 값을 사용 (클래스의 정보로 UID를 생성 혹은 값을 조회 가능)
