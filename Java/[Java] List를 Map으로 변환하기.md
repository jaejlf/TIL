# [Java] List를 Map으로 변환하기

```java
/**
 * key=id, value=User 객체 그대로 넣고 싶을 때
 */

// 방법 1) stream으로 변환 & Function.identity()를 사용해서 User 객체 그대로 리턴
Map<String, User> userMap = users.stream()
        .collect(Collectors.toMap(
                User::getId,        // 키는 User의 id
                Function.identity() // 값은 User 객체 자체
        ));


// 방법 2) stream으로 변환 & 람다식(u->u)을 사용해서 User 객체 그대로 리턴
Map<String, User> userMap = users.stream()
        .collect(Collectors.toMap(
                u -> u.getId,
                u -> u
        ));
```
- `스트림`을 통해 원하는 컬렉션(ex. Map)으로 수집할 수 있다.
- 변환 없이 그대로 처리가 필요할 때,  `Function.identity()` 또는 `u -> u`를 사용할 수 있다.
    - cf. Function.identity()와 u->u의 차이 : https://stackoverflow.com/questions/27524445/does-a-lambda-expression-create-an-object-on-the-heap-every-time-its-executed/27524543#27524543
- ⛔ Map은 각각의 key가 unique해야 하므로, Map으로 변환 시에는 key가 중복되는 경우가 없는지 주의가 필요하다. 
    - (이런 경우가 없는 것이 가장 좋겠지만) key로 사용하고자 하는 값이 중복될 수 있는 List를 Map으로 변환하고자 한다면 아래와 같이 대응할 수 있다.
    - ```java
        Map<String, User> userMap = users.stream()
               .collect(Collectors.toMap(
                       User::getId,
                       Function.identity(),
                       (existing, replacement) -> existing // 중복된 key가 있을 경우, 기존 값 유지
               ));
       ```
