# [Java] Java 프로세스 메모리 영역
```
| JVM 메모리
├── Heap Memory
│ ├── Young Generation
│ │ ├── Eden Space
│ │ ├── Survivor Space 1
│ │ └── Survivor Space 2
│ └── Old Generation
├── Non-Heap Memory
│ ├──Stack Memory
│ ├── Method Area
│ ├──Program Counter (PC)
│ ├── Native Method Stack

| Non-JVM 메모리
├── JNI
├── Direct Buffer
```

<br>

## [Java] JVM 메모리 

![image](https://github.com/user-attachments/assets/c6e55d84-7598-4657-8405-34f5b0f768fa)

## Heap 영역
- 객체와 배열이 생성되는 영역
- `GC` 가 관리하는 메모리 영역 
- `모든 스레드가 공유`한다.
- (**) `Heap Overflow`
    - 생성된 객체들에 대한 참조가 살아있는 경우, Heap의 최대 용량을 넘었을 때  
    - 메모리 누수인지, Overflow 상황인지 확인해보고 -> 만약 다 필요한 객체라면, 가용 메모리 및 Heap 메모리 옵션(xmx 등) 조정 고려하기

<br>

## Method 영역
- `클래스 수준의 정보`를 저장한다. (static 필드와 클래스 구조와 같은)
- `모든 스레드가 공유`한다. 
- Heap 영역과 구분하기 위해, Non-Heap 영역이라고 하기도 한다.
- (**) `런타임 상수 풀`
    - Method 영역의 일부
    - 클래스, 메서드, 필드 등에 대한 모든 레퍼런스 저 
 
<br>

## JVM 스택
- `스레드마다 고유한` 스택 영역을 가진다.
- 메서드가 호출될 때마다, 프레임(Frame)이 추가된다.

<br>

## 네이티브 메모리 스택
- JVM Stacks와 비슷한 역할이나, `네이티브 메서드`를 실행할 때 사용하는 스택 영역
- 네이티브 방식을 사용하고 있는 메서드가 있다면, 해당 메서드는 네이트브 스택 영역에 쌓인다.
- cf. `네이티브 메서드` : Java가 처리하지 않는, OS단에서 제어하는 코드 (C/C++)

<br>

## PC Registers (프로그램 카운터)
- 현재 실행 중인 스레드의 바이트 코드 `주소값` 저장
- 현재 명령이 끝나면 값을 증가시켜서 그 다음 명령어를 실행한다.
- `스레드마다 고유한` 프로그램 카운터를 가진다. (스레드 전환 후, 이전에 실행하다 멈춘 지점을 정확히 복원할 때 사용 => 따라서, 각 스레드의 프로그램 카운터는 서로 섞이지 않게 독립된 영역에 저장된다!)
- (**) 네이티브 메서드를 실행 중일때는, `undefined` 값
