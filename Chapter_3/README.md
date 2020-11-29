# 👨🏻‍💻 람다 표현식

## 람다란?
결론부터 말하자면, 람다가 자바 8 이전의 자바로 할 수 없었던 일을 할 수 있게 해주는 것은 아니다. 람다는 쉽게말하자면 익명 클래스를 좀 더 간결한 코드로 작성한 것이다. 하지만 이 '간결함' 덕분에 코드가 유연해지기도 한다. 람다는 특히 **함수형 인터페이스**와 함께 사용할 때 힘을 발휘한다.  
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
제네릭 파라미터 T는 **참조형(Reference type)** 에만 사용할 수 있다. 즉, **기본형(Primitive type)에는 사용할 수 없다.** 다행히도 자바는 기본형을 참조형으로 변환하는 기능을 제공한다. 이를 **박싱**이라고 한다. 물론, 참조형을 기본형으로 변환하는 기능도 제공하고 이를 **언박싱**이라고 부른다. 또한, 자바는 박싱과 언박싱이 자동으로 이루어지도록 **오토박싱**을 제공한다. 그럼 도대체 무엇이 문제인가? 변환과정에 메모리가 사용된다는 것이 문제이다. 예를들어 아래와 같은 코드를 실행하면 int를 Integer에 담기위해 박싱이 진행된다.
```
List<Integer> list = new ArrayList<>();
for (int i = 300; i < 400; i++) {
  list.add(i);
}
```
이처럼 불필요한 박싱과정은 피하기 위해 자바는 기본형에 특화된 인터페이스를 제공한다. **IntPredicate, IntToDoubleFunction, LongSupplier** 등이 이에 해당된다. 
