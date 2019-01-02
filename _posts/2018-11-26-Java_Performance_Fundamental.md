---
title: Java_Performance_Fundamental
key: 20181126
tags: Book
---

### Java Virtual Machine

### 자바란 뭐야?

* java programming language(.java)
  * 생산성
  * 런타임 메모리를 직접 할당하지 않는다.
* java class file format(.class)
  * Dynamic Linking : 클래스 파일은 실행 시간에 linking이 일어난다. ->  compact한 형태(lib 포함 x)
  * Bytecode로의 변경
  * Platform 독립적
  * Network Byte Order 사용
  * Symbolic Reference : 객체의 특정 메모리로 참조관계를 구성하지 않고 이름만으로 구성한다.
* java api
  * os와 java프로그램을 이어주는 가교 역할
* jvm
  * 하나의 개념 스펙에 불과하다.
  * 정의된 스펙을 구현한 하나의 독자적인 runtime instance

> .java를 컴파일하여 .class를 만들고(api도 추가한다) 이것을 jvm위에 돌린다.

## Runtime Data Areas

JVM이 프로그램을 수행하기 위해 OS 로 부터 할당 받는 메모리 영역

* PC register
* JVM Stacks
* Native method stacks
* Method area 
* Heap

> PC register , JVM stacks, native method stacks는 각 Thread별로 생성 되고 Method Area와 Heap은 모든 Thread에게 공유된다.

### PC Registers

JVM은 CUP에 직접 Instruction을 수행하지 않고 Stack에서 Operand를 뽑아내어 이를 별도의 메모리에 저장한다. 이것이 PC Registers다.

현재 스레드가 실행하는 주소와 명령어를 저장한다.

Native method를 실행할 때에는 JVM을 거치지 않고 실행되기 때문에, 이 때에는 undefined상태이다.

### JVM Stacks

Thread의 수행 정보가 있는 Frame을 저장하는 메모리 영역 Thread가 메서드를 하나 수행하게 되면 JVM은 *JVM Stack Frame*을 JVM stacks 에 Push 한다.

스택 트레이스나 스택 덤프로 얻어낸 정보가 나오는 곳이 JVM Stacks이다.

Current Frame -> 현재 수행하고 있는 모서드의 정보를 저장하는 곳

Current Class : 현재 실행되고 있는 클래스

스택 프레임에는 메서드의 파라미터 변수, 로컬 변수, 연산의 결과등을 저장한다.

스레드가 자바 메서드를 실행하면 JVM 은 Stack Frame을 하나 생성하여 JVM stacks에 Push 한다.

#### Stack Frame

Thread가 수행하고 있는 Application을 메서드 단위로 기록하는 곳이다.

Local Variable Section, Operand Stack, Frame Data 의 세 부분으로 구성되어 있다.

* Local Valriable Section

  * 파라미터, 지역변수를 저장한다.
  * 0부터 시작하는 Array로 구성됭 있고, Array의 인덱스를 통해 데이터에 접근한다.
  * 원시값은 크기에 따라 다른 크기로 할당되지만, 레퍼런스는 고정된 크기를 가진다.
  * byte, short, char는 int형으로 저장되고 실제로 쓰일 떄 원복된다.
  * boolean은 하나의 숫자에 불과하다.
  * Array의 0번 인덱스는 hidden this는 Local Method 혹은 Instance Method에 무조건 포함되는 것으로써, 여기에 저장되는 Reference를 통해 Heap에 있는 Class의 인스턴스 데이터를 찾아가게 된다. 클래스 메서드에는 존자해지 않는다.

* Operand Stack

  * JVM의 작업 공간
  * 하나의 Instruction 이 연산을 위해 Operand Stack에 값을 push하면 다음 Instruction에서는 이 값을 pop해서 사용한다.

  ```java
  public void operandStack(){
  		int a,b,c;
  		a = 5;
  		b = 6;
  		c = a + b;
  }
  
  public void operandStack();
      Code:
         0: iconst_5				//5 push
         1: istore_1				//Local Variable section의 1번 인덱스에 저장
         2: bipush        6		//6 push
         4: istore_2				//Local Variable section의 2번 인덱스에 저장
         5: iload_1				//1번 인덱스를 Operand Stack에 push
         6: iload_2				//2번 인덱스를 Operand Stack에 push
         7: iadd					//integer 더하기 연산
         8: istore_3				//Operand Stack을 pop해서 3번 인덱스에 저장
         9: return
  ```

* Frame Data

  * Constant Pool Resolution, Method가 정상종료했을 때의 정보, 비정상 종료했을 떄의 Exception 정보
    * Constant Pool Resolution : Constant Pool의 포인터 정보
    * Constant Pool이란 Resolution 하기 위한 정보가 모여있는 곳이다.
    * Resolution 이란 Symbolic Reference로 표현된 Entry를 Direct Reference 로 변경하는 과정이다.
  * 정상 종료시 Frame Data는 자신을 호출한 Stack Frame의 Instruction 의 포인터가 들어있다. 메소드가 종료되면 JVM은 이 정보를 PC Register에 설정하고 Stack Frame을 빠져 나간다.
  * 예외 발생시 예외를 어디서 핸들링해야 할지를 알려주는 Exception Table이 존재한다.

  ```java
  public void operandStack(){
  		int a,b,c;
  		a = 5;
  		b = 6;
  		try{
  			c = a + b;
  		}catch(NullPointerException e){
  			c = 0;
  		}
  }
  
  public void operandStack();
      Code:
         0: iconst_5
         1: istore_1
         2: bipush        6
         4: istore_2
         5: iload_1
         6: iload_2
         7: iadd
         8: istore_3
         9: goto          16
        12: astore        4
        14: iconst_0
        15: istore_3
        16: return
      Exception table:			//Exception table
         from    to  target type
             5     9    12   Class java/lang/NullPointerException
  ```


