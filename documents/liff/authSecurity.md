## LIFF ID

- [LINE Developers](Developershttps://developers.line.biz/)의 LIFF 주소를 발급할 때 생성되는 아이디를 의미한다.
- 어떤 개발자의 계정에서 발급된 아이디인지 확인하는 용도로 사용한다.

## 엑세스 토큰

- 라인 유저의 사용자 정보를 취득할 수 있는 유저 식별 문자열이다.
- 엑세스 토큰은 라인 앱 내에서 LIFF가 열리거나 라인 아이디로 로그인 했을 때 LIFF SDK에서 반환하는 특정 유저를 식별할 수 있는 문자열을 의미한다.
- LIFF에서 발급된 토큰의 경우에는 LIFF 창을 닫거나 토큰을 발급한지 12시간이 지나면 만료되는 토큰이다. 토큰은 유저의 정보를 취득할 수 있는 권한이 있기 때문에 토큰이 유출되었을 때 유출된 토큰으로 유저 정보를 계속적으로 취득할 수 있다. 물론 유출이 되지 않게 관리하는 것이 가장 중요하지만, 혹시라도 유출이 되었을 경우에 만료 시간을 두어 토큰을 사용을 막기 위한 수단으로 타임 아웃 기간이 존재한다.
- 보통은 엑세스 토큰을 LIFF의 프론트앤드에서 취득을 한 후, 취득한 유저에게 어떤 메시지(공지사항, 이벤트 안내, 개인 스케쥴 안내, 서비스 알람 등)를 보내기 위해서 회사의 서버로 토큰을 보낸다. 회사의 서버는 이 토큰을 LIFF 프론트앤드에서 받아서 라인 서버의 API에 통신을 하고 사용자의 표시이름, 아이디, 이메일 주소 등의 정보를 얻고 이 정보를 데이터베이스 등에 저장한다. 라인 ID가 저장이 되었다면 해당 라인 아이디의 유저에게 메시지를 보낼 수도 있고, 해당 라인 아이디로 로그인 한 경우 별도의 로그인 정보 입력 없이 라인 정보를 통해서 로그인하게 만들 수도 있다.
- 엑세스 토큰으로 취득할 수 있는 정보는 사용자 ID, 표시 이름, 이메일 주소 뿐만 아니라 프로필 이미지도 취득할 수 있다. LIFF 애플리케이션에서는 LIFF 어플에 유저가 로그인 하거나 라인 앱으로 접속을 한 경우 엑세스 토큰 뿐만 아니라 유저의 정보도 취득할 수 있다. LIFF 애플리케이션에서는 엑세스 토큰으로 유저 정보를 취득하는 것이 아니다. 엑세스 토큰으로 유저 정보를 취득하는 것은 LIFF가 아닌 곳에서 유저의 정보를 취득할 수 있게 하려는 목적이 있다.
- LIFF는 기본적으로 프론트앤드이다. 프론트앤드에서 보내는 정보는 리퀘스트 송신자의 데이터 조작이 가능하다. 접속한 유저의 정보를 취득해야 하는데, 다른 유저의 정보로 바꾸어 보낸다면 다른 유저의 정보로 어떤 처리를 하게 될 것이다. 이런 문제점을 방지하기 위해서 유저가 다른 유저 정보로 바꿔칠 가능성을 차단할 수 있는 엑세스 토큰을 발급하는 방식을 사용한다. 엑세스 토큰은 매번 바뀌는 값이고, 한 번 발급된 엑세스 토큰도 LIFF 앱을 닫았을 때나 발급 후 12시간이 지난 후에는 만료되는 토큰이기 때문에 다른 엑세스 토큰으로 위조하기 상대적으로 어렵게 만들어 두었다. 따라서 어떤 유저에 대한 정보를 LIFF가 아닌 다른 애플리케이션으로 전송하고 싶다면, 유저 정보를 직접 보내는 것이 아니라, 엑세스 토큰을 보내고 애플리케이션 쪽에서 라인 서버에 엑세스 토큰을 보내고 유저 정보를 받아오는 방식을 사용해야 한다.