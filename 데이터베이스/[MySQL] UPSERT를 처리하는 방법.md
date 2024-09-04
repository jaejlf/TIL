# [MySQL] UPSERT를 처리하는 방법
- UPSERT(=데이터가 있으면 UPDATE, 없으면 INSERT) 작업이 필요할 때, MySQL에서는 `INSERT ... ON DUPLICATE KEY UPDATE` 구문으로 처리가 가능하다.

```sql
INSERT INTO 테이블명 (컬럼1, 컬럼2, ...)
VALUES (값1, 값2, ...)
ON DUPLICATE KEY UPDATE -- PK나 UK가 겹치는 경우!
    컬럼1 = VALUES(컬럼1),
    컬럼2 = VALUES(컬럼2), ...;
```

<br>

## auto_increment 사용 시 주의
### UPSERT 시, auto_increment 값 증가 
- `INSERT ... ON DUPLICATE KEY UPDATE` 구문은 내부적으로 INSERT를 수행하고 -> Duplicate Key 오류로 인해 `실패 시` UPDATE 구문을 수행하는 방식이다.
- ⛔ INSERT에 실패하더라도, 이미 증가된 auto_increment 값은 되돌려지지 않으므로, UPDATE가 잦은 테이블에서는 실제 데이터 양보다 auto_increment가 많이 높게 잡히는 이슈가 있을 수 있다. (아니 있었다..^,^ 그러나 이미 커져버린 auto_increment 값은 영영 돌이킬 수 없...)
    - 해결 방안으로는,
        - INSERT ... ON DUPLICATE KEY UPDATE 구문 대신, INSERT/UPDATE를 명시적으로 구분하여 수행하도록 로직을 변경하거나
        - UPDATE가 잦은 테이블에 auto_increment PK를 두는 것이 꼭 필요한지 근본적으로 설계를 다시 고민해볼 필요가 있다.

<br>

### auto_increment  락
- 순차적으로 일관된 일련번호를 제공해야 하므로, `새로운 레코드`를 추가하려고 할 때, 내부적으로 테이블 수준의 잠금인 `auto_increment 락`이 걸린다.
- auto_increment 값을 가져오는 순간에 락이 걸리므로, 다음 쿼리는 auto_increment를 가져오기까지 대기해야 한다. (금방 해제가 되어서 대부분 문제가 되지는 않는다고 하긴 함.)