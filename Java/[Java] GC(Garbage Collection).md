# [Java] GC(Garbage Collection)

## GC (Garbage Collection)
- `더 이상 참조되지 않는 객체`를 찾아 메모리를 해제하는 역할
- `Heap 영역`이 GC의 대상이다.
    - cf. [[Java] Java 프로세스 메모리 영역](https://github.com/jaejlf/TIL/blob/today-i-learned/Java/%5BJava%5D%20Java%20%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%20%EB%A9%94%EB%AA%A8%EB%A6%AC%20%EC%98%81%EC%97%AD.md)
- GC는 메모리가 필요할 때, 즉 새로운 객체의 할당을 위해 한정된 Heap 영역의 재활용이 필요할 때 수행될 수 있다.

<br>

### Major GC, Minor GC
- Minor GC : Young 영역에 대한 가비지 컬렉션(Garbage Collection) 
- Full GC(Major GC) : Old 영역에 대한 가비지 컬렉션(Garbage Collection)

<br>

## GC 동작 방식
우선 GC의 대상인 Heap 영역은, 아래의 2가지 전제를 기반으로 설계되었다.

1. 객체는 생성된 후, 금방 Garbage가 된다.
    - 단편화 방지를 위해, 새로운 객체 할당만을 위한 전용 공간인 Eden 영역을 만들고, GC 당시 살아있는 객체들을 피신시키는 Survivor 영역을 구성하여, Garbage가 될 확률이 적은 객체를 따로 관리
2. 오래된(Old) 객체가 새로운(Young) 객체를 참조할 일은 드물다.

<br>

## GC의 종류

### Serial GC (-XX:+UseSerialGC)
- CPU 코어가 하나만 있을 때 사용하기 위해서 만든 가장 단순한 GC
- stop-the-world 시간이 길다.

<br>

### Parallel GC (-XX:+UseParallelGC)
- Java8의 default GC
- Minor GC를 멀티 스레드로 수행 (모든 자원을 투입하여 GC를 빨리 끝내는 전략)

<br>

### CMS GC (-XX:+UseConcMarkSweepGC)
- 애플리케이션의 스레드와 GC 스레드가 동시에 실행되어 stop-the-world 시간을 최대한 줄이기 위한 전략
- GC 대상을 파악하는 과정이 복잡한 여러 단계로 수행되기 때문에 다른 GC 대비 CPU 사용량이 높고, 메모리 단편화 문제가 있다.
- ⛔ Java9 부터 deprecated, Java14에서는 사용 중지됨!

<br>

### G1GC (-XX:+UseG1GC)
- Java 9+ 버전의 default GC
- 4GB 이상의 힙 메모리, Stop the World 시간이 0.5초 정도 필요한 상황에 사용 (Heap이 너무 작을 경우 미사용 권장)
- Region이라는 새로운 개념 도입
    - Eden, Survivor, Old 등 역할을 고정이 아닌 동적으로 부여한다. 가비지로 가득 찬 영역을 빠르게 회수하여 빈 공간을 확보하므로, 결국 GC 빈도가 줄어드는 효과를 얻게 됨.
    - 이전의 GC들처럼 일일이 메모리를 탐색해 객체들을 제거하지 않고, 대신 메모리가 많이 차있는 영역(region)을 인식하는 기능을 통해 메모리가 많이 차있는 영역을 우선적으로 GC
    - G1 GC에서는 순차적으로 이동하지는 않는다. 대신 G1 GC는 더욱 효율적이라고 생각하는 위치로 객체를 Reallocate(재할당)
 
<br>

### ZGC (-XX:+UseZGC)
- ZGC가 내세우는 최대 장점 중 하나는 힙 크기가 증가하더도 'stop-the-world'의 시간이 절대 10ms를 넘지 않는다는 것
  
