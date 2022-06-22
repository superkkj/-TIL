# f-lab 2주차


느슨했던 긴장감을 다시..불어넣기위해 하루하루 커밋하기 시작!



# final vs Immutable (final 클래스,변수,함수 등에 선언하는  키워드고  , Immutable은 패턴? 상태이다(변경불가능한))
## final 

 1. 메서드 및 변수에 사용되는 수정자. final 키워드 사용시 본질적으로 상수가 됨으로 수정이 안됀다.
 2. 변수의 참조주소는 변경될 수 없다
 3. 하지만 상태는 변경 할 수 있다 (setter)

![img.png](img.png)




## immutable
 1. 객체의 실제 값을 변경할 수 없지만, 참조주소는 변경 가능.  
 + 풀어서 얘기하면 heap 영역에서 그 객체가 가리키고있는 데이터 자체는 변화 불가 그러나 stack에 있는 주소값을 다른 주소값으로 가르키도록 변경하는건 가능하다 .


![img_35.png](img_35.png)

int , bollean) 원시타입은 Stack 영역에 그대로 올라가지만, 참조 변수를 가지고 있는 타입 들은 heap영역에 저장하고 그 주소값을 Stack영역에 가지고 있음


 2. 대표적을 String , Integer , Boolean

![img_2.png](img_2.png)
![img_3.png](img_3.png)





++ 지식 String 자료를 찾아보면서.. 

![img_1.png](img_1.png)

String + 를 하면 새로운 주소로 할당됨 .
그리고 String은 참조 주소값이기 때문에 == 보단 equlas로 비교하는게 맞다.

결론 : String 을 예시로 들면 값이 변경되는 것 처럼보이지만 참조 주소 변경이 된 것 
 
 장점 : 생성자 ,접근 메소드에 방어적 복사가 필요 없음 , 멀티 스레드 환경이라면 동기화 처리 없이 객체 공유, 객체 신뢰
 단점 : 객체가 가지는 값마다 새로운 인스턴스 생성 

*출처 https://aljjabaegi.tistory.com/465

