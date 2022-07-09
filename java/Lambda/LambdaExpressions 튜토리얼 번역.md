# 람다 표현식


익명 클래스의 한 가지 문제는 하나의 메서드만 포함하는 인터페이스와 같이 익명 클래스의 구현이 매우 간단한 경우 익명 클래스의 구문이 다루기 어렵고 불분명해 보일 수 있다는 것입니다. 
이러한 경우 일반적으로 누군가가 버튼을 클릭할 때 취해야 하는 작업과 같은 기능을 다른 메서드에 대한 인수로 전달하려고 합니다. 
Lambda 표현식을 사용하면 기능을 메서드 인수로 처리하거나 코드를 데이터로 처리할 수 있습니다.

이전 섹션인 Anonymous Classes 는 기본 클래스에 이름을 지정하지 않고 구현하는 방법을 보여줍니다. 
이것은 종종 명명된 클래스보다 더 간결하지만 메서드가 하나뿐인 클래스의 경우 익명 클래스도 약간 과도하고 번거롭게 보입니다.
람다 표현식을 사용하면 단일 메서드 클래스의 인스턴스를 보다 간결하게 표현할 수 있습니다.

이 섹션에서는 다음 항목을 다룹니다.

Lambda 표현식의 이상적인 사용 사례


    접근 방식 1: 하나의 특성과 일치하는 멤버를 검색하는 메서드 만들기
    접근 방식 2: 보다 일반화된 검색 방법 만들기
    접근 방식 3: 로컬 클래스에서 검색 기준 코드 지정
    접근 방식 4: 익명 클래스에서 검색 기준 코드 지정
    접근 방식 5: Lambda 식으로 검색 기준 코드 지정
    접근 방식 6: 람다 식과 함께 표준 기능 인터페이스 사용
    접근 방식 7: 애플리케이션 전체에서 람다 식 사용
    접근 방식 8: 제네릭을 보다 광범위하게 사용
    접근 방식 9: 람다 식을 매개변수로 허용하는 집계 작업 사용
    GUI 애플리케이션의 람다 표현식
    람다 표현식의 구문
    엔클로징 스코프의 지역 변수 접근하기
    대상 입력
    대상 유형 및 메서드 인수
    직렬화


Lambda 표현식의 이상적인 사용 사례
소셜 네트워킹 응용 프로그램을 만들고 있다고 가정합니다. 관리자가 특정 기준을 충족하는 소셜 네트워킹 응용 프로그램의 구성원에 대해 메시지 보내기와 같은 모든 종류의 작업을 수행할 수 있도록 하는 기능을 만들려고 합니다. 다음 표에서는 이 사용 사례를 자세히 설명합니다.

필드	설명
이름	선택한 구성원에 대한 작업 수행
주 배우	관리자
전제조건	관리자가 시스템에 로그인되어 있습니다.
사후 조건	지정된 기준에 맞는 멤버에 대해서만 작업이 수행됩니다.
주요 성공 시나리오
관리자는 특정 작업을 수행할 구성원의 기준을 지정합니다.
관리자는 선택한 구성원에 대해 수행할 작업을 지정합니다.
관리자가 제출 버튼을 선택합니다.
시스템은 지정된 기준과 일치하는 모든 구성원을 찾습니다.
시스템은 일치하는 모든 구성원에 대해 지정된 작업을 수행합니다.
확장
1a. 관리자는 수행할 작업을 지정하기 전이나 제출 버튼 을 선택하기 전에 지정된 기준과 일치하는 구성원을 미리 볼 수 있는 옵션이 있습니다 .

발생 빈도	하루 동안 여러 번.
Person이 소셜 네트워킹 애플리케이션의 구성원이 다음 클래스 로 표시된다고 가정합니다 .

public class Person {

    public enum Sex {
        MALE, FEMALE
    }

    String name;
    LocalDate birthday;
    Sex gender;
    String emailAddress;

    public int getAge() {
        // ...
    }

    public void printPerson() {
        // ...
    }
}

소셜 네트워킹 애플리케이션의 구성원이 List<Person>인스턴스에 저장되어 있다고 가정합니다.

