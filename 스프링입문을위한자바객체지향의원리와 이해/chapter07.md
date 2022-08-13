# chapter07 - 스프링 삼각형과 설정 정보

## 정리 && 요약

### Ioc/DI 제어의 역전 의존성 주입

+ 전체가 부분을 의존한다 / 집합관계 와 , 구성관계
+ A가 B를 사용한다 - > A가 B를 의존한다.
+ 의존성 주입은 , 생성자, 속성 , xml , autowird, resource가 있음
+ autowird 는 타입우선 Resource는 Id 우선  -> Reousrce는 자바 표준 어노테이션 프레임워크 교체시 유용


### AOP

 + 중복적인코드 횡단관심라들을 처리하기 위한 기술
 + Target:  부가기능 적용할 대상
 + Advice : 어떤 부가기능을 선택할지
 + JoinPoint : SpringAOP에선 메서드 실행 시점
 + PointCut: 실제 Advice 실행 시점.


### AOP



![](Img/img_39.png)


 + 스프링 프레임워크를 학습하는 것에 대해.
 + Ioc/DI , AOP , PSA 스프링의 3대 프로그래밍 모델 이해가 필 수 이다.
 



## Ioc/DI 제어의 역전 의존성 주입


 + 전체가 부분에 의존한다고 표현함.
 + 더 깊이는 의존하는 객체와 의존되는 객체 사이에
 + 집합관계(Aggregation) 와 구성 관계 (Composition)로 구분 함
 + 전체가 부분에 의존한다! 프로그래밍에 의존관계는 new로 표현된다 기억!
 + A가 B를 사용한다 - > A가 B를 의존한다.


![](Img/img_40.png)

![](Img/img_41.png)


 + 자바 코드로만 표현한 의존성 주입.
 + 주입이란? 외부에서 주입을 한다. 


![](Img/img_42.png)

 + 생성자를 통해서 주입을 하는 예제 코드
 + 어떤 Tire 구현체가 들어와도 실행되고, 확장성이 좋아짐.
 + 지금까지 의존성 주입은 전략패턴 응용 
 + 전략 : 구현체 : 컨텍스트 : tire 브랜드 사용하는 매서드 등..



 + 의존성 주입 종류 : 생성자주입, 속성 주입(setter) , xml(xml 파일 만 교체해주면되서 편리.) , autowird


## Autowird

 + 타입 기준 매칭
 + id 보다 타입을 우선시 한다.


## Resource

 + 자바 표준 어노테이션
 + 오토와이어드와 달리 id가 우선순위다.


## AOP - Aspect? 관점? 핵심 관심사? 횡단 괌신사?


 + Aspect-Oriendted Programming 관점 지향 프로그래밍
 + 로직(Code) 주입 

![](Img/img_43.png)


 + 횡단 관심사.
 + 코드  = 핵심관심사 = 횡단 관심사


![](Img/img_44.png)

 + AOP 적용 예제 코드
 + @Aspect  클래스를 AOP에 사용하겠다 -> 공통관심사를 처리하겠다.
 + @Before 대상 메서드 실행 전에 이 메서드를 실행함 .
 + JointPoint @Before에서 선언된 메서드인 RunSomthing() 의미 



 + AOP는 인터페이스 기반.
 + AOP는 프록시를 사용해 실제 메서드를 호출한다.


### 요약

 + 스프링 AOP는 인터페이스 프록시 런타임 기반이다.

### 용어

![](Img/img_45.png)

### Pointcut 자르는 지점 Aspect 적용 위치 지정자 .

