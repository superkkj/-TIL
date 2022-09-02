# 오브젝트와 의존관계


## 관심사 분리

+ 객체지향에서 변한다 ? 오브젝트에 대한 설계와 구현 코드가 변한다는 뜻.
+ 개발자는 미래를 준비하는데 있어 변화의 폭을 가장 줄여주는게 좋다.


## 분리

+ 프로그래밍의 기초 개념인 관심사 분리.
+ 관심사가 같은 것끼리 모으고 다른 것으로 분리 해주자.
+ 중복된 관심사를 메서드로 빼내서 , 메서드만 수정하면 나머지는 모드반영


## 상속을 통한 확장

+ 추상클래스를 만들고 구현부를 분리하자 NUserDao,DUserDao
+ 변경이 용이해지고 확장이 좋아진다.

+ 템플릿 메소드 패턴 - 서브클래스에서 메소드를 필요에 맞게 구현
+ 팩토리 메소드 패턴 - 서브클래스에서 구체적인 오브젝트 생성방법 결정  (Ex: getConnection())



## 인터페이스 도입

 + 상속을 통한 확장은 단점을 해결하기위한 방법
 + 두 개의 클래스를 긴밀하게 연결하지말고 중간의 추상적인 연결 고리를 만들어 둔다.
 + 그러나 인터페이스와 구현 클래스 사이의 관계 설정해주는 관심은 아직설정해주지 않음.
 + 여기서는 ! 클라이언트 오브젝트가 제 3의 관심사항인 관계설정 결정해주는 기능을 분리해두기 적절함.


## 관계를 어떻게?

 + 오브젝트 사이는 런타임시 사용관계 또는 의존관계를 맺어주면 됨.
 + 이제 책임을 클라이언트 오브젝트에게 넘기자 . 그러면 
 + 서비스 오브젝트를 사용할때 사용할 오브젝트를 생성자 방식으로 넘겨서 구현할 수 있따
 + 개방폐쇄의 원칙을 생각해보

## 개방 폐쇄 원칙

 + 지금까지 해온 작업들. 
 + 개방 폐쇄원칙으로 리팩토링된 코드는 전략 패턴에도 해당 된다.
 + 변경이 필요한 부분 인터페이스로 분리 / 구현한 알고리즘 클래스를 필요에 따라 바꿈

 + 이렇게 스프링은 객체지향적 설계원칙과 디자인 패턴에 나타난 장점을 자연스럽게 개발자들이 활용하게 해주는 프레임 워크.

# 제어의 역전 

## 팩토리

 + 생성 방법을 결정하고 만들어진 오브젝트 리턴하는 것.
 + 오브젝트를 생성하는 쪽과 생성된 오브젝트를 사용하는 쪽의 역할과 책임을 분리하는 목적



    public class DaoFactory {
        public UserDao userDao() {
         ConnectionMaker connectionMaker =new DConnectionMaker(); 
         UserDao userDao =new UserDao(connectionMaker);
         return userDao;
        }
    }
 + 컴포넌트의 구조 관계 정의한 설계도 같은 역할
 + 애플리케이션의 오브젝트들을 구성하고 그 관계를 정의하는 책임을 맡고 있다.


## 재어권의 이전을 통한 제어관계 역전

 + 제어의 흐름을 꺼꾸로 뒤집음.
 + 제어의역전 오브젝트는 자신이 사용할 오브젝트를 스스로 선택하지 않음 생성도하지 않음.
 + 모든 제어 권한을 다른 대상에게 위임하기 때문.
 + Ex) 서블릿에 대한 제어의 권한은 서블릿 컨테이너가 적절한 시점에 서블릿 클래스의 오브젝트를 만들고 그안에 메소드 호출.
 + 템플릿 메서드 패턴도 제어의 역전 개념. 서브클래스에서 구현한 메소드는 누가 언제 사용하는지 모름. 서브클래스에서 결정되는것이 아님.
 + 제어권은 상위 슈퍼클래스에 위임.. 필요할때 호출되서 사용

 + 위에 코드도 제어의 역전.
 + DaoFactory :  어떤 구현 클래스를 만들고 사용할지 등 ..
 + 수동적인 클래스들의 제어를 다른곳에 맡긴다..

