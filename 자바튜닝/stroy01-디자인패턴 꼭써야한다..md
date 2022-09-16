# 디자인패턴 꼭써야한다 - 01

## MVC

 + Model :  view에서 입력된 내용 저장 관리
 + Controller : view - model 연결자
 + 





## 패턴 

 + 무엇인가 만들기 위한 모델이나 가이드 , 설명의 집합
 + 즉 시스템을 만들기 위해 전체 중 일부 의미 있는 클래스들을 묶은 각각 집합을 디자인 패턴


### Transfer Object 패턴 

 + Value Object 로 불림 
 + 데이터를 전송하기 위한 객체에 대한 패턴
 + 보통 마이바티스에서 데이터 담는 객체 ? 이해 하면 편할듯 
 + Get / set 메서드를 만들지 않을때 성능상 더 빠름 
 + 이 패턴을 사용한다고, 성능은 좋아지지 않지만 - > 객체에 결과값을 담아 올 수 있어서 여러번 요청하는 일을 줄여 줌.




# 내가 만든 프로그램의 속도를 알고 싶다. - 02


 + 성능이 느릴 땐 병목 지점 파악하자



## 프로파일링 툴

 + 시스템 문제 분석 툴이라 이해 
 + ex) APM (Application Performance Monitoring / Management)
 + 프로파일링 툴 : 개발자 용 , 소스 레벨의 분석을 위하는 툴 , 애플리케이션의 세부 응답 시간까지 분석
 + 메모리 사용량을 객체나 클래스 소스의 라인 단위까지 분성  / APM에 비해 저렴 / 자바 기반의 클라이언트 프로그램 분석 할 수 있음

 
 + APM 툴 : 애플리케이션 장애 상황에 대한 모니터링 및 문제점 진단 주 목적
 + 서버의 사용자 수나 리소스 대한 모니터링
 + 실시간 모니터링을 위한 툴 / 가격 비쌈  / CPU 수 기반 가격 정해짐 / 자바 기반의 클라이언트 프로그램 분석 X


### 응답시간 프로파일링 기능 (프로파일링 툴 제공 기능)

 + 응답시간 측정 하기 위함.
 + 보통 CPU 시간과 대기시간 제공 .  
 + 하나의 메서드 한 라인 수행하는대 소요되는 시간은 무조건 CPU 시간과 대기 시간으로 나뉨

### 메모리 프로파일링

 + 잠깐 사용하고 GC의 대상이 되는 부분 찾거나 메모리 릭 발생 부분 찾기 위함.


## System 클래스

 + 배열 복사기능 있음.
 + System.nanoTime() 메서드는 나노단위로 현재시간을 리턴 / currentTimeMillis 보다 빠르다.
 + 나노타임 쓰는걸 권 (성능 측정할 떄)

### JMH

    @BenchmarkMode({ Mode.AverageTime })  // 평균 응답시간 측정
    @OutputTimeUnit(TimeUnit.MILLISECONDS)  // 시간 단위를 밀리초로

    public class CompareTimerJMH {


    @GenerateMicroBenchmark  // 측정 대상이 되는 메서드 선언 
    public DummyData makeObject() {
        HashMap<String , String> map new HashMap<String, String> (1000000);
        ArrayList<String> list = new ArrayList<String>(100000);
        return new DummyData(map,list)
        }
    }


# 왜 자꾸 String을 쓰지 말라는 거야 - 03

  + String은 메모리 낭비
  + StringBuffer / StingBuiler를 사용하면 메모리 낭비가 적고 성능적으로 훨씬 우수함.
  + 두 클래스는 CharSequence 구현체임  CharSequence로 받아서 처리하면 메모리 효율에 더 좋음.



## String vs StringBuffer  vs StringBuilder

 + String : 짧은 문자열 더할때
 + buffer : 스레드에 안전한 프로그램이 필요할때, 스레드에 안전한지 모를경우 사용 ex) singleton 클래스 OR static 선언 문자열
 + builder : 스레드 안전한지여부 관계없는 프로그램 개발  Ex) 메서드 내에 변수를 선언하면 해당 변수는 메서드 내에만 살아있으므로 사용해도됨 .

### jdk 5.0

 + jdk 5.0 이상에서는 컴파일러가 자동으로 StringBuilder로 변환해줌. (책에 예시는 스트링 외에 다른 클래스가 섞여서 + 될경우..? ex) "hello" + 1 이런식 int가.. )
 + 다른 사람 코드를 보니 순수 문자열 "hi"  + "test" 이런거는 치환이 안돼고, "hi" + str(String str="1111") 이런거는 빌더로 치환됨 .
 + 허나 반복 루프를 사용해서 문자열 더할때는 객체를 계속 추가한다는 사실이 변함없으므로 String 클래스를 지양하자 (bulider OR buffer도 자동치환경우 새로운객체를 매번 생성함.)
 + 마지막으로 자동치환을 해도 어짜피 문자열을 더할때 객체를 계속 생성하므로 String으로 많은 문자열 더하는건 지양을 하자 .



# 어디에 담아야 하는지 ..  - 04


## List 

 + Vector : 객체 생성시 크기를 지정할 필요 없는 배열 클래스 
 + ArrayList : vector 비슷 동기화처리 X
 + LikedList : ArrayList 동일  Queue 인터페이스 구현했기때문에 FIFO 작업 수행 
  
 + 데이터를 넣는 속도는 차이가 없음 (LikedList는 Peek() 순차적으로 결과받아오는 메서드 사용)
 + Vector는 동기화 선언때문에 다른스레드 접근을  막아서 느림.
 + 

## Map

  + HashTable: 데이터를 해쉬 테이블에 담음 -> 내부에서 관리하는 해쉬테이블 객체가 동기화 되있어서 , 동기화가 필ㅇ한 부분에서 사용
  + HashMAp : 데이터를 해쉬 테이블에 담는 클래스, 다른점은 null 값 허용 동기화되어있지않음
  + TreeMap: red-black 트리 키에 의해 순서 정해짐.
  + LinkedHashMap : HashMap 동일 이중 연결 리스트 방식 사용하여 데이터 담음 (앞뒤로 노드에 대한 링크정보 가지고 있음)


## Queue

 + 선입 선출

### List 보단 왜 Queue?

 + List는 데이터 삭제시 쉬프트 때문에 처리시간이 길어짐. (데이터가 많을 수록 지우는데 소요시간이 증가됨.)
 + Queue 구현체 : LinkedList / PriorityQueue (추가된 순서와 상관없이 먼저 생성된 객체가 먼저나오는 큐)


## Set 


### 누가 가장 빠름?

 + hashSet / LinkedHashSet 
 + TreeSet : 가장 느림 , 데이터를 저장하면서 정렬함  즉 데이터를 순서에 따라 탐색하는 작업일때는 TreeSet이 좋음


## Collection 관련 클랫 ㅡ동기화