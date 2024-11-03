# [Java] JVM Startup Parameter (feat. 메모리 설정)

![image](https://github.com/user-attachments/assets/a5f732b9-7bf6-4ba1-bc48-00759664ab11)

## 메모리 관련 설정
- `-Xms<size>` : 초기 힙 메모리 크기를 설정
- `-Xmx<size>`: 최대 힙 메모리 크기를 설정
- `-XX:InitialHeapSize=<size>` : 초기 힙 메모리 크기를 설정 (-Xms와 동일한 역할을 하며, 내부적으로 -Xms의 값을 설정)
- `-XX:MaxHeapSize=<size>` : 최대 힙 메모리 크기를 설정 (-Xmx와 동일한 역할을 하며, 내부적으로 -Xmx의 값을 설정)
- `-XX:NewRatio=<value>` : Young : Old의 비율 (값이 n이면, Young : Old = 1 : n)
- `-XX:SurvivorRatio ` : Eden : Survivor 1 : Survivor 2의비율 (값이 n이면,  Eden : Survivor 1 : Survivor 2 = n : 1 : 1), default는 n=8
<br>


## 컨테이너 환경 관련 설정
- `-XX:+UseContainerSupport` : 컨테이너 환경을 제대로 인식하도록 하는 설정 (Java10 이상 기본 적용)
- `-XX:MaxRAMPercentage=<value>` : 최대 힙 메모리 비율을 설정
- `-XX:MinRAMPercentage=<value>` : 최소 힙 메모리의 비율을 설정

<br>

- G1GC 튜닝 옵션 : https://www.oracle.com/technical-resources/articles/java/g1gc.html