![](Img/img_47.png)

 + *runSomething -> pointcut
 + @Before("execution (* runSomething())) ? -> 지금 선언하고있는 public void before) 기울어진 볼드체 부분의
 + 메서드 (* runSomething()가 실행 되기전 @Before에 실행하라는 의미다. 여기서 public void before는 횡단 관심사를 실행하는 메서드
 + 결국 Pointcut은 -> 횡단 관심사를 적용할 타깃 메서드를 선택하는 지시자(메서드 선택필터)
 + 또는 줄여서 타깃 클래스의 타깃 메서드 지정자라고 함.

![](Img/img_48.png)


 + PointCut 예제




### joinPoint - 연결점? 연결 가능한지점.

![](Img/img_46.png)

 + Pointcut은 JoinPoint 부분 집합 
 + 스프링 AOP는 인터페이스를 기반 -> 인터페이스는 추상 메서드의 집합체 -> 스프링 AOP는 메서드에만 적용 가능하다는 결론.

 + Pointcut의 후보가 되는 모든 메서드들이 JointPoint 즉 Aspect 적용이 가능한 지점이 됨.
 + JointPoint란 Aspect 적용이 가능한 모든 지점을 말함.
 + Spring AOP에서 JoinPoint란 스프링 프레임워크가 관리하는 빈의 모든 메서드에 해당함.

![](Img/img_49.png)


 + romeo.runSomething() 메서드를 호출한 상태 -> JoinPoint = romeo 객체의 runSomething()메서드가 됨
 + JoinPoint 파라미터를 이용하면 실행 시점ㅁ에 실제 호출된 메서드가 무엇인지 실제 호출된 메서드를 소유한 객체가 무엇인지, 호출된 파라미터 무엇인지 정보확인가능.

### 정리

 + 광의의 JoinPoint란 Aspect 적용이 가능한 모든 지점이다.
 + 협의의 JoinPoint란 호출된 객체의 메서드다.

## Advice

 + Pointcut에 적용할 로직, 메서드 의미함. 여기서 + 언제라는 개념 포함.
 + 결론: Pointcut에 언제 무엇을 적용할지 정의한 메서드.


![](Img/img_50.png)


## Aspect- 관점? 측면? Advisor의 집합체

 + 여러 개의 Advice와 여러 개의 Pointcut의 결합체를 의미하는 용어
 + Aspect = Advice들 + Pointcut들
 + Advice는 언제 무엇을 의미
 + Pointcut은 어디에 를 의미
 + Aspect : 언제 어디에 무엇을 의미.

Advisor- 조언자? 어디서 , 언제 무엇을!

 + Advisor = 한 개의 Advice + 한 개의 Pointcut
 + 스프링 AOP에만 사용하는 용어



## AOP 추가 

### AOP는 ?

 + 객체 지향 프로그래밍 보완.
 + 모듈화의 단위는 관점 (OOP는 클래스)
 + IoC컨테이너가 AOP에 의존하지 않는 반면 AOP는 Spring Ioc를 보완하여 매우 유능한 미들웨어 솔루션 제공


### 컨셉

 + Aspect : 여러 클래스에 흩어진 관심사의 모듈화 @Aspect 어노테이션으로 클래스에 선언함.
 + Join Point : 메소드 실행이나 예외 처리와 같은 프로그램 실행중 포인트. AOP에선 항상 메소드 실행을 나타냄
 + Pointcut: 조인 포인트랑 일치하는 술어, Advice는 Pointcut표현식과 연관됨. 
 + Pointcut과 일치?하는 모든 조인 포인트에서 실행 (특정 이름의 메소드 실행), Pointcut표현식에 의해 매칭되는 조인포틴트 개념은 AOP 핵심


### Spring AOP Capabilities and Goals

 + Spring AOP는 순수 자바로 구현되어서 특별한 컴파일 과정이 필요없음
 + 클래스 로더 계층구조를 제어 할 필요가 없어 서블릿 컨테이너 또는 애플리케이션 서버에 사용하기 적합.
 + 현재 메소드 실행 조인 포인트만 지원합니다.


## 우아한 테코톡 정리


 + service에 스탑와치 기능이 들어가는게 맞나?
 + 부가기능이 들어 간건아닐까?



### 부가기능

 + 서비스는 비즈니스 로직 핵심기능만.
 + 그 의외에 는 시간 측정 트랜잭션등은 인프라 로직


### 인프라 로직
 
 + 애플리케이션의 전 영역에 나타나 중복 코드 가능성 존재 (유지보수 힘듬)
 + 비즈니스 로직과 섞여있으면 비즈니스 로직 이해하기 어려움..

![](Img/img_61.png)

 + 로깅 트랜 잭션 , 권한검사 ,성능 검사등 하나의 관심사를 가지게 되는 인프라로직,
 + 비즈니스로직을 수행하는데있어 부가기능인 인프라로직의 중복이이 횡단으로 나타내기 때문에 횡단관심사(cross-cutting concern)라 불림


### AOP

 + 관점 지향 프로그래밍 -> 횡단 관심에 따라 프로그래밍 한다.
 + 자바에서 구현체는 : AspectJ


### AOP 용어 ( 부가기능이란? 횡단의 관심을 가짐 )

 + Target : 어떤 대상에 부가 기능을 부여함?
 + Advice :  어떤 부가 기능? Before ,AfterReturning, AfterThrowing 어떠한 기능을 부가적으로 좀더 부여할것인가 기능의 관점
 + Join Point : 어디에 적용할 것인가? 메서드 , 필드 , 객체 , 생성자등 
 + -> AspectJ 자바구현체에서는 메서드가 호출이나 필드가 생성될때등 다양하게 조인 포인트 구현 여러가지 상황에서 부가기능 수행
 + -> Spring Aop에선 메서드가 실행 될때만.
 + Point Cut : 실제 advice가 적용될 지점, Spring Aop 에선 advice가 적용될 메서드를 선정 
 + Join Point가 메서드 실행 시점이기 때문에 Advice가 적용될 메서드를 선정하는 기능을 함.


### 코드

![](Img/img_62.png)

 + 메소드가 실행전 후가 필요해서 @Around : advice 부분
 + Executuon(*org~!~) : PointCut 어떠한 패키지구조에서 어떠한 메소드에 적용할 것이다.


![](Img/img_63.png)


 + 어노테이션기반 포인트컷 지정 가능.
 + PerformanceCheck 커스텀 어노테이션을 적용한 메서드는 해당 AOP 적용 됨.

### 번외

 + 자기가 자신의 메소드를 호출할 때
 + 타깃오브젝트가 타겟오브젝트에있는 메서드를 실행할때 AOP가 실행 안됌
 + 왜? 자기자기의 메서드를 실행할 때  타겟오브젝트에서 실행한 메서드 내부에서 실행한 메서드기때문에
 + 이때는 AOP가 적용안됀 그냥 타깃오브젝트의 메서드를 실행하기때문에
### 구현 방법

 + 컴파일  J.java -> J.class 컴파일 시점에 Aspect 적용
 + 클래스 로드시 J.class -> 메모리상에 올릴 때
 + 프록시패턴 : 스프링 AOP에서 사용함 
 + J라는 타겟을 부가기능을 제공하는 프록시로 감싸서 실행하는 방식 

## PSA- 일관성 있는 서비스 추상화 

 + 일관성 있는 서비스 추상화 
 + 서비스 추상화 ex) JDBC
 + 데이터 베이스 종류 관계없이 같은 방식제어할수있는 이유 - 어댑터 패턴.
 + 어댑터 패턴을 적용해 같은 일을 하는 다수의 기술을 공통의 인터페이스로 제어할 수 있게하는 것을 서비스 추상화라 함
 + 스프링 프레임워크는 서비스 추상화를 위해 다양한 어댑터 제공
 + OXM(Object XML Mapping:객체와 XML 매핑) / ORM / 캐시 / 트랜잭션


출처 

+ https://velog.io/@orijoon98/Spring-Framework-Documentation3-Aspect-Oriented-Programming
+ https://www.youtube.com/watch?v=Hm0w_9ngDpM