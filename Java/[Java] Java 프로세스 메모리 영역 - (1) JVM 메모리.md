# [Java] Java 프로세스 메모리 영역 - (1) JVM 메모리

![image](https://github.com/user-attachments/assets/4637721d-9359-4332-a014-ef1282960c8f)

머리에 큰 그림을 넣기 위한 hierarchy. 이것저것 참고하며 그린 거라 틀린 부분이 있을지도 ;-; 

먼저, 파란색 영역인 JVM 메모리 영역 부터 알아보기.

<br>

## JVM 메모리 영역

![image](https://github.com/user-attachments/assets/db8ceca3-00b5-4a7f-b568-ad59adbaab4a)

- JVM 메모리 영역 = 런타임 데이터 영역(Runtime Data Area)
- JVM이 프로그램을 수행(run)하기 위해 OS에서 할당받은 메모리 공간이다. 즉, 프로그램 실행 데이터를 관리하는 영역이기 때문에 `런타임 데이터 영역`이라고 부른다.

<br>

| 메모리 영역 | 설명 | 스레드 간 공유 | 생성-소멸 | GC 대상 |
| -- | -- | -- | -- | -- |
| Heap 영역 | new 명령어로 생성된 객체와 배열이 저장되는 영역 | O | 프로그램 실행-종료 | O | 
| Method 영역 (Java7까지만) | 클래스, 변수, 메서드, static 변수, 상수 등이 저장되는 영역 | O | JVM 실행-종료 | X | 
| Meta Space 영역 (Java8부터) | Method 영역의 정보 + 클래스의 메타 데이터가 저장되는 영역 (=> 메서드 영역 대체) | O | JVM 실행-종료 | X | 
| JVM 스택 | 메서드 내에서 사용되는 값(매개변수, 지역변수, 리턴값 등)들이 저장되는 영역으로, 메서드가 호출될 때마다 프레임(Frame)이 추가 | X | 스레드 시작-종료 | X | 
| 네이티브 메모리 스택 | 네이티브 메서드(= Java가 아닌 다른 언어(C/C++ 등)를 통해 OS단을 제어하는 메서드)의 호출을 위해 할당되는 영역으로, 언어에 맞게 스택이 생성 | X | 네이티브 메서드 호출-종료 | X | 
| PC Registers (프로그램 카운터) | 현재 실행 중인 스레드의 바이트 코드의 주소값이 저장되는 영역으로,  현재 명령이 끝나면 값을 증가시켜서, 그 다음 명령어를 실행  | X | 스레드 시작-종료 | X | 

<br>

### Heap 영역

![image](https://github.com/user-attachments/assets/865b8ff2-7eb6-4307-843c-5cb997b06a2e)
