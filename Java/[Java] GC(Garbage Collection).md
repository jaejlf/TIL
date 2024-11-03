# [Java] GC(Garbage Collection)

## GC(Garbage Collection)
- `더 이상 참조되지 않는 객체`를 찾아 메모리를 해제하는 역할
- `Heap 영역`이 GC의 대상이다. (cf. [[Java] Java 프로세스 메모리 영역](https://github.com/jaejlf/TIL/blob/today-i-learned/Java/%5BJava%5D%20Java%20%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%20%EB%A9%94%EB%AA%A8%EB%A6%AC%20%EC%98%81%EC%97%AD.md))
- GC는 메모리가 필요할 때, 즉 새로운 객체의 할당을 위해 한정된 Heap 영역의 재활용이 필요할 때 수행될 수 있다.

<br>

### Major GC, Minor GC
- Minor GC : `Young 영역`에 대한 가비지 컬렉션(Garbage Collection) 
- Full GC(Major GC) : `Old 영역`에 대한 가비지 컬렉션(Garbage Collection)

<br>

## GC 동작 방식
우선 GC의 대상인 Heap 영역은, 아래의 2가지 전제를 기반으로 설계되었다.

1. 객체는 생성된 후, 금방 Garbage가 된다.
    - 단편화 방지를 위해, 새로운 객체 할당만을 위한 전용 공간인 Eden 영역을 만들고, GC 당시 살아있는 객체들을 피신시키는 Survivor 영역을 구성하여, Garbage가 될 확률이 적은 객체를 따로 관리
2. 오래된(Old) 객체가 새로운(Young) 객체를 참조할 일은 드물다.

<br>

## GC의 종류

### Serial GC (-XX:+UseSerialGC)

![image](https://github.com/user-attachments/assets/2683b3a7-e31d-43a4-887c-7adbcbd15a22)

- `단일 스레드`로 동작한다. 즉, GC가 시작되면 완료될 때까지 다른 모든 작업 스레드가 멈춰있어야 한다. 즉, 
- stop-the-world 시간이 길다.
- CPU 코어가 하나만 있을 때 사용하기 위해서 만든 가장 단순한 GC

<br>

### Parallel GC (-XX:+UseParallelGC)

![image](https://github.com/user-attachments/assets/39f85cb0-9795-44c3-b606-2e3c82b587ee)

- Java8의 default GC
- 모든 자원을 투입하여, GC를 빨리 끝내자는 전략!
- Young 영역에서 수행되는 `Minor GC`를 `멀티 스레드`로, 동시에 GC를 수행한다.
- 대규모의 데이터를 처리하는 애플리케이션(ex. 배치 서비스)에서 사용하기에 적합하다.
    - 대규모의 데이터를 처리하는 애플리케이션은 일반적으로 많은 객체를 생성하고, 이러한 객체는 짧은 생명 주기를 가진다. Parallel GC는 Young 영역에서 객체를 빠르게 수집하여 Old 영역으로 승격시키는 데에 최적화되어 있다.
    - 또한 멀티 스레드를 통해 빠르게 작업을 수행하므로, stop-the-world 시간을 최소화할 수 있다.

> 🛣️ PLAB(Parallel Local Allocation Buffer)를 통해, Youung -> Old 영역으로 이동하기 전, 스레드마 Old 영역의 일부를 미리 할당받아 사용한다. (여러 개의 스레드가 동시에 수행되더라도 원활하게 GC가 동작할 수 있도록!)

<br>

### CMS GC (-XX:+UseConcMarkSweepGC)
- 애플리케이션의 스레드와 GC 스레드가 동시에 실행되어 stop-the-world 시간을 최대한 줄이기 위한 전략
- GC 대상을 파악하는 과정이 복잡한 여러 단계로 수행되기 때문에 다른 GC 대비 CPU 사용량이 높고, 메모리 단편화 문제가 있다.
- ⛔ Java9 부터 deprecated, Java14에서는 사용 중지됨!

<br>

### G1GC (-XX:+UseG1GC)

![image](https://github.com/user-attachments/assets/e4d8d838-465b-4840-9914-9ec64f3fccf9)

- Java 9+ 버전의 default GC
- Eden, Survivor, Old 등 역할을 고정이 아닌 동적으로 부여한다. `Region`이라는 새로운 개념 도입! 각 Region이 Eden/Survivor/Old 영역을 담당한다.
- 이전의 GC들처럼 일일이 메모리를 탐색해 객체들을 제거하지 않고, 대신 메모리가 많이 차있는 영역(region)을 인식하는 기능을 통해 메모리가 많이 차있는 영역을 우선적으로 GC
- G1GC에서는 순차적으로 영역을 이동하지는 않고, 대신 G1GC는 더욱 효율적이라고 생각하는 위치로 객체를 Reallocate(재할당)
 
<br>

### ZGC (-XX:+UseZGC)

![image](https://github.com/user-attachments/assets/af25655e-3a03-459f-bcb1-7404601cf59e)

- `ZPage`라는 논리적인 단위로 메모리를 구분
- 작은 객체들이 할당되는 `Small 영역`, 중간 크기 객체들이 할당되는 `Medium 영역`, 큰 객체들이 할당되는 `Large 영역`으로 나뉜다. 각 영역의 크기는 동적으로 결정된다.
  