이 섹션은 이 사용 사례에 대한 순진한 접근 방식으로 시작합니다. 로컬 및 익명 클래스를 사용하여 이 접근 방식을 개선한 다음 람다 식을 사용하여 효율적이고 간결한 접근 방식으로 마무리합니다. 예제에서 이 섹션에 설명된 코드 발췌문을 찾으십시오 RosterTest.

접근 방식 1: 하나의 특성과 일치하는 멤버를 검색하는 메서드 만들기
한 가지 간단한 접근 방식은 여러 메서드를 만드는 것입니다. 각 방법은 성별 또는 연령과 같은 하나의 특성과 일치하는 구성원을 검색합니다. 다음 방법은 지정된 연령보다 오래된 구성원을 인쇄합니다.

    public static void printPersonsOlderThan(List<Person> roster, int age) {
        for (Person p : roster) {
            if (p.getAge() >= age) {
                p.printPerson();
            }
        }
    }
참고 : A List는 주문형 Collection입니다. 컬렉션 은 여러 요소를 단일 단위로 그룹화하는 개체입니다 . 
컬렉션은 집계 데이터를 저장, 검색, 조작 및 전달하는 데 사용됩니다. 컬렉션에 대한 자세한 내용은 컬렉션 추적을 참조하십시오.

이 접근 방식은 잠재적으로 애플리케이션 을 취약 하게 만들 수 있으며, 
이는 업데이트(예: 최신 데이터 유형) 도입으로 인해 애플리케이션이 작동하지 않을 가능성이 있습니다. 응용 프로그램을 업그레이드 Person하고 다른 멤버 변수를 포함하도록 클래스 구조를 변경한다고 가정합니다. 
아마도 클래스는 다른 데이터 유형이나 알고리즘으로 나이를 기록하고 측정합니다. 이 변경 사항을 수용하려면 많은 API를 다시 작성해야 합니다. 또한 이 접근 방식은 불필요하게 제한적입니다.
예를 들어 특정 연령보다 어린 회원을 인쇄하려면 어떻게 해야 합니까?

접근 방식 2: 보다 일반화된 검색 방법 만들기
다음 방법은 보다 일반적입니다 printPersonsOlderThan. 지정된 연령 범위 내의 구성원을 인쇄합니다.


    public static void printPersonsWithinAgeRange(
        List<Person> roster, int low, int high) {
            for (Person p : roster) {
                if (low <= p.getAge() && p.getAge() < high) {
                p.printPerson();
            }
        }
    }

지정된 성별의 구성원을 인쇄하거나 지정된 성별 및 연령 범위의 조합을 인쇄하려면 어떻게 합니까?
Person클래스를 변경하고 관계 상태 또는 지리적 위치와 같은 다른 속성을 추가 하기로 결정했다면 어떻게 하시겠습니까? 
이 방법은 보다 일반적이지만 printPersonsOlderThan가능한 각 검색 쿼리에 대해 별도의 방법을 만들려고 하면 여전히 코드가 깨지기 쉽습니다. 
대신 다른 클래스에서 검색하려는 기준을 지정하는 코드를 분리할 수 있습니다.

접근 방식 3: 로컬 클래스에서 검색 기준 코드 지정
다음 방법은 지정한 검색 기준과 일치하는 멤버를 인쇄합니다.

    public static void printPersons(
        List<Person> roster, CheckPerson tester) {
            for (Person p : roster) {
                if (tester.test(p)) {
                    p.printPerson();
                }
        }
    }
이 메서드는 메서드 를 호출하여 매개 변수 에 지정된 검색 기준을 충족하는지 여부를 매개 변수 에 포함된 각 Person인스턴스를 확인합니다 . 메서드 가 값을 반환 하면 메서드가 인스턴스 에서 호출됩니다 .

ListrosterCheckPersontestertester.testtester.testtrueprintPersonsPerson

검색 기준을 지정하려면 CheckPerson인터페이스를 구현합니다.

    interface CheckPerson {
        boolean test(Person p);
    }

