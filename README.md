# SSRF
### 특징: 서버 측 요청을 변조 시킨다 즉 요청을 보내 서버가 내부 서버를 공격하는 식의 취약점이며 OWASP10에 새로 추가된 항목이다 
### SSRF 테스트 코드
![image](https://github.com/user-attachments/assets/80aa2687-61a4-4cd3-98b9-b585914505b5)
### SSRF 공격 체크(공격을 통해 host 파일 내용 탈취)
![ssrf](https://github.com/user-attachments/assets/7cecdccc-d681-4c6d-a1ab-384c34614c05)
### SSRF를 통한 Brute Force 공격(Burf Suite / IP Scanning)
![burf](https://github.com/user-attachments/assets/bfb0562c-e743-4d99-9a0c-28b2e78d7b13)
#### 해당 공격을 통해 1번, 113번의 주소가 추출 되는걸 볼수 있다.  
### 보안 방법  
#### 1. 코드를 통한 필터링
![secure2](https://github.com/user-attachments/assets/55273f26-25ad-47e2-bbe9-8744f3333849)
#### 1-1. 방어 결과 url 테스트  
![SSRF_SEC](https://github.com/user-attachments/assets/ff7ff35b-a395-49de-9eb9-a82f1e262476)  
#### http, https 스킴 제외 모두 차단 / http, https에서도 화이트 리스트인 backend.local 만 허용하는걸 볼수 있다.  
#### 1-2. 방어 결과 IP Scanning 테스트  
![2025-01-23 19-26-24](https://github.com/user-attachments/assets/b27c7318-81db-457f-9746-06c6e17b7151)
#### 1번과, 113번이 탐지되지 않고 400 오류: invalid_request로 바뀐 부분을 볼수 있다.
#### 2. 네트워크를 통한 탐지 및 방어(Surikata / IDS,IPS)





