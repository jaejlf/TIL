# [Java] 날짜(Date)와 시간(Time)을 처리하기

![image](https://github.com/user-attachments/assets/fa8aa031-6bc2-41f6-8c76-a973005b28d8)

## LocalDateTime, ZoneDateTime, Instant 
- LocalDate (날짜) + LocalTime (시간) = LocalDateTime (날짜  & 시간)
- LocalDateTime (날짜 ) + Timezone (시간대) = ZonedDateTime (날짜  & 시간)
- Instant
    - 날짜와 시간을 초(나노초) 단위로 표현 (타임스탬프) 
    - `에포크 타임 (1970년 1월 1일 00:00:00 UTC)을 기준`으로 경과한 시간을 초(나노초) 단위로 표현한다. 즉, 타임존과 무관하게 항상 일관된 값을 가진다. 
    ``` java
    long epochMilli = localDateTime
                      .toInstant(ZoneOffset.UTC) // UTC 타임존에 맞추어 Instant로 변환
                      .toEpochMilli(); // 밀리초 단위의 타임스탬프로 변환
    ```

<= Local은 시간대를 포함하지 않으므로, 타임존에 따라 결과가 달라질 수 있다.
<= 글로벌~ 서비스를 제공해야 할 때, LocalDateTime을 써야 하나 ZoneDateTime을 써야 하나! 상황에 따라 고민이 필요하다. (어떤 타임존으로 데이터를 내려줘야 하는지 명확하지 않은 상황이라면, 그냥 일관되게 타임스탬프로 내려주는 것도 괜찮은 방법인 것 같다.)

<br>

## Duration으로 시간 차이 구하기

- 시간 - 시간 = Duration
    ```java
     Long differ = Duration.between(start, end).toMinutes() // end-start의 차이를 분 단위로 리턴 (start > end인 경우 마이너스로 결과가 나올 수도 있다.)
    ``` 
- cf. 날짜간 차이는 `Period`로 가능하다고 함!