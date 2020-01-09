---
layout: post
title: 자바의 제네릭 타입경계와 브릿지 메서드
date: 2019-12-31 20:41:32 +0900
categories: java
ref: Core Java 9, Cay S. Hostmann
---
## Generic type bound (타입 경계)
* 제너릭 클래스 혹은 메서드 타입 파라미터의 타입을 제한해야하는 경우에 사용.
* `<T extends ClassName>`의 형태로 선언한다.
```java
public <T extends AutoCloseable> void close(T closeable) {
    closeable.close();
}
```

## 제약
* Primitive 타입을 사용할 수 없다. 
    * 만들 당시에는 별 문제가 없었겠지만 함수형 인터페이스가 추가되기 시작하면서 이를 지원하기 위한 추가적인 메서드가 필요하게 되었다.
* 타입이 소거되기 때문에 런타임에 타입체크를 할 수 없다.
``` java
//컴파일러가 타입을 알수 없기 때문에 캐스팅이 필요하다.
@SupppressWarning("unchecked") ArrayList<String> list = (ArrayList<String>) obj; 
//이런식으로 모든 요소를 검사하는 리스트를 만들수도 있다.
List<String> checkedList = Collections.checkedList(new ArrayList<>(), String.class); 
```

* 타입 변수의 인스턴스를 만들수 없다.
    * ```T[] test = new T[n]``` 와 같은 호출은 불가능하다.
    * 사용자가 ```String[]::new```처럼 생성자를 메서드 참조로 전달하거나, 리플렉션의 newInstance 메서드를 사용해야한다.
* 정적 타입의 선언이 불가능하다.
* 타입 소거후에 브릿지 메서드끼리 충돌이 발생할 수도 있다.
* 제너릭 클래스의 객체를 예외로 던지거나 잡을 수 없다.

## 타입소거
* 자바의 제너릭은 하위 호환성을 위해서 VM에서 타입을 지우는 방식으로 구현되어 있다.
* 하휘 호환성 관점에서는 성공적이었지만 모호한 문제가 많이 발생한다.
    * 제너릭 타입은 경계가 있을 경우 첫번째 경계로 아닐경우 raw 타입(Object)로 컴파일된다.
    * 예를들면 `Entry<String, Integer> entry`는 `Entry<Object, Object> entry`로 타입이 소거된다.
    * 그래서 entry에서 값을 가져올때마다 컴파일러는 캐스팅하는 코드를 삽입하게 된다.
```java
Entry<String, Integer> entry = ...
String key = entry.getKey();
//vm내에서는 String key = (String) entry.getKey() 로 동작한다.
```

### 브릿지 메서드
* 메서드의 매개변수나 리턴타입을 소거하기 위해서 만들어지는 메서드
```java
public class SomeList extends ArrayList<String> {
    @Override
    public boolean add(String e) {
        ...
    }
}
void Test() {
    ArrayList<String> list = new SomeList();
    list.add("string");
}
```

* ArrayList의 타입소거된 ```add()``` 메서드를 호출할 경우 SomeList에는 ```add(Object e)``` 메서드가 존재하지 않기 때문에 이를 위해 브릿지 메서드를 생성한다.

### 와일드카드
* Employee를 상속받은 Manager클래스가 존재하고, hire(Employee employee)라는 메소드가 있다고 가정.
* Manager는 Employee의 서브타입이므로 hire(manager)로 호출이 가능하다. (convariance)
* 하지만 `hireAll(List<Employee> employees)`라는 메소드를 가정하면 얘기가 조금 복잡해진다.
    * `List<Employee>`는 `List<Manager>`의 **슈퍼타입**이 아니기 때문에 `List<Manager>`를 매개변수로 전달하지 못한다.
```java
List<Manager> managers = new ArrayList<>();
managers.add(employee); //만약 슈퍼타입이라면 이런 형태의 호출이 발생할수 있기 때문이다.
```
* 이 경우 와일드 카드를 사용하면 간단히 해결이 가능하다.
```java
void hireAll(List<Employee> employees) {} //List<Manager>를 전달할 수 없다.
void hireAll(List<? extends Employee> employees) {} //이제 전달 가능해졌다.
```
* 이 때 위 리스트에서 list의 매개변수 타입이 ```? extends Employee``` 이기 때문에 읽기는 가능하지만 쓰기가 불가능해진다. 만약 와일드카드로의 캐스팅을 시도할 경우 익셉션이 발생한다.
* 서브타입이 아니라 슈퍼타입의 와일드카드도 사용할 수 있다. 일반적으로 Predicate 같은 함수형 객체에서 많이 사용된다.
```java
void printNames(Employee[] employees, Predicate<Employee> filter) {
    employees.forEach(e -> filter.test(e) && System.out.println(e.getName));
}
void test(Employee[] employees) {
    Predicate<Object> p = o -> o.toString().length() < 10;
    printNames(employees, p); //컴파일 오류 발생
}
```
* 다음과 같이 슈퍼타입 와일드카드를 사용할 경우 Object는 Employee의 슈퍼타입이기 때문에 Predicate<Object>도 사용가능해진다.
```java
void printNames(Employee[] employees, Predicate<? super Employee> filter) {
    employees.forEach(e -> filter.test(e) && System.out.println(e.getName));
}
```