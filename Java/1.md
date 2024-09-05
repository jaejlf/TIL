# [Java] 불변 객체(Immutable)로 사이드 이펙트 방지하기
## Intro
```java
// 기본형 타입
int a = 10;
int b = a; // a의 값을 복사하여 대입

// 참조형 타입
User user1 = new User();
User user2 = user1; // user1의 참조값을 복사하여 대입 (즉, 같은 객체를 공유)
```
=> 만약 user1과 user2를 다른 사람으로 관리하고 싶다면 아래와 같이 각각의 User 객체를 생성하여 사용하면 된다.
- ```java
    User user1 = new User();
    User user2 = new User();
    ```
- ⛔ 그러나 객체 간에 참조값을 공유하는 것을 시스템적으로 막을 수는 없으므로, 객체 변경에 의한 사이드 이펙트가 발생할 수 있다. (ex. user1의 이름을 변경했는데 user2의 이름도 변경되는)

<br>

## 불변 객체(Immutable Object)
- 객체의 상태가 `변하지 않는` 객체
- cf. 가변 객체(Mutable) : 처음 만든 이후 `상태가 변할 수 있는` 객체
```java
public class User {
    // final 키워드를 통해 필드값이 변하지 않도록
    private final String name; 
    private final String address;

    ...
}
```
=> User 객체의 필드를 모두 `final`로 선언하여, User 객체는 최초 생성된 이후 필드값을 변경할 수 없는 `불변 상태`가 되었다. 
=> ✅ 이제 User 객체가 변경되는 상황은 시스템적으로 불가하므로!  'user1의 이름을 변경했는데  user1의 이름을 변경했는데 user2의 이름도 변경' 되는 것과 같은 상황은 더 이상 발생하지 않는다.

<br>

### 불변 객체의 값을 변경해야할 때
- 불변 객체인데 값을 변경해야 하는 경우가 생겼다면, 아래와 같이 `새로운 객체`를 리턴하여 불변 객체의 값을 변경할 수 있다. (정확히 말하면 기존 객체는 여전히 불변성을 유지하고 있으며, 새로운 객체를 통해 값이 `변경된 것처럼` 보이게 하는 것이다.)
```java
public class User {
    // final 키워드를 통해 필드값이 변하지 않도록
    private final String name; 
    private final String address;

    ...

    public User rename(String name) {
        return new User(name, this.address);  // address는 그대로 유지
    }

}
```