다음 클래스 CheckPerson는 메서드에 대한 구현을 지정하여 인터페이스를 구현합니다 test. 
이 메서드는 미국에서 병역에 적합한 구성원을 필터링합니다. 매개변수가 남성이고 18세에서 25세 사이인 true경우 값 을 반환합니다.Person

    class CheckPersonEligibleForSelectiveService implements CheckPerson {
        public boolean test(Person p) {
            return p.gender == Person.Sex.MALE && p.getAge() >= 18 && p.getAge() <= 25;
        }
    }

이 클래스를 사용하려면 새 인스턴스를 만들고 printPersons메서드를 호출합니다.

printPersons(
roster, new CheckPersonEligibleForSelectiveService()
);

이 접근 방식이 덜 부서지기는 하지만(구조를 변경하면 메서드를 다시 작성할 필요가 없음) Person여전히 추가 코드가 있습니다.
즉, 애플리케이션에서 수행하려는 각 검색에 대한 새 인터페이스와 로컬 클래스가 있습니다. 
인터페이스를 구현 하기 때문에 CheckPersonEligibleForSelectiveService 로컬 클래스 대신 익명 클래스를 사용할 수 있으며 각 검색에 대해 새 클래스를 선언할 필요가 없습니다.

접근 방식 4: 익명 클래스에서 검색 기준 코드 지정
다음 메서드 호출의 인수 중 하나는 printPersons미국에서 병역 의무에 해당하는 18세에서 25세 사이의 남성을 필터링하는 익명 클래스입니다.

    printPersons(
        roster,
            new CheckPerson() {
                public boolean test(Person p) {
                return p.getGender() == Person.Sex.MALE
                && p.getAge() >= 18
                && p.getAge() <= 25;
            }
        }
    );

이 접근 방식은 수행하려는 각 검색에 대해 새 클래스를 만들 필요가 없기 때문에 필요한 코드의 양을 줄입니다.
CheckPerson
그러나 인터페이스에 메서드가 하나만 포함되어 있다는 점을 고려하면 익명 클래스의 구문은 부피가 큽니다 
. 이 경우 다음 섹션에 설명된 대로 익명 클래스 대신 람다 식을 사용할 수 있습니다.

접근 방식 5: Lambda 식으로 검색 기준 코드 지정
CheckPerson 인터페이스는 기능적 인터페이스 입니다. 기능적 인터페이스는 하나의 추상 메서드 만 포함하는 모든 인터페이스입니다 .
(기능 인터페이스에는 하나 이상의 기본 메서드 또는 정적 메서드 가 포함될 수 있습니다 .)
기능 인터페이스에는 추상 메서드가 하나만 포함되어 있으므로 구현할 때 해당 메서드의 이름을 생략할 수 있습니다. 
이렇게 하려면 익명 클래스 식을 사용하는 대신 다음 메서드 호출에서 강조 표시된 람다 식 을 사용합니다.

    printPersons(
        roster,
        (Person p) -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25
    );

람다 식 을 정의하는 방법에 대한 정보 는 람다 식의 구문을 참조하십시오 .

인터페이스 대신 표준 기능 인터페이스를 사용할 수 있으므로 CheckPerson필요한 코드의 양이 훨씬 더 줄어듭니다.

접근 방식 6: 람다 식과 함께 표준 기능 인터페이스 사용

Predicate<T> interface in place of CheckPerson. This interface contains the method boolean test(T t):

    interface CheckPerson {
        boolean test(Person p);
    }


이것은 매우 간단한 인터페이스입니다. 하나의 추상 메서드만 포함하기 때문에 기능적 인터페이스입니다.
이 메소드는 하나의 매개변수를 취하고 boolean값을 리턴합니다. 이 방법은 너무 간단하여 애플리케이션에서 정의할 가치가 없을 수도 있습니다.
결과적으로 JDK는 패키지에서 찾을 수 있는 몇 가지 표준 기능 인터페이스를 정의합니다 java.util.function.

Predicate<T> 예를 들어 . 대신 인터페이스를 사용할 수 있습니다 

