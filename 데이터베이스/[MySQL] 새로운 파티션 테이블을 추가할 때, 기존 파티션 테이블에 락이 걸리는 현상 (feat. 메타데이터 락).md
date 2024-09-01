# [MySQL] 새로운 파티션 테이블을 추가할 때, 기존 파티션 테이블에 락이 걸리는 현상 (feat. 메타데이터 락)

## Intro

![image](https://github.com/user-attachments/assets/e5b408e1-8dfc-4733-ad17-adf2bdf44ee3)

- 일(daily) 단위로 range 파티셔닝이 된 테이블에, 주기적으로 새로운 파티션 테이블을 추가하는 스케쥴이 동작하고 있었다.
- 스케쥴을 통해 새로운 파티션 테이블을 추가(ALTER TABLE ADD PARTITION) 하던 중, 기존의 파티션 테이블에 INSERT 하는 쿼리가 `Waiting for table metadata lock`인 상태로 대기하고 있는 이슈가 발생했다.
- 지금 INSERT 중인 파티션 테이블과 전혀 관련이 없는, 물리적으로 분리된 다른 파티션 테이블에서 작업을 하고 있는데 왜 문제가 생긴 것인지… 에 대해 이해하려면 `메타데이터 락`의 개념에 대해 알아야 한다.

<br>

## 메타데이터 락

- 메타데이터 락은 테이블의 메타데이터의 변경, 즉 테이블(또는 뷰)의 이름이나 스키마를 변경할 때 발생하는 락이다.
    - 메타데이터 : 데이터를 설명하는 데이터 (테이블 스키마, 컬럼 정보, 인덱스 등)
    - 메타데이터 락이 발생하는 상황 : `ALTER TABLE`, `DROP TABLE`, `CREATE INDEX`, …
- 명시적으로 획득/해제할 수 없고, 테이블의 변경이 일어날 때 자동으로 획득하는 락이다.
- 테이블 수준에서 작동하는 락이다. (즉,  테이블 파티셔닝 여부와 무관하게 해당 테이블 전체에 락이 걸린다.)
- 메타데이터 락이 걸렸을 때, 단순 SELECT는 일반적으로 영향을 받지 않으며, INSERT/UPDATE/DELETE는 메타데이터 락이 해제될 때까지 대기하거나 실패할 수 있다.

<br>

### 'Waiting for table metadata lock' 해결 방법

1. 파티션 테이블 생성 스케쥴을 트래픽이 적은 시간대로 조정하였고,
2. ALTER 쿼리의 수행 시간이 빠르게 끝날 수 있도록 작업 단위를 작게 변경하였다.

당장은 이렇게 해결이 되었지만, 트래픽이 늘어나거나 어떠한 이유로든 메타데이터 락을 잡고 있는 시간이 길어지게된다면, 언제든 서비스 장애로 이어질 수 있을 것이다…🥶 (`Online DDL` 기능을 사용하면 테이블 변경이 일어나는 중에도 INSERT/UPDATE를 수행할 수 있다고 하는데, 관련해서는 더 조사가 필요하다.)

<br>

## (추가!) MAXVALUE 파티션 테이블 사용은 신중하게…

range 파티셔닝을 할 때, 지정된 범위를 초과하는 데이터를 처리하기 위한 용도로 MAXVALUE 파티션을 두었다.  그런데 MAXVALUE 파티션 테이블이 존재하면, 그 뒤의 파티션 테이블을 추가할 때 아래와 같이 `REORGANIZE` 명령어 사용해야 한다.  (필수적으로!)

```sql
ALTER TABLE p_table1 REORGANIZE PARTITION pMAXVALUE INTO ( -- REORGANIZE!
	PARTITION p20240902 VALUES LESS THAN (TO_DAYS('2024-09-03')),
	PARTITION pMAXVALUE  VALUES LESS THAN (MAXVALUE)
);
```

REORGANIZE 명령어는 MAXVALUE 파티션 테이블을 ‘쪼개서’ 새로운 파티션을 만들겠다는 것으로, `해당 파티션 테이블에 들어있는 데이터를 모두 재배치`하는 작업이 필요하다.

재배치해야 하는 데이터가 많을수록 수행 시간이 오래 걸리고 DB 서버에 영향이 갈 수 있으므로, 지정된 범위를 초과하는 데이터가 들어올 일이 없다면, 굳이 MAXVALUE 파티션을 두어 불필요한 REORGANIZE 작업이 일어나지 않도록 하는 것이 좋을 것 같다.

<br>

## Summary

-  테이블(또는 뷰)의 이름이나 스키마를 변경할 때, 테이블 수준의 메타 데이터 락이 걸린다.
-  INSERT/UPDATE/DELETE는 메타데이터 락이 해제될 때까지 대기하거나 실패할 수 있다.
- range 파티셔닝을 할 때, MAXVALUE 파티션 테이블이 꼭 필요한지는 고민해 볼 필요가 있다.