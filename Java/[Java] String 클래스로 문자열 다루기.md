# [Java] String 클래스로 문자열 다루기
## Intro
```java
char[] hello1 = new char[]{'h', 'e', 'l', 'l', 'o'}';
String hello2 = new String("hello");
String hello3 = "hello"; // 내부적으로 new String("hello")로 처리된다.
```
- Java에서는 아래와 같은 방법으로 문자열을 만들 수 있다.
- String 클래스에서는 내부적으로 byte[] 배열에 문자 데이터를 보관한다. (`private final byte[] value;`)
> cf. Java 9 이전에는 `char[]` 배열에 문자 데이트를 보관하였다.
> - char는 2byte인데, 영어/숫자는 보통 1byte로 표현 가능하여, 단순 영어/숫자로 된 경우 1byte를 사용하고 그렇지 않은 경우에는 2byte인 UTF-16 인코딩을 사용한다. (메모리를 더 효율적으로 사용하기 위함.)
- String은 `클래스`이므로, `참조형`이다. 
- Java 언어에서 편의를 위해 `+` 연산자를 제공한다. (`+` 연산자를 통해 문자열 연산이 이루어지도록 지원)

<br>

## equals()와 ==
- `equals()` : 두 객체가 `논리적으로 같은지` 확인 (값이 동일한지 확인)
- `==` : 두 객체가 `동일한지` 확인 (동일한 객체를 가리키고 있는지) 
```java
/**
 * `str1 == str2` 는 false, `str1.equals(str2)`는 true
 */
 String str1 = new String("hello");
 String str2 = new String("hello");


/**
 * `str3 == str4` 는 true, `str3.equals(str4)`는 true
 */
String str3 = "hello";
String str4 = "hello";
```
=> str3, str4와 같이 `문자열 리터럴`을 사용하는 경우 `String 풀`을 사용하므로, 같은 객체를 가리키고 있다.

<br>

### String 풀(Pool)

![image](https://github.com/user-attachments/assets/a9418eba-6222-4c1f-907c-b1e6a952b273)

<br>

## String은 불변 객체
- String은 `불변 객체`이므로, 변경이 필요한 경우 `새로운 객체`를 리턴하여 값을 변경할 수 있다.
    - cf. [[Java] 불변 객체(Immutable)로 사이드 이펙트 방지하기-불변 객체의 값을 변경해야할 때](https://github.com/jaejlf/TIL/issues/13#:~:text=%EC%9D%B4%EC%83%81%20%EB%B0%9C%EC%83%9D%ED%95%98%EC%A7%80%20%EC%95%8A%EB%8A%94%EB%8B%A4.-,%EB%B6%88%EB%B3%80%20%EA%B0%9D%EC%B2%B4%EC%9D%98%20%EA%B0%92%EC%9D%84%20%EB%B3%80%EA%B2%BD%ED%95%B4%EC%95%BC%ED%95%A0%20%EB%95%8C,-%EB%B6%88%EB%B3%80%20%EA%B0%9D%EC%B2%B4%EC%9D%B8%EB%8D%B0%20%EA%B0%92%EC%9D%84)
- 만약 String이 불변 객체가 아니라면?
    - `String 풀`을 사용하므로, 값이 변경된다면 다른 객체의 값도 변경되는 문제가 발생할 수 있다. 
    - ⛔ 문자열 값의 변경이 잦은 경우, 계속해서 새로운 객체가 생기고, 더 이상 사용되지 않는 객체들은 GC의 대상이 될 것이다. (시스템 자원을 더 많이 소모하게 된다.)
    - ✅ 문자열을 변경하는 동안 `가변 객체`인 `StringBuilder`를 사용하고, 변경이 끝나면 사이드 이펙트가 없도록 `불변 객체`인 `String`으로 변환하여 사용하는 것이 좋다.