CheckPerson. 이 인터페이스에는 다음 메서드가 포함되어 있습니다 boolean test(T t).

    interface Predicate<Person> {
        boolean test(Person t);
    }


인터페이스 Predicate<T>는 일반 인터페이스의 한 예입니다. 
(제네릭에 대한 자세한 내용은 제네릭(업데이트됨) 단원을 참조하십시오.) 
제네릭 유형(예: 제네릭 인터페이스)은 꺾쇠 괄호( ) 안에 하나 이상의 유형 매개변수를 지정 <>합니다.
이 인터페이스에는 하나의 유형 매개변수 만 포함됩니다 T. 
실제 형식 인수를 사용하여 제네릭 형식을 선언하거나 인스턴스화하면 매개 변수화된 형식이 있습니다. 
예를 들어 매개변수화된 유형 Predicate<Person>은 다음과 같습니다.

    interface Predicate<Person> {
        boolean test(Person t);
    }


이 매개변수화된 유형에는 와 동일한 반환 유형 및 매개변수가 있는 메소드가 포함되어 있습니다 CheckPerson.boolean test(Person p). 결과적으로 다음 방법이 설명하는 대로 Predicate<T>대신 사용할 수 있습니다 .CheckPerson

    public static void printPersonsWithPredicate(
        List<Person> roster, Predicate<Person> tester) {
            for (Person p : roster) {
            if (tester.test(p)) {
            p.printPerson();
            }
        }
    }

결과적으로 다음 메소드 호출은 접근법 3: 선택적 복무 자격이 있는 구성원을 확보하기 위해 로컬 클래스에 검색 기준 코드 지정에서 호출할 때와 printPersons동일 합니다 .

    printPersonsWithPredicate(
        roster,
        p -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25
    );

이것은 이 메서드에서 람다 식을 사용할 수 있는 유일한 장소가 아닙니다. 다음 접근 방식은 람다 식을 사용하는 다른 방법을 제안합니다.

접근 방식 7: 애플리케이션 전체에서 람다 식 사용
printPersonsWithPredicate 람다 식을 사용할 수 있는 다른 위치를 확인 하려면 메서드 를 다시 고려하세요.

    public static void printPersonsWithPredicate(
            List<Person> roster, Predicate<Person> tester) {
            for (Person p : roster) {
            if (tester.test(p)) {
            p.printPerson();
            }
        }
    }
이 메소드는 매개변수 Person에 포함된 각 인스턴스가 List매개변수 에 roster지정된 기준을 충족하는지 여부를 확인합니다 . 인스턴스가 에 지정된 기준을 충족하지 않으면 인스턴스 에서 메서드 가 호출됩니다 .PredicatetesterPersontesterprintPersonPerson

메서드를 호출하는 대신 에서 지정한 기준을 충족 printPerson하는 인스턴스에서 수행할 다른 작업을 지정할 수 있습니다 . 람다 식으로 이 작업을 지정할 수 있습니다. 하나의 인수(객체 유형 )를 취하고 void를 반환 하는 와 유사한 람다 식을 원한다고 가정합니다 . 람다 식을 사용하려면 함수형 인터페이스를 구현해야 합니다. 이 경우 하나의 형식 인수를 취하고 void를 반환할 수 있는 추상 메서드가 포함된 기능 인터페이스가 필요합니다. 인터페이스에는 이러한 특성이 있는 메서드가 포함되어 있습니다. 다음 메서드는 호출 을 메서드 를 호출 하는 인스턴스로 바꿉니다 .PersontesterprintPersonPersonPersonConsumer<T>void accept(T t)p.printPerson()Consumer<Person>accept

    public static void processPersons(
        List<Person> roster,
            Predicate<Person> tester,
            Consumer<Person> block) {
                for (Person p : roster) {
                 if (tester.test(p)) {
                block.accept(p);
            }
        }
    }
결과적으로 다음 메소드 호출은 접근법 3: 선택 복무 자격이 있는 구성원을 확보하기 위해 로컬 클래스에 검색 기준 코드 지정에서 호출할 때와 printPersons동일 합니다 . 멤버를 인쇄하는 데 사용되는 람다 표현식이 강조 표시됩니다.

    processPersons(
        roster,
        p -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25,
        p -> p.printPerson()
    );

