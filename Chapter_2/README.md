# 👨🏻‍💻  동작 파라미터화 코드 전달하기
요구사항은 항상 바뀌기 마련이다. 오늘은 녹색사과를 필터링 해 보여달라 했지만, 내일은 무게가 150g 이상인 사과들만 필터링해 보여달라고 할 수도 있다. 이처럼 시시각각 변하는 **요구사항에 쉽고 빠르게 대처하기 위해서 필요한 것**이 바로 **동작 파라미터화**이다.

### 동작 파라미터화
**동작 파라미터화**란, **아직은 어떻게 실행할 것인지 결정하지 않은 코드블록을 말한다.** 즉, 어떻게 실행할 지는 컴파일 타임이 아닌 **런타임**에 결정하도록 미뤄두는 것이다. 예를 들어, 메서드의 인수로 코드블록을 전달할 수가 있다. 결과적으로 코드블록에 따라 **메서드의 동작이 파라미터화된다.**   
<br>

## 사과 필터링하기
녹색 사과를 필터링 필터링 하는 코드를 작성해달라고 요청한다면 가장 쉬운 방법은 아래와 같을 것이다. 
```
public static List<Apple> filterGreenApples(List<Apple> inventory){
  List<Apple> result = new ArrayList<>();
  for (Apple apple : inventory ) {
    if (GREEN.equals(apple.getColor()) {
      result.add(apple);
    }
  }
  return result;
}
```
녹색 사과를 필터링하기에는 나쁘지 않은 코드이다. 하지만 이러한 코드는 매 번 달라지는 요청에 대응할 수 없다. 빨간 사과를 필터링해달라고 한다면? 함수를 하나 더 만들어야 하는 번거로움이 발생한다. 만약 색을 파라미터화 한다면 조금 더 유연하게 대처할 수 있을 듯 하다.

### 색을 파라미터화
```
public static List<Apple> filterApplesByColor(List<Apple> inventory, Color color){
  List<Apple> result = new ArrayList<>();
  for (Apple apple : inventory ) {
    if (apple.getColor().equals(color)) {
      result.add(apple);
    }
  }
  return result;
}
```
초기의 코드보다 조금 더 유연해 졌다. 위 함수 하나로, 빨간 사과, 녹색 사과, 검정색 사과 등등 모두 필터링 할 수 있게 되었다. 하지만 문제가 생겼다. 무게가 150g이상인 사과를 요청한다면 ??
A등급 이상의 사과를 요청한다면 ? 이처럼 변화하는 요구 사항들에 대해 유연하게 대처할 수 있도록 고안된 방법이 바로 **동작 파라미터화**이다. 

### 동작 파라미터화
```
public interface ApplePredicate {
  boolean test (Apple apple);
}
```
위와 같은 인터페이스를 작성하고, 위의 인터페이스를 구현하는 클래스를 생성한다면?
```
public static List<Apple> filterApples(List<Apple> inventory, ApplePredicate p){
  List<Apple> result = new ArrayList<>();
  for (Apple apple : inventory ) {
    if (p.test(apple)) {
      result.add(apple);
    }
  }
  return result;
}
```
자 이제 우리는 만능 filter메서드를 갖게 되었다. 이제 어떤 기준으로 필터링하는 지는 몰라도 된다. 요청이 들어올 때마다, 요청에 해당하는 ApplePredicate만 새로 작성하여 인자로 넣어주기만 하면 된다. 물론, 지금 상태로도 충분히 잘 구현한 메서드이다. 하지만 굳이 조금 더 욕심내자면, 매 번 요청이 바뀔때마다 ApplePredicate를 작성하는 것이 조금 귀찮다. 그런 우리를 구원해줄 방법이 바로 **람다**이다. 람다를 사용하면 아래와 같이 간결한 코드로 사과를 필터링 할 수 있다.
```
List<Apple> redApples = filter(intventory, (Apple apple) -> RED.equals.(apple.getColor()));
```
람다에 대해서는 3장에서 더 공부하도록 하자.