## 결론

 + 자연스럽게 관심을 분리하고 책임을 나누고 유연하게 확장 가능한 구조로 만들기 위해
 + DaoFactory를 토입했던 과정이 제어의 역전을 적용하는 작업이었다.

# 스프링 IOC

 + 빈: 스프링이 제어권을 가지고 직접만들고 관계를 부여하는 오브젝트. @Bean : 오브젝트 생성을 담당하는 Ioc용 메서드 표시 
 + 애플리케이션 컨텍스트 : 별도로 설정정보를 담고 있는 무언 가를 가져와 활용하는 범용적인 IoC 엔진 -> 우리가작성한 다오팩토리를 활용해보자 


## 애플리케이션 컨텍스트 ( 스프링을 사용했을 때 장점)

 + 클라이언트는 구체적인 팩토리 클래스 알필요 없음.
 + 설정 정보(Factory)를 알필요가 없이 필요한 오브젝트를 가져 올 수 있음.
 + 종합 Ioc 서비스 제공.
 + 오브젝트가 만들어지는 방식 . 시점 전략 다르게 OR 자동생성 오브젝트 후처리 정보 조합
 + 인터셉트등 다영한 차원에서 기능 제공

 + 애플리케이션 컨텍스트는 빈을 검색하는 다양한 방법 제공



# 싱글톤 레지스트리

 + 애플리케이션 컨텍스트는 오브젝트 팩토리와 유사 허나 싱글톤으로 관리하는 싱글톤 레지스트리임

## 싱글톤 이유

 + 서버환경 이기 때문.
 + 서블릿은 대부분 멀티스레드 환경에서 싱글톤 동작 - > 서블릿 클래스당 하나의 오브젝트 -> 여러 스레드에서 공유


## 싱글톤 패턴 구현 한계

 + private 생성자를 Private 제한함 -> 상속 불가능
 + 테스트 하기 힘듬 / 서버환경에선 여러 서버가 있으므로 싱글톤이 하나만 되는걸 보장하지 않음
 + 전역 상태이기때문에 아무 객체나 자유롭게 접근하고 수정 공유함.


## 싱글톤 레지스트리

 + 자바의 기본적인 싱글톤 패턴 구현은 여러가지 단점이 있다.
 + 스프링이 직접 싱글톤 오브젝트를 만들고 관리 기능 제공 하는 것 (싱글톤 레지스트리)
 + 스프링 컨테이너는 싱글톤 관리 컨테이너이기도 함. -> 평범한 자바를 싱글톤으로 관리 (Public 생성자 사용가능)

## 싱글톤 주의사항

 + 무상태방식으로 -> Private 등 상태를 변경못하게 


# 재어의 역전 의존관계 주입

 + IOC는 폭넓게 쓰는의미라 스프링 Ioc하면 스프링만의 특별한 Ioc라는 의미가 의미가 될수도잇고 애매함
 + 그래서 의존 관계 주입이라는 이름을 사용 -> DI 컨테이너 라고 불리기도 함 
 + 인터페이스를 활용핸 의존관계를 설정해두면 구현체와 관계가 느슨해 지기때문에 변화를 덜 받게 된다.
 + 의존관계주입 : 런타임시 구현체와 클라이언트를 런타임시 연결해주는 작업.
 + 핵심은 설계 시점에 알지 못햇던 두 오브젝트를 관꼐를 맺도록 도와주는 제 3의존재가 있는 것

## 의존관계 검색

 + 굳이 스프링 빈이 아니어도 적용가능.
 + ex) this.connectionMaker = userDaoFactory.getConnectionMaker()

## 장점 

 + 객체지향 설계 장점을 그대로.



## 의존관계 주입

 + 수정자 메소드 주입 -> 내부의 메소드에서 사용하게하는 DI 방식에 활용하기 적당하다.