회원 프로필을 인쇄하는 것보다 더 많은 작업을 수행하고 싶다면 어떻게 하시겠습니까? 
회원의 프로필을 확인하거나 연락처 정보를 검색한다고 가정해 보겠습니다. 이 경우 값을 반환하는 추상 메서드가 포함된 기능적 인터페이스가 필요합니다. 
Function<T,R> 인터페이스는 메소드를 포함 합니다 R apply(T t). 다음 메서드는 매개변수에 지정된 데이터를 검색한 다음 매개변수 에 지정된 데이터 mapper에 대해 작업을 수행합니다 block.

    public static void processPersonsWithFunction(
        List<Person> roster,
            Predicate<Person> tester,
            Function<Person, String> mapper,
            Consumer<String> block) {
            for (Person p : roster) {
            if (tester.test(p)) {
            String data = mapper.apply(p);
            block.accept(data);
            }
        }
    }

다음 방법 roster은 병역 대상자에 포함된 각 구성원의 이메일 주소를 검색한 후 인쇄합니다.

    processPersonsWithFunction(
        roster,
        p -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25,
        p -> p.getEmailAddress(),
        email -> System.out.println(email)
    );
접근 방식 8: 제네릭을 보다 광범위하게 사용
방법을 재고하십시오 processPersonsWithFunction. 다음은 모든 데이터 유형의 요소를 포함하는 컬렉션을 매개변수로 받아들이는 일반 버전입니다.

    public static <X, Y> void processElements(
        Iterable<X> source,
        Predicate<X> tester,
        Function <X, Y> mapper,
            Consumer<Y> block) {
            for (X p : source) {
            if (tester.test(p)) {
            Y data = mapper.apply(p);
            block.accept(data);
            }
        }
    }
Selective Service 자격이 있는 회원의 이메일 주소를 인쇄하려면 processElements다음과 같이 메소드를 호출하십시오.

    processElements(
    roster,
        p -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25,
        p -> p.getEmailAddress(),
        email -> System.out.println(email)
    );
이 메서드 호출은 다음 작업을 수행합니다.

컬렉션에서 개체의 소스를 가져옵니다 source. Person이 예에서는 컬렉션에서 개체 의 소스를 가져옵니다 roster. rostertype 의 컬렉션인 컬렉션도 type List의 객체 임을 주목하십시오 Iterable.
개체와 일치하는 Predicate개체 를 필터링합니다 tester. 이 예에서 Predicate개체는 선택적 서비스에 적합한 구성원을 지정하는 람다 식입니다.
Function필터링된 각 개체를 개체 에서 지정한 값에 매핑 합니다 mapper. 이 예에서 Function개체는 구성원의 전자 메일 주소를 반환하는 람다 식입니다.
개체에 지정된 대로 매핑된 각 개체에 대해 작업을 수행 Consumer합니다 block. 이 예에서 Consumer개체는 개체에서 반환된 전자 메일 주소인 문자열을 인쇄하는 람다 식입니다 Function.
이러한 각 작업을 집계 작업으로 바꿀 수 있습니다.

접근 방식 9: 람다 식을 매개변수로 허용하는 집계 작업 사용
roster다음 예에서는 집계 작업을 사용 하여 Selective Service 자격이 있는 컬렉션에 포함된 구성원의 전자 메일 주소를 인쇄합니다 .

    roster
    .stream()
    .filter(
    p -> p.getGender() == Person.Sex.MALE
    && p.getAge() >= 18
    && p.getAge() <= 25)
    .map(p -> p.getEmailAddress())
    .forEach(email -> System.out.println(email));

다음 표 processElements에서는 해당 집계 작업과 함께 메서드가 수행하는 각 작업을 매핑합니다.

processElements동작	집계 작업

