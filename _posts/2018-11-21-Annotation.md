---
title: Annotation
key: 20181121
tags: java
---
## 메타 에너테이션
### @Target

에너테이션이 적용 가능한 대상을 지칭

에너테이션, 생성자, 필드, 지역변수, 메서드, 패키지, 매개변수, 타입

```java
@Target({FIELD,TYPE,TYPE_USE})
클래스

////
@Target(ElementType.TYPE)
클래스
```

* FIELD : 기본형 타입
* TYPE_USE : 참조형 타입

### @Retention

유지 시간

```java
@Retention(RetentionPolicy.SOURCE)
클래스
```

* SOURCE : 소스파일에만
* CLASS : 클래스 파일에 존재, 실행시에 사용 불과, default
* RUNTIME : 클래스 파일에 존재. 실행시에 사용 가능

### @Repeatable

여러번 붙일 수 있게 한다.



## 에너테이션 타입 정의하기

### 에너테이션의 요소

에너테이션 내에 선언된 메서드를 '에너테이션의 요소(element)'라고 한다

```java
@interface testInfo{
    int	count();
    String testBy();
    String[] testTools();
    TestType testType();
    DateTime testDate();	//에너테이션도 가능
}

@interface DateTime(){
    String yymmdd();
    String hhmmss();
}
```

에너테이션의 요소는 반환값이 있고 매개변수는 없는 추상 메서드의 형태를 가진다.

에너테이션을 적용할 떄 이 요소들의 값을 빠짐없이 지정해 주어야 한다.

```java
@TestInfo{
    count = 3,
    testBy = "kim",
    testTools = {"JUnit","AutoTester"},
    testType=TestType..FIRST,
    testDate=@DateTime(yymmdd="160101",hhmmss="235959")
}
```

에너테이션의 각 요소는 기본값을 가질 수 있으며, 기본값이 있는 요소는 에너테이션을 적용할 때 값을 지정하지 않으면 기본값이 사용된다.

```java
@interface TestInfo{
    int count() default 1;
}
```

에너테이션의 요소가 오직 하나뿐이고 이름이 value인 경우, 에너테이션을 적용할 때 요소의 이름을 생략하고 값만 적어도 된다.

```java
@interface TestInfo(){
    String value();
}

@TestInfo("passed")
class NewClass{...}
```

요소가 배열일 경우

```java
@interface TestInfo{
    String[] testTools();
}

@Test(testTools={"JUnit","AutoTester"})
@Test(testTools="JUnit")
@Test(testTools={})
```

모든 에너테이션의 조상은 Annotation이다. 그러나 에너테이션은 상속이 허용되지 않는다.

에너테이션은 일반적인 인터페이스로 정의되어 있다. 

그리고 에너테이션 인터페이스에 equals() hashCode() toString() 이 있으므로 모든 에너테이션은 앞의 메서드를 사용할 수 있다.

에너테이션 요소의 규칙

* 기본형, String, enum, 에너테이션, Class만 허용
* 매개변수 선언 불가
* 예외 선언 불가
* 요소를 타입에 타입 매개변수 불가 ex)ArrayList<T>

## 예제

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
    String strValue();
    int intValue();
}
```

```java
public class MyService {
    @MyAnnotation(strValue = "hi", intValue = 0607)
    public void printSomething(){
        System.out.println("test my annotation");
    }
}
```


```java
public static void main(String[] args) throws Exception{
    Method[] methods = Class.forName(MyService.class.getName()).getMethods();

    for (int i = 0; i < methods.length; i++) {
        if (methods[i].isAnnotationPresent(MyAnnotation.class)) {
            MyAnnotation an = methods[i].getAnnotation(MyAnnotation.class);

            System.out.println("my annotation str value:" + an.strValue());
            System.out.println("my annotation int value:" + an.intValue());q
        }
    }
}
```

