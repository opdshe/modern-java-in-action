# 👨🏻‍💻 람다 표현식

## 람다란?
결론부터 말하자면, 람다가 자바 8 이전의 자바로 할 수 없었던 일을 할 수 있게 해주는 것은 아니다. 람다는 쉽게 말하자면 익명 클래스를 좀 더 간결한 코드로 작성한 것이다. 하지만 이 '간결함' 덕분에 코드가 유연해지기도 한다. 람다는 특히 **함수형 인터페이스**와 함께 사용할 때 힘을 발휘한다.  
<br>

## ✨ 함수형 인터페이스란?
함수형 인터페이스란 **정확히 하나의 추상 메서드를 지정하는 인터페이스이다.** 없어도 안 되고, 2개 이상도 안 된다. **오직 하나의 추상 메서드를 가져야 한다.**(이때, 디폴트 메서드의 수는 중요하지 않다.)
<br>  
**@FunctionalInterface란**  
@FunctionalInterface는 함수형 인터페이스임을 나타내는 어노테이션이다. 즉, @FunctionalInterface로 인터페이스를 선언했지만 실제로 함수형 인터페이스가 아니면(추상 메서드가 하나가 아니면) 컴파일 에러를 발생시킨다.  
<br>

## 함수형 인터페이스의 종류  
### Predicate
```
@FunctionalInterface
public interface Predicate<T> {
  boolean test(T t);
}
```
- **T 형식의 객체를 인수로 받아 boolean을 반환한다**
- **test**라는 추상 메서드를 정의
<br>

### Consumer
```
@FunctionalInterface
public interface Consumer<T> {
  void accept(T t);
}
```
- **T 형식의 객체를 인수로 받아 void를 반환한다**
- **accept**라는 추상 메서드를 정의
- T 형식의 객체를 인수로 받아 어떤 동작을 수행하고 싶을 때 사용
<br>

### Function
```
@FunctionalInterface
public interface Function<T, R> {
  R apply(T t);
}
```
- **T 형식의 객체를 인수로 받아 R형식의 객체를 반환한다**
- **apply**라는 추상 메서드를 정의
- 입력을 출력으로 매핑하고 싶을 때 사용
<br>


### 기본형 특화 인터페이스 제공
제네릭 파라미터 T는 **참조형(Reference type)** 에만 사용할 수 있다. 즉, **기본형(Primitive type)에는 사용할 수 없다.** 다행히도 자바는 기본형을 참조형으로 변환하는 기능을 제공한다. 이를 **박싱**이라고 한다. 물론, 참조형을 기본형으로 변환하는 기능도 제공하고 이를 **언박싱**이라고 부른다. 또한, 자바는 박싱과 언박싱이 자동으로 이루어지도록 **오토박싱**을 제공한다. 그럼 도대체 무엇이 문제인가? **변환과정에 메모리가 사용된다는 것이 문제이다.** 예를들어 아래와 같은 코드를 실행하면 int를 Integer에 담기위해 박싱이 진행된다.
```
List<Integer> list = new ArrayList<>();
for (int i = 300; i < 400; i++) {
  list.add(i);
}
```
이처럼 불필요한 박싱 과정을 피하기 위해 자바는 기본형에 특화된 인터페이스를 제공한다. **IntPredicate, IntToDoubleFunction, LongSupplier** 등이 이에 해당된다.  
<br>


## 지역 변수의 제약
람다에서는 **자유 변수**(파라미터로 넘겨진 변수가 아닌 외부에서 정의된 변수)에 접근이 가능하다. 단, 람다에서 사용될 지역 변수는 명시적으로 final이 선언되어 있어야 하거나, 실질적으로 **final로 선언된 변수와 똑같이 사용되어야 한다.** 즉, 재할당이 불가능하다. 예를 들어 아래와 같은 코드는 컴파일 할 수 없다.
```
int portNumber = 1337;
Runnable r = () -> System.out.println(portNumber);
portNumber = 31337;
```
<br>

**✨ 지역 변수의 제약이 필요한 이유**  

**인스턴스 변수는 힙**에 저장되는 반면, **지역 변수는 스택에 저장된다** 멀티 스레드 환경에서 실행할 때 각 스레드는 개별적 스택을 가진다. 이는 동기화 문제를 초래할 수 있어서 자바에서는 람다에서 자유 변수에 접근 시 원래 변수에 접근을 허용하는 것이 아니라 자유 지역 변수의 **복사본을 제공한다.** 따라서 복사본의 값이 바뀌지 않아야 하므로 지역 변수에는 한 번만 값을 할당해야 한다는 제약이 생긴 것이다.  
<br>


## 메서드 참조
메서드 참조를 이용하면 기존의 메서드 정의를 재활용해서 람다처럼 전달할 수 있다. 쉽게 말해 **메서드 참조는 람다의 축약형**이라고도 할 수 있다. 메서드 참조를 이용하면 조금 더 **가독성을 높일 수 있다.** 예를 들어, 
```(Apple apple) -> apple.getWeight()``` 보다는 ```Apple::getWeight```가 훨씬 눈에 잘 들어온다. 

### 생성자 참조
인수가 없는 생성자는 Supplier에 담을 수 있다 (void -> R 반환)  
``` Supplier<Apple> constructor = Apple::new;```  
인수가 있는 생성자는 Function에 담을 수 있다. (T -> R 반환)  
``` Function<Integer, Apple> constructor = Apple::new;```  
<br>

#### ✨생성자에 인수가 여러개라면?
BiFunction, TriFunction과 같은 인터페이스를 통해 구현이 가능하다
```
BiFunction<Color, Integer, Apple> constructor = Apple::new;
```

### 알아두면 좋은 구현법
```
static Map<String, Function<Integer, Fruit>> map = new HashMap<>();
static {
  map.put("apple", Apple::new);
  map.put("orange", Orange::new);
  //등등
}

public static Fruit giveMeFruit(String fruit, Integer weight){
  return map.get(fruit.toLowerCase())
            .apply(weight);
}
```
처음 봤을 땐 이해하기 어려웠지만, 한 번 이해하고 나니 굉장히 유용하게 사용할 수 있을 것 같다는 생각이 든다. (ex. 사용자의 입력에 해당하는 커맨드 매핑)