개체의 소스 얻기	Stream<E> stream()
개체와 일치하는 Predicate개체 필터링	Stream<T> filter(Predicate<? super T> predicate)
개체에 지정된 대로 개체를 다른 값에 Function매핑	<R> Stream<R> map(Function<? super T,? extends R> mapper)
Consumer개체 에 지정된 대로 작업 수행	void forEach(Consumer<? super T> action)


연산 filter, map및 forEach는 집계 연산 입니다 . 
집계 작업은 컬렉션에서 직접 처리하지 않고 스트림에서 요소를 처리합니다(이 예에서 호출된 첫 번째 메서드가 인 이유입니다 stream). 
스트림 은 요소의 시퀀스입니다 . 컬렉션과 달리 요소를 저장하는 데이터 구조가 아닙니다. 대신 스트림은 파이프라인을 통해 컬렉션과 같은 소스의 값을 전달합니다. 
파이프라인 은 스트림 작업 의 시퀀스이며 이 예에서는 filter- map입니다 forEach. 또한 집계 작업은 일반적으로 람다 식을 매개 변수로 허용하므로 동작 방식을 사용자 지정할 수 있습니다.

집계 작업에 대한 더 자세한 설명은 집계 작업 단원을 참조하십시오.

GUI 애플리케이션의 람다 표현식
키보드 동작, 마우스 동작 및 스크롤 동작과 같은 GUI(그래픽 사용자 인터페이스) 응용 프로그램에서 이벤트를 처리하려면 일반적으로 특정 인터페이스를 구현하는 이벤트 처리기를 만듭니다. 종종 이벤트 핸들러 인터페이스는 기능적 인터페이스입니다. 그들은 한 가지 방법만 가지고 있는 경향이 있습니다.

JavaFX 예제 (이전 섹션 Anonymous Classes 에서 설명 )에서 강조 표시된 익명 클래스를 다음 명령문에서 람다 식으로 바꿀 수 있습니다. HelloWorld.java

        btn.setOnAction( 새로운 EventHandler<ActionEvent>() {

            @Override 
            public void handle(ActionEvent 이벤트) { 
                System.out.println("Hello World!"); 
            } 
        } );

메서드 호출 은 개체 btn.setOnAction가 나타내는 버튼을 선택할 때 발생하는 작업을 지정 합니다.
btn이 메서드에는 유형의 개체가 필요합니다 EventHandler<ActionEvent>. EventHandler<ActionEvent> 인터페이스에는 단 하나 의 메소드, void handle(T event). 이 인터페이스는 기능적 인터페이스이므로 다음 강조 표시된 람다 식을 사용하여 바꿀 수 있습니다.

        btn.setOnAction(
           이벤트 -> System.out.println("Hello World!") 
        );


람다 표현식의 구문


람다 식은 다음으로 구성됩니다.

괄호로 묶인 형식 매개변수의 쉼표로 구분된 목록입니다. 이 CheckPerson.test메서드에는 클래스 p의 인스턴스를 나타내는 매개 변수가 하나 있습니다.Person

참고 : 람다 식에서 매개변수의 데이터 유형을 생략할 수 있습니다. 또한 매개변수가 하나만 있는 경우 괄호를 생략할 수 있습니다. 예를 들어 다음 람다 표현식도 유효합니다.

    p -> p.getGender() == Person.Sex.MALE
    && p.getAge() >= 18
    && p.getAge() <= 25

단일 표현식 또는 명령문 블록으로 구성된 본문입니다. 이 예에서는 다음 표현식을 사용합니다.

    p.getGender() == Person.Sex.MALE
    && p.getAge() >= 18
    && p.getAge() <= 25

단일 표현식을 지정하면 Java 런타임이 표현식을 평가한 다음 해당 값을 반환합니다. 또는 return 문을 사용할 수 있습니다.

    p -> {
    return p.getGender() == Person.Sex.MALE
    && p.getAge() >= 18
    && p.getAge() <= 25;
    }

return 문은 표현식이 아닙니다. 람다 식에서는 문을 중괄호( )로 묶어야 합니다 {}. 그러나 void 메서드 호출을 중괄호로 묶을 필요는 없습니다. 예를 들어 다음은 유효한 람다 식입니다.

    email -> System.out.println(email)

