---
title: "Modern_java_in_action_part_2"
date: 2021-06-22T23:31:46+09:00
draft: true
tags: ["java"]
---

# 동적 파라미터화 코드 전달하기

동적 파라미터화(Behavior parameterization)을 이용하면 자주 바뀌는 요구사항에 효과적으로 대응할 수 있다. 동적 파라미터란 아직은 어떻게 실핼할 것인지 결정하지 않은 코드 블록을 의미한다. 이 코드 블록은 나중에 프로그램에서 호출한다. 즉, 코드 블록의 실행은 나중으로 미뤄진다. 결과적으로 코드 블록에 따라 메서드의 동작이 파라미터화된다.

## 변화하는 요구사항에 대응하기

여기에서는 예제를 바탕으로 유연한 코드를 만드는 과정을 보여준다. 해당 예제들을 통해 유연한 코드를 어떻게 작성하였는지 직관적으로 확인할 수 있다.

1. 첫 번째 시도: 녹색 사과 필터링

   ```java
   public static List<Apple> filterGreenApples(List<Apple> inventor) {
       for(Apple apple: inventory){
           if(GREEN.equals(apple.getColor())){
               result.add(apple);
           }
       }
       return result;
   }
   ```

2. 두 번째 시도: 색을 파라미터화

   ```java
   public static List<Apple> filterGreenApples(List<Apple> inventor, Color color) {
       for(Apple apple: inventory){
           if(apple.getColor().equals(color)){
               result.add(apple);
           }
       }
       return result;
   }
   ```

   이와같은 코드를 통해 다음 처럼 구현한 메서드를 호출할 수 있다.

   ```java
   List<Apple> greenApples = filterApplesByColor(inventory, GREEN);
   List<Apple> redApples = filterApplesByColor(inventory, RED);
   ```

   여기에서 다양한 무게에 대응할 수 있도록 무게 정보 파라미터도 추가하면 다음과 같은 코드가 된다.

   ```java
   public static List<Apple> filterApplesByWeight(List<Apple> inventory, int weight){
       List<Apple> result = new ArrayList<>();
       for(Apple apple:inventoty){
           if(apple.getWeight() > weight){
               result.add(apple);
           }
       }
   }
   ```

3. 세 번째 시도: 가능한 모든 속성으로 필터링
   책에서는 만류에도 불구하고 모든 속성을 메서드 파라미터로 추가한 모습이라고 설명하였다. 그냥 보기에도 좋은 코드가 아니니 가능하다면 절대 따라하지 말자.

   ```java
   public static List<Apple> filterApples(List<Apple> inventory, Color color, int weight, boolean flag){
       List<Apple> result = new ArrayList<>();
       for(Apple apple: inventory){
           if((flag && apple.getColor().equals(color)) || (!flage && apple.getWeight() > weight)){
               result.add(apple);
           }
       }
       return result;
   }
   ```

   해당 책에서는 사용하는 예제도 들었지만 여기서는 그냥 넘어가겠다. 시도만을 적었는데도 시간이 아까운 형편없는 코드이기 때문이다.

   이렇게 걸어야하는 조건이 많아질수록 기존 방법에서는 복잡해지고 형편없는 코드가 만들어진다. 이러한 것을 효과적으로 전달하기 위해 동작 파라미터화를 이용해서 유연성을 얻는 방법이 등장한다.

## 동작 파라미터화

위 예시들에서 boolean 값으로 구분이 되는 경우가 있다. 예를들어 사과가 초록색인가? 사과가 150 그램 이상인가? 하는 질문들은 모두 boolean이 결과값으로 나온다.

이러한 참 또는 거짓을 반환하는 함수를 프레디케이트라고 한다. 선택 조건을 결정하는 인터페이스를 정의하자.

```java
public interface ApplePredicate{
    boolean test(Apple apple);
}
```

다음 예제처럼 다양한 선택 조건을 대표하는 여러 버전의 ApplePredicate를 정의할 수 있다.

```java
public class AppleHeavyWeightPredicate implements ApplePredicate {
    public boolean test(Apple apple) {
        return apple.getWeight() > 150;
    }
}
```

해당 Predicate를 사용하면 조건에 따라 filter 메서드가 다르게 동작할 것이라고 예상할 수 있다. 이를 전략 디자인 패턴(Strategy design pattern)이라고도 부른다.

이렇게 동작 파라미터화는 메서드가 다양한 동작(또는 전략)을 받아서 내부적으로 다양한 동작을 수행할 수 있다. 그럼 해당 방법으로 네 번째 시도를 해보자.

4. 네 번째 시도 : 추상적 조건으로 필터링

```java
public static List<Apple> filterApples(List<Apple> inventory, ApplePredicate p) {
    List<Apple> result = new ArrayList<>();
    for(Apple apple:inventory){
        if(p.test){
            result.add(apple);
        }
    }
    return result;
}
```

## 복잡한 과정 간소화

위에서 설명한 방법만으로는 아직 복잡한 과정이 남아있다. 새로운 메소드로 동작을 전달하려면 ApplePredicate 인터페이스를 구현하는 여러 클래스를 정의한 다음에 인스턴스화 해야한다. 이는 번거로운 작업이며 시간 낭비이다.

그렇다면 이것을 해결하는 방법을 알아보도록하자.

### 익명 클래스

```java
List<Apple> redApples = filterApples(inventory, new ApplePredicate()){
    public boolean test(Apple apple){
        return RED.equals(apple.getColor())
    }
}
```

익명 클래스로도 아직 부족한 점이 남아있다

1. 익명 클래스는 여전히 많은 공간을 차지한다.
2. 많은 프로그래머가 익명 클래스의 사용에 익숙하지 않다.

코드의 장황함은 나쁜 특성이다. 장환한 코드는 구현하고 유지보수하는 데 시간이 오래 걸릴 뿐 아니라 읽는 즐거움을 빼앗는 요소로, 개발자로부터 외면받는다.

### 람다 표현식 사용

자바 8에서 등장한 람다 표현식을 사용해서 다음처럼 간단하게 재구현할 수 있다.

```java
List<Apple> result = filterApples(inventory, (Apple apple) -> RED.equals(apple.getColor()));
```

### 리스트 형식으로 추상화

```java
public interface Predicate<T> {
    boolean test(T t);
}

public static <T> List<T> filter(List<T> list, Predicate<T> p) {
    List<T> result = new ArrayList<>();
    for(T e:list){
        if(p.test(e)) {
            result.add(e);
        }
    }
    return result;
}
```

이제 다른 객체, 정수, 문자열 등의 리스트에서도 필터 메서드를 사용할 수 있다. 다음은 람다 표현식을 사용한 예제다.

```java
List<Apple> redApples = filter(inventory, (Apple apple)->RED.equals(apple.getColor()));

List<Integer> eventNumbes = filter(number, (Integer i) -> i % 2 == 0);
```

이와 같은 방법으로 유연성과 간결함이라는 두 마리 토끼를 모두 잡을 수 있었다. 자바 8이 아니면 불가능한 일이다.
