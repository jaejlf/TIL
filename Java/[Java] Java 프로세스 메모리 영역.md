# [Java] Java 프로세스 메모리 영역

![image](https://github.com/user-attachments/assets/81315026-3615-44b7-9714-98cd7b8faf26)

_머리에 큰 그림을 넣기 위한 hierarchy. 이것저것 참고하며 그린 거라 틀린 부분이 있을지도 ;-;_

<br>

## JVM 메모리 

![image](https://github.com/user-attachments/assets/1cec7393-0b8d-4f97-a6d5-555d1f377362)

- JVM 메모리 영역 = 런타임 데이터 영역(Runtime Data Area)
- JVM이 프로그램을 수행(run)하기 위해 OS에서 할당받은 메모리 공간이다. 즉, 프로그램 실행 데이터를 관리하는 영역이기 때문에 `런타임 데이터 영역`이라고 부른다.

| 메모리 영역 | 스레드 간 공유 | 생성-소멸 | GC 대상 |
| -- | -- | -- | -- |
| Heap 영역 | O | 프로그램 실행-종료 | O | 
| Method 영역 | O | JVM 실행-종료 | X | 
| JVM 스택 | X | 스레드 시작-종료 | X | 
| Native Method 스택 | X | 네이티브 메서드 호출-종료 | X | 
| PC Registers(프로그램 카운터) | X | 스레드 시작-종료 | X | 

> 🗑️ Method 영역은 Java8부터 MetaSpace 영역 즉, Non-Heap 영역에 위치하므로 일반적으로는 GC의 대상이 아니다. 하지만, MetaSpace가 가득차거나 사용되지 않는 클래스 메타 데이터를 정리하는 과정에서 GC가 발생할 수 있다.

<br>

### Heap 영역

![image](https://github.com/user-attachments/assets/865b8ff2-7eb6-4307-843c-5cb997b06a2e)

- new 명령어로 생성된 객체와 배열이 저장되는 영역
- 객체들에는 `참조값`을 통해 접근하고, 참조가 사라지면 GC에 의해 메모리에서 해제된다.
- 구성 요소
    - Young Gen
        - Eden : 새로 생성된 객체들이 Heap에 최초로 할당되는 공간
        - Survivor
            - Eden 영역에서 살아남은 객체들이 잠시 머무르는(= 대피하는) 공간
            - Survivor 영역은 2개로 구성되는데, 객체를 여기로 이동시킬 때에는 하나의 Survivor 영역만 사용 (이후 또 다른 Survivor 영역으로 이동될 수 있음.)
            - `Minor GC` : Eden 영역에서 Survivor 영역으로 옮기는 전반의 과정
    - Old Gen
        - Young 영역에서 오랫동안 살아남은(= 특정 횟수 이상 참조되어 기준 age를 초과한) 객체들이 이동하는 공간
        - 비교적 오랫동안 참조가 되어, 앞으로도 계속 사용될 확률이 높은 객체들을 저장하는 공간
        - `Full GC(Major GC)` : Old Gen의 메모리가 충분하지 않을 때, Old 영역에서 발생하는 GC
          
<br>

### Method 영역
- 실행 중인 프로그램의 메서드와 클래스 정보를 저장하는 영역 (클래스, 변수, 메서드, static 변수, 상수 등)
- 구성 요소
    - 런타임 상수 풀(Runtime Constant Pool)
        - 각각의 클래스 및 인터페이스에 대한 constant_pool 테이블을 실행 시 참고하기 위한 저장소
        - 상수 값들을 저장
    - 필드 정보, 메서드 정보/코드, 예외 테이블
 
> ✅ Method 영역과 Perm 영역 / MetaSpace 영역
> 
> 간략하게 메모하고 넘어가자면, 
> - Java8에서부터 Method 영역이 MetaSpace 영역으로 대체되었다 (x)
> - Java8에서부터 Method 영역이 저장되는 위치가 Perm Gen에서 MetaSpace 영역으로 대체되었다 (O)

<br>

### JVM 스택
- 메서드 내에서 사용되는 값(매개변수, 지역변수, 리턴값 등)들이 저장되는 영역
- 메서드가 호출될 때마다 프레임(Frame)이 추가된다.

<br>

### 네이티브 메서드 스택
- 네이티브 메서드(= Java가 아닌 다른 언어(C/C++ 등)를 통해 OS단을 제어하는 메서드)의 호출을 위해 할당되는 영역 (JVM 스택과 사실상 비슷한 역할!)

<br>

### PC 레지스터(프로그램 카운터)
- 현재 실행 중인 스레드의 바이트 코드의 `주소값`이 저장되는 영역
- 현재 명령이 끝나면 값을 증가시켜서 그 다음 명령어를 실행한다.
- 스레드 전환 후 이전에 실행하다가 멈춘 지점을 정확하게 찾아가야하므로, 스레드마다 독립적인 PC(프로그램 카운터)를 가진다.
- 네이티브 메서드를 실행 중일 때는 Undefinded 값을 가진다.

<br>

## Non-JVM 메모리

### JNI(Java Native Interface)
- Java가 다른 언어로 만들어진 어플리케이션과 상호 작용할 수 있는 인터페이스 제공하는 프로그램
- Java는 특정 운영체제에 종속되지 않도록, JVM 위에서 실행되기 때문에, 운영체제의 고유 기능(Native)을 모두 담지는 못한다. 이러한 경우에 운영체제가 구현된 언어(C/C++)를 직접 수행할 수 있도록 해주는 방법이 바로 JNI!
-  네이티브 메모리를 관리해야 하므로, 메모리 누수가 발생할 가능성이 있다. 따라서 사용에 주의가 필요하다.

<br>

### Direct Buffer
- Direct Buffer를 이용해 네이티브 메모리 영역을 사용하는 경우 (ex. java.nio.DirectByteBuffer)