람다 식은 메서드 선언과 매우 유사합니다. 람다 식을 이름이 없는 메서드인 익명 메서드로 간주할 수 있습니다.

다음 예 Calculator는 둘 이상의 형식 매개변수를 사용하는 람다 식의 예입니다.


    public class Calculator {
    
        interface IntegerMath {
            int operation(int a, int b);   
        }
      
        public int operateBinary(int a, int b, IntegerMath op) {
            return op.operation(a, b);
        }
     
        public static void main(String... args) {
        
            Calculator myApp = new Calculator();
            IntegerMath addition = (a, b) -> a + b;
            IntegerMath subtraction = (a, b) -> a - b;
            System.out.println("40 + 2 = " +
                myApp.operateBinary(40, 2, addition));
            System.out.println("20 - 10 = " +
                myApp.operateBinary(20, 10, subtraction));    
        }
    }

이 메서드 operateBinary는 두 개의 정수 피연산자에 대해 수학 연산을 수행합니다. 작업 자체는 의 인스턴스에 의해 지정됩니다 IntegerMath. 이 예에서는 람다 식 addition및 subtraction. 이 예에서는 다음을 인쇄합니다.

40 + 2 = 42
20 - 10 = 10
엔클로징 스코프의 지역 변수 접근하기
로컬 및 익명 클래스와 마찬가지로 람다 식은 변수를 캡처 할 수 있습니다 . 그들은 둘러싸는 범위의 지역 변수에 대해 동일한 액세스 권한을 갖습니다. 그러나 로컬 및 익명 클래스와 달리 람다 표현식에는 섀도잉 문제가 없습니다( 자세한 내용은 섀도잉 참조 ). 람다 표현식은 어휘 범위가 있습니다. 이는 상위 유형에서 이름을 상속하지 않거나 새로운 수준의 범위를 도입하지 않음을 의미합니다. 람다 식의 선언은 둘러싸는 환경에서와 같이 해석됩니다. 다음 예 LambdaScopeTest는 이를 보여줍니다.


    import java.util.function.Consumer;
    
    public class LambdaScopeTest {
    
        public int x = 0;
     
        class FirstLevel {
     
            public int x = 1;
            
            void methodInFirstLevel(int x) {
    
                int z = 2;
                 
                Consumer<Integer> myConsumer = (y) -> 
                {
                    // The following statement causes the compiler to generate
                    // the error "Local variable z defined in an enclosing scope
                    // must be final or effectively final" 
                    //
                    // z = 99;
                    
                    System.out.println("x = " + x); 
                    System.out.println("y = " + y);
                    System.out.println("z = " + z);
                    System.out.println("this.x = " + this.x);
                    System.out.println("LambdaScopeTest.this.x = " +
                        LambdaScopeTest.this.x);
                };
     
                myConsumer.accept(x);
     
            }
        }
     
        public static void main(String... args) {
            LambdaScopeTest st = new LambdaScopeTest();
            LambdaScopeTest.FirstLevel fl = st.new FirstLevel();
            fl.methodInFirstLevel(23);
        }
    }

이 예에서는 다음 출력을 생성합니다.

x = 23
y = 23
z = 2
this.x = 1
LambdaScopeTest.this.x = 0
람다 식 선언 x대신 매개변수를 대체하면 컴파일러에서 오류가 생성됩니다.ymyConsumer

    Consumer<Integer> myConsumer = (x) -> {
    // ...
    }
람다 식이 새로운 수준의 범위 지정을 도입하지 않기 때문에 컴파일러에서 "Lambda 식의 매개 변수 x는 바깥쪽 범위에 정의된 다른 지역 변수를 다시 선언할 수 없습니다"라는 오류를 생성합니다. 결과적으로 포함하는 범위의 필드, 메서드 및 지역 변수에 직접 액세스할 수 있습니다. 예를 들어 람다 식은 x메서드의 매개 변수에 직접 액세스합니다 methodInFirstLevel. 둘러싸는 클래스의 변수에 액세스하려면 키워드를 사용하십시오 this. 이 예에서 this.x는 멤버 변수 를 나타냅니다 FirstLevel.x.

