---
layout: post
title: Java Primitive Types
date: 2019-12-31 21:25:54 +0900
categories: java
---

## 부동소숫점
- Java의 부동소숫점은 0으로 나누는것이 가능하다.
  - 1.0 / 0.0 = Infinity
  - 0.0 / 0.0 = NaN (not a number)
    - NaN == NaN은 true가 아니다. NaN 검사를 위해서는 Float.isNaN를 사용해야한다.
  - 2.0 - 1.1은 0.8999999
    - 2진수에서 나누어떨어지지 않기때문에 반올림 오류가 발생
      - BigDecimal의 Exact...  메소드를 이용해 처리 가능하다.
  - 메소드에 strictfp 제어문을 추가하면 deterministic한 연산결과를 얻을수 있다.
- Math의 (add,subtract,increment,decrement,negate)Exact메소드들은 오버플로우가 발생하면 예외를 발생한다.
- BigInteger를 사용하면 이론적으로 무한대에 가까운 정수를 저장, 계산할수 있다.
- StringBuilder, StringBuffer의 차이점은 Buffer는 모든 구문에 synchronized가 붙어있다.
- String <-> Int
  - Integer.toString(), Integer.parseInt(), String.valueOf()
- CharSequence는 String, StringBuilder, StringBuffer등 문자열을 나타내는 클래스들의 슈퍼타입(인터페이스)
- 자바는 유니코드가 16비트에서 21비트로 옮겨가던 과도기에 만들어졌기 때문에 유니코드 코드포인트를 원래의 시퀀스가 아니라 UTF-16으로 인코딩한 시퀀스를 사용한다.
  - 일부 중국어와 특수문자가 필요없다면 charAt으로 바로 코드포인트를 받아와도 된다.
  - 제대로 처리하려면 codePointAt을 사용해야한다. 코드포인트의 총수는 codePointCount로 확인할 수 있따.
  - Java8부터 CharSequence에 codePoints()로 코드포인트의 스트림을 받아올수 있는 메소드가 추가되었다.
  - 차기버전에서는 UTF-8을 사용하도록 바뀔수도 있다.
  - UTF-8과 16의 차이 (https://pickykang.tistory.com/13)

## 표준입출력
  - 표준입력을 위해서는 System.in을 사용하지만 한글자 단위만 처리가능하므로 Scanner를 사용. Scanner scanner = new Scanner(System.in)
    - next(공백), nextLine(라인), nextInt(정수)등의 메소드로 리터럴을 구분가능하다.
    - 비밀번호용으로 console을 사용할수도 있다. 
      - Console terminal = System.console();
      - terminal.readPassword("Password: ");
    - 파일 입출력을 하고싶을때는 shell의 redirection사용
      - java test.class < input > output.log
  - System.out.printf
    - System.out(String.format()) 같은 미련한 짓을 했구나..
    - $로 인수 인덱스를 지정할수 있다. <는 바로 이전에 사용한 인자 인덱스

## 오브젝트의 크기
  - Object의 구조
    - ObjectHeader
      - Klass Pointer
        - 클래스 포인터, 8byte를 사용해야할것 같지만 CompressedOops옵션이 기본으로 활성화 되어있기 때문에 4byte만 사용함
      - Mark Word
        - 4byte 였으나, 64bit arch 이후 8byte 사용
        - Usage
          - Biased Locking
            - 한 쓰레드에서만 사용하는 오브젝트일 경우 synchronization로직에서 좀 더 퍼포먼스 이득을얻을 수 있음. JVM 옵션에서 활성화 필요함
          - GC Flag
    - String
      - oops 활성화의 경우
      - ObjectHeader = 16
      - pointerToArray = 4
      - 2 Hashes (Integer) = 8
      - No Padding Needed
      - 32byte.
    - Array
      - ObjectHeader + Size = 16 (4byte oops 기준)

## 객체
- 생성자에서는 첫줄에서 this, super로 다른 생성자를 참조할 수 있다.
```java
public class Foo {
    public Foo() {
    }
}
```
- 로컬변수와는 달리 멤버변수는 타입에 따라 0, null, ""로 초기화 된다.
- 클래스를 참조할때 호출되는 초기화 블록을 사용하는것도 가능하다.
  - 정적클래스에서는 static {} 으로 사용