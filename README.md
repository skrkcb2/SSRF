# SSRF
### 특징: 서버 측 요청을 변조 시킨다 즉 요청을 보내 서버가 내부 서버를 공격하는 식의 취약점이며 OWASP10에 새로 추가된 항목이다 
### SSRF 테스트 코드
![image](https://github.com/user-attachments/assets/80aa2687-61a4-4cd3-98b9-b585914505b5)
### SSRF 공격 체크(url 파라미터의 파일 스킴을 통해 host 파일 내용 탈취)  
### 구문: http://backend.local:8090/board/ssrf?url=file:///host경로
![ssrf](https://github.com/user-attachments/assets/7cecdccc-d681-4c6d-a1ab-384c34614c05)
### SSRF를 통한 Brute Force 공격 Burf Suite(요청을 인터셉트후 Intruder에서의 공격) / IP Scanning
### 구문: http:////backend.local:8090/board/ssrf?url=http://192.168.55.$1$ / number 설정을 통한 무차별 공격
![burf](https://github.com/user-attachments/assets/bfb0562c-e743-4d99-9a0c-28b2e78d7b13)
#### 해당 공격을 통해 1번, 113번의 주소가 추출 되는걸 볼수 있다.  
### 보안 방법  
#### 1. 코드를 통한 필터링
![secure2](https://github.com/user-attachments/assets/55273f26-25ad-47e2-bbe9-8744f3333849)
#### 1-1. 방어 결과 url 테스트(공격 구문 과 동일)    
![SSRF_SEC](https://github.com/user-attachments/assets/ff7ff35b-a395-49de-9eb9-a82f1e262476)  
#### http, https 스킴 제외 모두 차단 / http, https에서도 화이트 리스트인 backend.local 만 허용하는걸 볼수 있다.  
#### 1-2. 방어 결과 IP Scanning 테스트(공격 구문 과 동일)     
![2025-01-23 19-26-24](https://github.com/user-attachments/assets/b27c7318-81db-457f-9746-06c6e17b7151)
#### 1번과, 113번이 탐지되지 않고 400 오류: invalid_request로 바뀐 부분을 볼수 있다.
#### 2. 네트워크를 통한 탐지 및 방어(Surikata / IDS,IPS)
#### 방어 내용: SSRF 취약점이 있는 웹 서버를 통해 내부 서버로의 접근(192.168.81.135) 차단.
#### 방어 구문: drop http $EXTERNAL_NET any -> $HOME_NET any (msg:"Detect SSRF in URI"; flow:to_server,established; content:"192.168.81.135"; http_uri; nocase; sid:1000003;)
#### 방어 내용: 차단(drop), 프로토콜(http), 외부에서 내부 서버에 오는 모든 주소 포트($EXTERNAL_NET any, $HOME_NET any), 메시지 내용(msg:*), 트래픽이 서버를 향해 들어오는, tcp 연결이 이미 설정된 트래픽만 (flow:to_server,established) ,대소문자 구별 없이(nocase),패킷의 문자열 포함 할 경우(content:"192.168.81.135"), HTTP 요청의 URI를 대상으로 검사(http_uri), sid(고유 ID)
#### 방어가 안된 상태 / 공격으로는 "http://backend.local:8090/board/ssrf?url=http://192.168.81.135" 해당 get요쳥
![SSRF_DEFEND](https://github.com/user-attachments/assets/53c357f7-006a-4e30-bbe1-3e31a3dbf393)
#### 방어 테스트 / 수키라타를 통해 방어 구문을 규칙 설정 후 해당 http://192.168.81.135 서버에 요청을 차단(공격 구문은 위와 동일)
![suricata](https://github.com/user-attachments/assets/e23b6ba5-ceb4-4551-afc0-b21434c1974a)