그러나 로컬 및 익명 클래스와 마찬가지로 람다 식은 최종적이거나 사실상 최종적인 바깥쪽 블록의 로컬 변수 및 매개 변수에만 액세스할 수 있습니다. 이 예에서 변수 z는 사실상 최종 변수입니다. 초기화된 후에는 값이 변경되지 않습니다. 그러나 람다 식에 다음 할당 문을 추가한다고 가정합니다 myConsumer.

    Consumer<Integer> myConsumer = (y) -> {
    z = 99;
    // ...
    }

이 대입문 때문에 변수 z는 더 이상 사실상 최종적이지 않습니다. 결과적으로 Java 컴파일러는 "외부 범위에 정의된 로컬 변수 z는 최종적이거나 사실상 최종적이어야 합니다"와 유사한 오류 메시지를 생성합니다.

대상 입력

람다 식의 유형을 어떻게 결정합니까? 18세에서 25세 사이의 남성인 멤버를 선택하는 람다 식을 기억하십시오.

    p -> p.getGender() == Person.Sex.MALE
    && p.getAge() >= 18
    && p.getAge() <= 25

이 람다 표현식은 다음 두 가지 방법에 사용되었습니다.

public static void printPersons(List<Person> roster, CheckPerson tester)접근 방식 3: 로컬 클래스에서 검색 기준 코드 지정

public void printPersonsWithPredicate(List<Person> roster, Predicate<Person> tester)접근 방식 6: 람다 식과 함께 표준 기능 인터페이스 사용

Java 런타임이 메소드 printPersons를 호출할 때 의 데이터 유형을 예상 CheckPerson하므로 람다 표현식은 이 유형입니다. 그러나 Java 런타임이 메소드 printPersonsWithPredicate를 호출할 때 의 데이터 유형을 예상 Predicate<Person>하므로 람다 표현식은 이 유형입니다. 이러한 메소드가 예상하는 데이터 유형을 대상 유형 이라고 합니다 . 람다 표현식의 유형을 판별하기 위해 Java 컴파일러는 람다 표현식이 발견된 컨텍스트 또는 상황의 대상 유형을 사용합니다. 따라서 Java 컴파일러가 대상 유형을 결정할 수 있는 상황에서만 람다 표현식을 사용할 수 있습니다.

    변수 선언
    
    과제(Assignments)
    
    반환 문 (Return statements)
    
    배열 이니셜라이저 (Array initializers)
    
    메서드 또는 생성자 인수
    
    람다 표현식 본문
    
    조건식,?:
    
    캐스트 표현식 (Cast expressions)

대상 유형 및 메서드 인수
메소드 인수의 경우 Java 컴파일러는 두 가지 다른 언어 기능인 오버로드 확인 및 유형 인수 유추를 사용하여 대상 유형을 결정합니다.

다음 두 가지 기능 인터페이스( java.lang.Runnable및 java.util.concurrent.Callable<V>) 를 고려하십시오.

    public interface Runnable {
    void run();
    }
    
    public interface Callable<V> {
    V call();
    }

이 메서드 Runnable.run는 값을 반환하지 않지만 반환합니다 Callable<V>.call.

invoke다음과 같이 메서드를 오버로드했다고 가정합니다 (메서드 오버로드에 대한 자세한 내용은 메서드 정의 참조).

    void invoke(Runnable r) {
    r.run();
    }
    
    <T> T invoke(Callable<T> c) {
        return c.call();
    }
다음 문에서 어떤 메서드가 호출됩니까?

    String s = invoke(() -> "done");


해당 메서드 invoke(Callable<T>)가 값을 반환하기 때문에 메서드가 호출됩니다. 방법 invoke(Runnable)은 하지 않습니다. 이 경우 람다 식의 유형 () -> "done"은 입니다 Callable<T>.

직렬화
대상 유형과 캡처된 인수가 직렬화 가능한 경우 람다 식 을 직렬화 할 수 있습니다 . 그러나 내부 클래스 와 마찬가지로 람다 식의 직렬화는 권장되지 않습니다.