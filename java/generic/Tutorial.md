# generic tutorial




## 왜 제네릭을 쓰지?

 1. 제네릭을 사용하면 클래스나, 인터페이스 등 메서드를 정의할때 클래스나 인터페이스가 매개 변수가 될 때가 있다.
 2. 이럴때 비슷한 타입 등 매개 변수 입력이 같은 코드를 재 사용할 수 있는 방법을 제공한다. 
 3. 컴파일 때 검사를 함으로 좀더 안전하고 런타임에서 오류가 일어나는 거보다 좀 더 쉽기 때문이다.


## 제네릭을 사용하지않을때 와 사용 할 때 예제 코드

    List list = new ArrayList();
    list.add("hello");
    String s = (String) list.get(0);


    List<String> list = new ArrayList<String>();
    list.add("hello");
    String s = list.get(0);   // no cast


 + 제네릭을 사용함으로서 따로 캐스팅 없이 String에 바로 적용 
 + 다양한 유형의 컬렉션에서 작동을하고 사용자가 따로 정의도 할 수 있으며
 + 안전하고 읽기가 쉬워 진다.


## 제네릭 유형 

    public class Box {
        private Object object;
    
        public void set(Object object) { this.object = object; }
        public Object get() { return object; }
    }


 + 기본 유형이 object로 선언을해서 원하는 것을 자유롭게 전달 될 수 있지만.
 + 컴파일시 어떤 타입이 어떻게 사용되는지는 알수 없다
 + Integer를 넣고 Integer를 빼내는 것이 정상적이지만 String으로 전달해 오류가 발생 이 가능하다.


## 제네릭 유형 호출 및 인스턴스화

     public class Box<T> {
     private T t;

     public void set(T t) { this.t = t; }
     public T get() { return t; }
     }


 + object를 제네릭 형식으로 T로 대체 했다. 모든 유형에 대해 가능 하다.
 + 이런 형식은 인터페이스 생성하는데 적용도 가능하다.

 + Box<Integer> integerBox; 
 + Integer 유형으로 Box 객체 생성 예시.


## 다이아몬드 

 + java SE 7 이상에서는 컴파일러가 컨텍스트에서 유형 인수를 결정이나 추론 할 수 있는
 + 클래스 생성자에 호출하는대 <> 를써서 이 안에 유형을 지정 할 수 있다.  + 
 + <> 비공식적으로 다이아몬드라 칭한다.


## 다중 파라미터 타입

    public interface Pair<K, V> {
    public K getKey();
    public V getValue();
    }
    
    public class OrderedPair<K, V> implements Pair<K, V> {

    private K key;
    private V value;

    public OrderedPair(K key, V value) {
	this.key = key;
	this.value = value;
    }

    public K getKey()	{ return key; }
    public V getValue() { return value; }
    }


 + Pair<String, Integer> p1 = new OrderedPair<String, Integer>("Even", 8);
 + Pair<String, String>  p2 = new OrderedPair<String, String>("hello", "world");
 + OrderedPair<String, Integer> p1 = new OrderedPair<>("Even", 8);
 + OrderedPair<String, String>  p2 = new OrderedPair<>("hello", "world");
 + 이런식으로 정의가 가능하다.  제네릭 인터페이스 생성시 제네릭 클래스 생성때와 동일한 규칙을 따름 

출처 : https://docs.oracle.com/javase/tutorial/java/generics/why.html


