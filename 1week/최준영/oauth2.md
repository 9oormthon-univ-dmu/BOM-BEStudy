
* Resource Owner : 사용자
* Resource Server : 플랫폼
* Client : 어플리케이션 서비스

---

* client_id : 클라이언트 식별자   
* client_secret : 클라이언트 비밀번호   
* redirect_uri : 접근 코드 응답을 받을 URI   

---
## OAuth2 토큰 발급 과정

1. 사용자의 소셜로그인
2. 플랫폼에서 Redirect_uri 경로로 접근 코드 전달
3. 클라이언트에서 접근 코드를 받아 플랫폼에게 토큰을 요청
4. 플랫폼에서 Access_token과 refresh_token 응답

---

## API 호출

1. 클라이언트에서 Access_token을 헤더에 담아 API 호출
2. 플랫폼에서 API 호출에 대한 응답
3. Access_token 만료시 refresh_token을 이용하여 재발급
4. 플랫폼에서 Access_token과 refresh_token 응답
5. 위 과정 반복

--- 

## Access_token과 Refresh_token

* Access_token : API 호출시 사용되는 토큰
* Refresh_token : Access_token 만료시 재발급을 위한 토큰

보통 토큰 탈취의 위험성 때문에 Access Token은 짧게, Refresh Token은 길게 설정   
Access Token은 만료시간이 짧기 때문에 탈취 당해도 길게 사용되지 않음   
Refresh Token은 보통 요청시에 사용되지않고 Access Token의 재발급을 위해 사용되므로 탈취당할 위험성이 낮음, 따라서 만료 시간을 길게 설정   


# 마무리
1주차 스터디를 통해서 OAuth2에 대하여 다시 복습하고 정리할수있어서 좋았다.


