# 2022년 2월 19일

## 1. HTTP 개관

- HTTP: 신뢰성 있는 데이터 전송 프로토콜 사용, 순서와 안전성이 보장
- 서버는 리소스를 저장하고 클라이언트가 요청한 데이터를 HTTP응답으로 돌려준다.
    - 웹 리소스: 모든 종류의 콘텐츠 소스
- MIME 타입: 웹에서 전송되는 객체 각각에 붙이는 데이터 포멧 라벨 (수많은 타입을 다루므로)
    - 클라이언트(브라우저)는 이 타입을 다룰 수 있는지 확인하고 필요에 따라 외부 플러그인 소프트웨어를 실행하여 파일을 다룬다.
    - 주 타입과 부 타입을 `/`로 구분한 문자열 라벨이다. (e.g. `text/html`, `image/jpeg`, `application/json` )
- URI(통합 자원 식별자): 서버 리소스를 식별하고 위치를 지정할 수 있는 이름
    - URL: 리소스에 대한 구체적인 위치. `https://`스킴(프로토콜) + `www.naver.com`서버의 인터넷 주소 + `/specials.gif/` 리소스 이름
    - URN: 위치에 상관없이 이름만 식별가능하다면 접근이 가능, 아직 실험중인 상태
- 트랜잭션: 요청 명령과 응답 결과로 이루어져 있다.
    - 메서드(요청): 요청 메시지가 갖는 하나의 동작으로, 서버에게 어떤 요청인지 알림 (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`)
    - 상태코드(응답): 요청에 대한 응답 상태를 알려주는 세 자리 숫자
    - 메시지(요청, 응답): 단순한 줄 단위의 문자열, 세 가지로 구성
        - 시작줄: 위의 메서드나 상태코드 등 무엇을 해야 하는지, 무엇을 했는지 나타낸다.
        - 헤더: `이름:값`의 형태로 구성된 문자열, 빈 줄로 끝난다.
        - 본문: 임의의 이진 데이터 포함 가능
- TCP(Transmission Control Protocol, 전송 제어 프로토콜) / IP
    - TCP: 대중적이고 신뢰성있는 인터넷 전송 프로토콜로, 오류가 없고 순서에 맞는, 어떤 크기로도 보낼 수 있게 조각나지 않는 데이터스트림을 제공한다. HTTP는 자신의 메시지 데이터를 전송하기 위해 TCP 상용하며, 이와 마찬가지로 TCP는 IP 위의 계층
    - TCP/IP: TCP와 IP가 층을 이루는, 패킷 교환 네트워크 프로토콜의 집합
    - IP: HTTP 클라이언트가 요청을 보내기 전 IP 주소, 포트번호를 사용해 TCP/IP 커넥션을 맺어야 하므로 서버 IP 주소와 서버에서 실행중인 프로그램이 사용 중인 포트번호가 필요하다. URL로 이를 알 수 있으며 URL은 DNS를 통해 쉽게 IP로 될 수 있다.
- 프로토콜 버전 (10장 참고할 것)
- 웹의 구성요소: 애플리케이션
    - 프락시: 클라이언트의 모든 HTTP 요청을 받아 서버에 전달, 보안을 위해 사용하며 요청/응답을 필터링
    - 캐시: 특별한 종류의 프락시 서버로, 더 빨리 문서를 받을 수 있게 자주 찾는 문서들의 사본을 저장
    - 게이트웨이: 다른 서버들의 중개자, HTTP 트래픽을 다른 프로토콜로 변환
    - 터널: 데이터를 그대로 전달해주는 SSL 트래픽을 HTTP 커넥션으로 전송하는 등 (이해못함)
    - 에이전트: 웹 요청을 만드는 모든 애플리케이션

## 2. URL과 리소스

- URI: 인터넷의 리소스를 가리키는 표준이름으로 여기서는 URL과 같이 사용한다.
- **URL은 애플리케이션이 리소스에 접근할 수 있는 방법을 제공하며, 리소스가 어디에 위치하고 어떻게 가져올지 정의한다.**
- URL 문법, 단축 URL, URL 인코딩과 문자규칙, 공통 URL 스킴, URN을 포함한 URL의 미래
- URL 구성: `스킴://서버위치/경로`
    - 첫 부분(http)은 스킴으로, 웹 클라이언트가 리소스에 어떻게 접근하는지 알려준다.
    - 두 번째 부분(www.abc.com)은 서버의 위치로, 리소스가 호스팅 된 위치를 알려준다
    - 세 번쨰 부분(/index.html)은 리소스 경로로, 서버의 리소스들 중 요청받은 리소스를 알려준다.
- URL 문법: `스킴://사용자이름:비밀번호@호스트:포트/경로;파라미터?쿼리#프래그먼트`
    - 스킴: 어떤 프로토콜을 사용하여 접근해야 하는지
    - 사용자이름, 비밀번호: 몇몇 스킴은 리소스 접근을 위해 필요로 한다.
    - 호스트, 포트: 서버 호스트명 또는 IP 주소 + 호스팅 서버가 열어둔 포트번호
    - 경로: 리소스가 서버의 어디에 있는지, 자체 파라미터를 가질 수도 있다.
    - 파라미터: 정확한 요청을 위한 키와 값의 쌍을 `=`로 전달하며 다른 부분들과는 `;`로 구분
    - 질의문자열(쿼리): 다른 부분과는 `?`로 구분하며, 요청하는 리소스 형식의 범위를 좁히기 위해 전달
    - 프래그먼트: 리소스의 특정 부분을 가리킬 수 있도록 `#` 문자로 구분하여 특정 이미지나 일부분을 문자열로 지정하면 프래그먼트 컴포넌트 부분은 서버에 보내지 않고 클라이언트에서만 해당 부분을 보여준다.
- 단축 URL
    - 상대 URL: URL을 짧게 표기하는 방식, 기저(base) URL을 기준으로 탐색된 경로이며 기저 URL은 `<BASE>` 태그로 명시하거나, 현재 리소스 위치를 base로 사용.