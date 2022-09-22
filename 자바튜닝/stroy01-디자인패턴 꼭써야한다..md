# 디자인패턴 꼭써야한다 - 01

## MVC

 + Model :  view에서 입력된 내용 저장 관리
 + Controller : view - model 연결자

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


# for 루프를 더 빠르게 - 05

 + if문을 쓴다고 큰 성능저하가 많이 일어나지않음
 + if 보단 case문이 가독성이 좋아 숫자분기땐 권장 허나 조건이많으면 좋지 않다. 시간이 오래걸리니


### 반복문 주의사항

 + for(int loop=0jloop<list.size()jloop++) -> 계속 size함수를 호출하게된다..
 + int listSize = list.size()로 해주고 for문 돌려 주자. (근데 실제로 차이별로안나는 거같음)
 + for-each 문은 For문보다 느리다.
 + 반복문을 돌릴때 함수가 계속 호출되는것을 지양하자.



# static 제대로 한번 써보자 - 06


 + 자주 사용하고 절대 변하지않는 변수로 Final static 선언
 + 보통 변수에 선언하면 요청할때마다 계속 불러야되지만
 + 스태틱을 사용하면 한번만 하면 그 다음엔 안불러도되서 읽기전용일때 속도가 빨라짐 
 + 읽기전용으로 쓰지않고 계속 바뀌는 값으로 static을 선언하면 여러 스레드가 접근시 심각한 오류를 발생 시킬 수 있음
 
## 메모리 릭.

 + 이미 알고 있는 스태틱으로 선언된 리스트나 배열등에 값을 추가하면 ??? 릭 발생


# 클래스 정보 - 07


## reflection 

 + 이 패키지에 있는 클래스들은 JVM에 로딩되어 있는 클래스와 메서드 정보 읽어 올 수 있음.

## class 클래스

 + 생성자는 따로없고, 클래스에 대한 정보를 얻을 때 사용
 + Object 클래스에 있는 getClass() 메소드를 이용하는 것이 일반적

## Method 클래스

 + 메서드에 대한 정보를 얻을 수 있음
 + 생성자가 없음으로 Class getMethods()  / GetDeclaredMethod() 메서드를 써야함.

## Field 클래스

 + 클래스에 있는 변수들의 정보 제공을 위해 사용됨 .

