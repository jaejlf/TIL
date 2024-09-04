# [운영체제] CPU Bound와 IO Bound

![image](https://github.com/user-attachments/assets/fd6c0529-f1cb-47e8-b830-3c9e5f6147cd)

cf. 버스트(burst) : 어떤 현상이 짧은 시간에 집중적으로 일어나는 것

- CPU 버스트
    -  메모리에 올라와 있는 프로세스가, 자신의 차례가 되어 CPU에서 실행됐을 때, `CPU에서 한 번에 연속적으로 실행되는 시간`
    - CPU 바운드 프로세스 = CPU 버스트가 많은 프로세스 (ex. 동영상 편집 프로그램)
- IO 버스트
    - 프로세스가 `IO 작업을 요청하고 결과를 기다리는 시간`
    - IO 바운드 프로세스 = IO 버스트가 많은 프로세스 (ex. 일반적인 API 서버) 