# [네트워크] URL 인코딩&디코딩
## Intro
```
- www.api.com/name=this/is/error : 200 OK
- www.api.com/this/is/error : 404 Not Found
```

=> query param에 넣으면 200 OK, path variable에 넣으면 404 Not Found
- 당연하게도 path variable에서의 `/`는 경로 구분자로 인식되어 404 Not Found 에러가 발생한다. (404 Not Found = 요청한 경로를 찾을 수 없다!)
- query parameter의 `this/is/error`는 문자열 전체를 요청 데이터로 인식하여 에러가 발생하지 않는다. 

<br>

## URL 인코딩/디코딩
- `URL 인코딩`
    - 클라이언트 -> 서버로 요청을 보낼 때, 표준 형식(=ASCII 코드)으로 변환하는 작업
    - 공백, 특수 문자 등이나 URL의 예약된 문자('?', '&', '=' 등)와 요청 데이터를 구분하기 위함. 
    - HTTP 프로토콜에 맞게! 데이터가 전송 과정에서 변경되지 않도록! 예약어로 인해 혼동되지 않도록!
- `URL 디코딩` : 인코딩 과정에서 변환된 문자를 다시 원래 형태로 복원하는 작업 
- cf. 
    - Spring Boot에서는 HTTP Servlet Request이 디코딩을 해준다.
    - Feign 클라이언트를 통해 서버 -> 서버로 요청을 보낼 때도 Feign이 알아서 인코딩을 해서 요청을 보낸다. (package feign.codec)