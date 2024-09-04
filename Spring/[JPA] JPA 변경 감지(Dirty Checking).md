# [JPA] JPA 변경 감지(Dirty Checking)
- `영속 상태의` 엔티티에 변경이 일어나면, JPA가 `자동으로 감지`하여 데이터베이스에 반영되도록 하는 기능
- 영속성 컨텍스트의 `1차 캐시`를 통해 이 기능을 제공한다.

<br>

```java
User user = userRepository.findById(id) // 1번 유저
        .orElse(new User("새로운 유저", "newnewnewnew")); // 2번 유저
  
user.updateName("바꾼 이름"); // <-- 변경
  
return user; // 메서드 종료 (변경이 감지되면, 트랜잭션 커밋 시점에 UPDATE 쿼리가 자동으로 발생)
```
=> ✅ case 1) user = 1번 유저(= DB에 존재하는 기존 유저)일 때 => UPDATE 쿼리 발생
 
![image](https://github.com/user-attachments/assets/9d455e95-f54f-4c18-8068-edf07e28803c)

- 1차 캐시에 처음 DB에서 조회해온 엔티티가 존재하기 때문에, JPA가 변경을 감지할 수 있었다.
<br>

=> ⛔ case 2) user = 2번 유저(= DB에 없는 신규 유저)일 때 => SELECT 쿼리 이후, update 쿼리 발생하지 않음. 

![image](https://github.com/user-attachments/assets/59dd439a-ff72-450d-864e-b35784036ef2)

-  DB에서 조회된 값이 없는 상태로, 영속성 컨텍스트가 관리하는 엔티티가 아닌 상태에서 변경이 일어났다. 따라서 JPA가 변경을 감지할 수 없었다. (= 자동으로 update 쿼리 발생하지 않음.)


<br>

> 변경 감지를 통해 수행되는 UPDATE 쿼리는, `모든 컬럼`에 대해 UPDATE가 일어난다.
> - 코드상 email 컬럼에 대해서만 업데이트를 하였는데, 실제 UPDATE 쿼리는 모든 컬럼(name, email) 에 대해 수행된 것을 확인할 수 있다. (컬럼 많을수록 데이터 전송량 증가 <-> DB쪽 쿼리 재사용 가능하다는 것... 등.. trade off 고려해보기) 