## reflection 관련 클래스 잘못사용하면 ?

 + if (src.getClass().getName().equals("java.math.BigDecimal")) {
 + 미미하지만 성능의 영향을 주긴함 하나하나 모이면 .. instanceof 사용이 더빠를걸..


    public String checkClass(Object src) {
    if (src instanceof java.math.BigDecimal) {
    // 데이터 처리
    }


## 정리

 + 클래스의 메타정보는 JVM Perm 영역에 저장됨 . 





# synchronized는 제대로 알고 쓰자 - 08

## 스레드 

 + 프로세스와 스레드는 1:다  관계 
 + 프로세스에서 만들어 사용하고있는 메모리를 공유.


## Thread / Runnable



    class RunThreads {
        public static void main(String[] args) {
            Runnablelmpl ri = new Runnablelmpl ();
            Thread Extends Thread ThreadExtends();
            new Thread (ri).start();
            te.start();
        }
    }


## sleep(), wait(), join

 + 진행중인 스레드를 대기하기 위한 메서드.
 + 3가지 메서드 모두 예외를 던지도록 되어 있어 반드시 예외 처리 해줘야 됨. 



### sleep()

 + sleep(long millis) : 명시된 Ms 만큼 해당 스레드 대기. 정적 메서드임
 + sleep(long millis, int nanos) : 명시된 Ms + 나노시간만큼 해당 스레드 대기. 정적 메서드

### wait()

 + 모든 클래스의 부모 클래스인 Object 클래스에 선언되 있음 , 어떤 클래스도 사용 가능,
 + 명시된 만큼 스레드 대기.
 + sleep 과 다른점은 매개 변수 , 아무런 매개 변수를 지정하지않으면 notify/nofityAll 매서드가 호출될때 까지 대기함.

### join()

 + 메서드는 명시된 시간만큼 해당 스레드가 죽기를 기다림.
 + 아무런 매개변수 없으면 죽을때 까지 계속 대기.


## interrupt(), nofify(), notifyAll()
 
 

### interrupt()

 + 앞서 3개 메서드를 모두 멈출 수 있는 유일한 메서드는 interrupt()
 + 호출되면 중된 스레드는 InterruptedException 발생 
 + 수행확인은 interrupted() / isInterrupted() 메서드 
 + 전자는 스레드의 상태를 변경 , 후자는 상태만 리턴.
 + isAlive() 스레드 생존 여부 확인 . 


### notify() / notifyAll()

 + wait()를 멈추기 위한 메서드.
 + 전자는 객체의 모니터 관련 단일 스레드를 깨우고
 + 후자는 객체의 모니터와 관련있는 모든 스레드를 깨운다.


## interrupt() 메서드는 절대적인 것이 아님

 + Interrupt() 메서드 호출하여 특정 메서드 중지시키려할때 
 + 항상 멈추는 것은 아님.
 + interrupt() 메서드는 해당 스레드가 block 되거나 특정 상태에만 동작함.
 + 그래서 중간에 sleep 을넣어주거나 계속되는 스레드를 잠시 멈출때 작동함.



    public void run() {
                while (flag) {
                value++ j if (value == Integer.MAX_VALUE) {
                value = Integer.MIN_V ALUEj System.out.println("MAX_V ALUE reached! ! !") 
                    try {
                    Thread.sleep(0, 1) j
                    } catch (Exception e) {
                    break
                    }
            }
        }
    }


## synchronized 이해

 + 동사 : 동시에 일어나다 , 동시에 진행하다.
 + 하나의 객체에 여러 객체가 동시에 접근 처리하는 상황이 발생할때 사용함.
 + 천천히 한명씩들어 와! 메서드나 블록을 제어 .

### 언제 사용해 ?

 + 하나의 객체를 여러 스레드에서 동시 사용할 경우나
 + static으로 선언한 객체를 여러 스레드에서 동시에 사용할 경우에 사용해야됨 .



## 동기화 - 동일객체 접근 시

 + 필요한 부분에만 동기화를 사용해 성능을 줄이자


## 동기화 - static 사용시 

 + 클래스 변수에 값이 일정하지않음 -> 메서드도 클래스 메서드를 참조하도록 static 


    public class ContributionStatic {
    private static int amount = 0;
    
        public static synchronized void donate() {
        }
    
        amount++;
    
        public int getTotal() {
            return amount;
        }
    }



## 동기화를 위해 자바에서 제공하는 것.


### java.util.concurrent

 + LOCK : 실행 중인 스레드를 간단한 방법으로 정지시켰다 실행함, 상호 참조로 데드락 피함
 + Executors: 스레드를 더 효율적을 관리하는 클래스들 제공 / 스레드 풀도 제공
 + Concurrent 콜렉션 : 앞서 살펴본 콜렉션의 클래스들 제공
 + Atomic 변수: 동기화가 되어있는 변수 제공 , 사용시 synchronized 식별자를 메서드에 지정할 필요없이 사용



## JVM내 Synchronization 동작


 + 자바의 HtoSpot VM은 자바 모니터를 제공함으로서 스레드들이 상호 배제 프로토콜에 참여하게 돕는다.
 + 자바 모니터는 잠기거나 풀린 상태중 하나이며 , 동일한 모니터에 진입한 여러 스레드들 중 한 시점에는 단 하나의 스레드만 모니터 가짐
 + 정리 -> 모니터를 가진 스레드만 모니터에 의해 보호되는 영역에 들어가 작업
 + 보호되는 영역 -> synchronized  
 + jdk 5 부터 XX:+UseBiasedLocking 옵션을 통해 biased locking 기능 제공.
 + 그 전까지는 대부분의 객체들이 하나의 스레드에 의해 잠기게 됬지만 , 이 옵션은 스레드가 자기 자신을 향하여 bias 됨
 + 즉 이 상태가 되면 스레드는 많은 비용이 드는 인스트럭션 재배열 작업을 통해 잠김과 풀림 작업을 수행함. -> 진보된 적응 스피닝 기술을 사용해 처리량 개선한다 함.
 + 동기화 성능이 보다 빠라짐.



# IO 에서 발생하는 병목현상 - 10


 + 잘못 사용하면 응답속도에 영향을 주는 부분 .


## 기본적인 IO 처리


