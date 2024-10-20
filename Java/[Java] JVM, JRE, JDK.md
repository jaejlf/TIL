# [Java] JVM, JRE, JDK
## JVM, JRE, JDK

![image](https://github.com/user-attachments/assets/cdf4341e-88c0-40b8-acd2-9f9147470347)

### JVM (Java Virtual Machine)
-  Java 프로그램을 컴파일한, `바이트 코드를 실행할 수 있는` 플랫폼 종속적인 가상 환경 (사실은 그냥 프로그램!)
 -  컴퓨터들은 모두 다른 기계어를 쓰기 때문에, 각각에 맞는 기계어로 번역을 해주어야한다. JVM은 각 시스템에 맞게 바이트 코드를 기계어로 번역해주는 역할을 한다. (바이트 코드는 프로그래밍 언어와 기계어의 중간 정도)
- JVM이 바이트 코드를 `그때그때` 기계어로 번역해준다. 이렇게 그때그때 컴파일하는 방식을 `JIT(Just In Time) 컴파일` 이라고 한다. 
- `Write One Run Anywhere!` 한 번 작성하면 어디에서든 실행한다!

<br>

### JRE (Java Runtime Environment)
- JVM + Library
- Java 프로그램을 `실행`하기 위한 환경 
- java.util 등 Java 프로그램 실행에 필요한 기본 `라이브러리`가 포함된 환경 (개발 관련 도구는 포함 X)
    - Java 코드만으로 프로그램이 돌아가지는 않음! (ex. List 같은 자료구조들, 모두 라이브러리로 들어있음.) 
- 최소 배포 단위이며, Java11부터는 JRE를 따로 제공하지는 않음.

<br>

### JDK (Java Development Kit) 
- JRE + 개발툴(javac, jar, ...)
- 주요 JDK로는 Oracle JDK, Amazon Corretto가 있다.

<br>

## Java 플랫폼

![image](https://github.com/user-attachments/assets/67915f73-3a7b-4e5c-941c-305448373993)

cf. Java Archive | Oracle : https://www.oracle.com/kr/java/technologies/downloads/archive/
- `Java SE(Standard Edition)` 
    - java.lang, java.io 등 Java 프로그래밍에 필요한 기본 기능과 표준 라이브러리를 표함
    - 다른 에디션들도 Java SE를 기반으로 구축
- `Java EE(Enterprise Edition)`
    - 기업용 애플리케이션 개발을 위해, SE에서 확장된 버전
    - Servlet, JDBC 등 웹 애플리케이션과 대규모 시스템을 위한 추가 기능을 포함