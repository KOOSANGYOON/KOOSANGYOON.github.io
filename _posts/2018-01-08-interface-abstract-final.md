---
layout: post
title: "JAVA 인터페이스/추상클래스/final"
date: 2018-01-08 11:30:40
image: 'https://github.com/KOOSANGYOON/TIL/blob/master/TIL201801/abstract.jpg?raw=true'
description: TIL (추상클래스를 학습해서 추상화를 넣어봤어 ..)
category: 'TIL'
tags:
- TIL
- CODING
- INTERFACE
- ABSTRACT CLASS
- FINAL
- JAVA
twitter_text: 인터페이스/추상클래스/final 학습
introduction: 인터페이스/추상클래스/final 학습
---

# (2018.01.08)

## TIL

1. 인터페이스 / 추상 클래스

2. final 예약어

3. JAVA BOWLING 에의 적용

4. 학습 자료 출처

---
### 1. 인터페이스 / 추상 클래스

JAVA BOWLING 과제를 더욱 깔끔하고 간결하게 수행하기 위해서 `추상 클래스` 에 대해 학습했다.
추상 클래스를 학습하면서 자연스럽게 `클래스` , `인터페이스` , `추상 클래스` 를 모두 학습했다.

#### 1) 인터페이스와 추상 클래스를 사용하는 이유
- 설계시 선언해 두면 개발할 때 기능을 구현하는 데에만 집중할 수 있다.
- 개발자의 역량에 따른 메소드의 이름과 매개변수 선언의 격차를 줄일 수 있다.
- 공통적인 인터페이스와 추상 클래스를 선언해 놓으면, 선언과 구현을 구분할 수 있다.

#### 2) 인터페이스 사용 방법
- 선언 시에
    ```java
    public interface interfaceName {
      public boolean method1(parameter);
      public boolean method2(parameter);
      public boolean method3(parameter);
      ...
    }
    ```
> 이렇게 인터페이스 내부에 선언된 메소드들은 몸통이 있어서는 안된다.

 > 즉, 인터페이스 선언을 위한 중괄호만 있고, 몸통 구현을 위한 중괄호는 없다.

- 활용 시에
    ```java
    public class CLASS1 implements interfaceName {
      public boolean method1(parameter) {
        return false;
      }
      public boolean method2(parameter) {
        return true;
      }
      public boolean method3(parameter) {
        return parameter != 0;
      }
      ...
    ```
> 클래스 이름 뒤에 `implements` 라는 예약어를 붙여주면 된다.

 > 인터페이스에 정의된 메소드들의 몸통을 반드시 만들어 줘야 한다.

### ※ 상속과의 다른 점
 - JAVA 에서 상속은 하나밖에 되지 않는다.
 - 하지만, 인터페이스는 여러 개를 implements 할 수 있다.
 - 인터페이스는 `생성자` 로 사용될 수 없다.
   (컴파일러 에러 발생)

   ```java
   interfaceName classOne = new interfaceName();
   ```
  > 이렇게 사용할 수 없다.

    ```java
  interfaceName classOne = new CLASS1();
    ```
  > 이렇게 사용 가능하다. (CLASS1은 interfaceName을 인터페이스하는 클래스이다. 활용예시 참고)

#### 3) abstract class

`추상 클래스` 는 인터페이스도 아니고, 그렇다고 클래스라고도 하기 힘들다.
abstract class 역시 마음대로 초기화하고 실행할 수 없다.
그 abstract class를 구현해 놓은 클래스로 초기화 및 실행이 가능하다.

#### 4) abstract class (추상 클래스) 사용 방법

- 선언 시에

  ```java
  public abstract class className {
    public abstract boolean method1(parameter);
    public abstract boolean method2(parameter);
    public abstract boolean method3(parameter);
    public void helloWorld() {
      System.out.println("Hello, World!");
    }
  }
  ```

- 사용 시에

  ```java
  public class className1 extends className {
    public abstract boolean method1(parameter) {
      return true;
    }
    public abstract boolean method2(parameter) {
      return false;
    }
    public abstract boolean method3(parameter) {
      return parameter == 10;
    }
  }
  ```

 > abstract class 는 `abstract` 라는 예약어를 사용한다.

#### 5) abstract class 의 특징

- abstract 클래스 안에는 abstract 로 선언된 메소드를 가질 수 있다.

- abstract 로 선언된 메소드가 1개만 있더라도 그 클래스는 무조건 abstract 클래스로 선언되어야 한다.

- 인터페이스와 마찬가지로 메소드 선언문에는 `abstract` 예약어를 사용하여 만들고 몸통이 없다.

- 하지만, 인터페이스와는 달리 구현되어있는(몸통이 있는) 메소드를 가질 수 있다.

---
### 2. final 예약어

상속과 관련하여 알아두어야 하는 예약어이다. final은 클래스, 메소드, 변수에 모두 선언할 수 있다.

- 클래스에 final을 선언할 때,

  ```java
  public final class class1 {
    ...
  }
  ```

 > final 클래스는 상속을 해 줄 수 없다.
  ```java
  public class class2 extends class1 {
    ...
  }
  ```> -> 컴파일 에러 발생

- 메소드에 final을 선언할 때,

  ```java
  public abstract class class1 {
    public final void helloWorld() {
      System.out.println("Hello, World!");
    }
  }
  ```

 > final 메소드는 더이상 @Overriding 할 수 없다.

- 변수에 final을 선언할 때,

  ```java
  public class class1 {
    final int myAge = 30;
  }
  ```

  > myAge 변수는 더이상 값을 바꿀 수 없는 상수로 만들어 진다.

---
### 3. JAVA BOWLING 에의 적용

볼링의 프레임을 (1 ~ 9 프레임 / 10 프레임) 이 두가지로 구현했고,
10 프레임이 1 ~ 9 프레임을 상속받는 구조로 구현했었다.

이 구조를 Frame 이라는 추상 클래스를 만들고,

1 ~ 9 (NORMAL FRAME) / 10 (FINAL FRAME) 로 만들어서 두 프레임 모두 Frame를
상속(extends) 받는 구조로 변경하였다.

POBI (박재상 님)가 이번 bowling 과제가 아니면 추상 클래스와 인터페이스를 제대로 학습 할 기회가 없을거라고 말씀해주셔서 이렇게 구조를 변경하기로 했다.

이번 기회를 통해 추상 클래스와 인터페이스를 확실하게 정리해두고 지나갈 수 있었으면 한다.

---
### 4. 학습 자료 출처

- 출처
{저자 = 이상민 | 발행일 : 2013.02.28 | 제목 = 《자바의 神 VOL. 1 (기초 문법편)》 | 출판사 : 로드북 }
