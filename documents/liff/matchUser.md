## 유저를 매칭하는 법

- LIFF로 들어온 라인 유저와 서비스 유저를 매칭할 필요가 있는 경우가 있다.

### 기존 서비스의 유저를 매칭하지 않는 방법

- LIFF의 엑세스 토큰을 통해서 유저의 라인 아이디를 취득할 수 있다. 이 아이디를 서비스의 로그인 아이디로 하여 서비스의 유저를 만들 수 있다.
- 서버는 LIFF에서 토큰을 받아서 해당 토큰으로 아이디를 확인한 후 이미 등록된 라인 아이디인 경우 서비스에 로그인 되도록 만든다.
- 이 방법은 간단한 편에 속하는데 왜냐하면 기존에 서비스에 라인 아이디 없이 등록된 아이디와 라인 아이디를 매칭하는 단계 없이 서비스의 모든 유저 생성 방식이 라인을 기반으로 하기 때문이다.

### 기존 서비스의 유저를 매칭하는 방법

- LIFF의 단점은 라인 내부에서 실행된다는 것이다. LIFF로 접근한 유저의 아이디를 취득할 수 있으므로 LIFF 웹에서 서비스 유저의 아이디 비밀번호를 입력해서 로그인을 하는 방법이 있지만 아이디와 비밀번호에 대한 정보가 라인의 로그에 저장될 수 있다. LIFF는 라인 애플리케이션 내부에서 실행되는 웹이기 때문에 웹에서 일어나는 사항에 대해 정보를 취득할 수 있다. 따라서 비밀번호와 같은 민감한 정보는 어플 내부 브라우저에서 입력하지 않도록 하는 것이 좋고, 애플리케이션 내에서 실행되었을 때에는 입력하지 않도록 하는 것이 중요하다. 카카오톡, 인스타그램 등에서 웹을 열면 사파리나 크롬과 같은 브라우저에서 열리는 것이 어플리케이션 내부에서 열리는데 이는 사용자의 웹 접근 정보를 취득할 수 있기 때문이다.
- LIFF에서 아이디와 비밀번호를 입력하지 않는다면 어떻게 기존 서비스의 유저 아이디와 라인 아이디를 매칭할 수 있을까? LIFF에서는 로그인 아이디만 받고 비밀번호는 입력하지 않는 방법이 있다. 비밀번호를 입력하지 않는다면, 전화번호나 메일 등의 방식으로 인증을 해야 한다.

#### 서비스에서 QR 코드 제공

- 중요한 것은 기존에 서비스에 등록된 유저인지 확인하기 위함이다. 기존 서비스에 로그인을 한 뒤, 서비스 화면에서 로그인된 유저의 정보가 담긴 QR 코드를 보여주고 스마트폰으로 QR 코드를 읽어서 LIFF 앱을 여는 방법을 사용할 수 있다.
- QR 코드는 LIFF의 LIFF ID에 부여된 `https://liff.line.me`으로 시작하는 URL을 제공하되, 쿼리 스트링을 통해서 `?service_user_id=암호화된_유저_식별자`의 방식으로 QR 코드를 만들고 이 QR 코드를 읽은 유저가 LIFF의 웹을 라인에서 열면 LIFF의 토큰과 `암호화된_유저_식별자`를 서비스 서버에 보내어 라인 아이디에 매칭되는 서비스 유저 정보를 저장하는 방식을 사용할 수 있다.
- QR 코드를 활용한 방식은 별도의 스마트폰 이외의 모니터가 있을 때 인증이 가능하다. 서비스에 로그인 한 웹에서 발급된 QR 코드를 직접 읽어야 하기 때문이다. 그렇지 않으면, 웹에서 QR이 아닌 방법으로 `https://liff.line.me/라인_LIFF_식별자?service_user_id=암호화된_유저_식별자`의 URL을 복사하여 라인 채팅창에 붙여넣는 방식으로 인증을 할 수도 있다.

#### email이나 전화번호 인증

- 서비스 유저의 아이디를 입력하고 해당 서비스의 유저 정보에 등록된 이메일 또는 전화번호로 인증 번호를 발송하는 방법이다.
- 이 때 이메일 또는 전화번호는 개인 정보에 해당하기 때문에 최소한의 정보만을 보여주는 것이 중요하다. 메일의 경우 @이하의 메일 도메인만을 보여줘서 어떤 서비스의 메일로 전송하는지 정도만 알려주고 전화번호는 일부 정보만 보여줘도 추측하기 쉽기 때문에 일부 정보를 보여주지 않던가 최종 2,3글자의 번호만을 공개하는 방법이 있다. 또는 전화번호를 직접 입력을 해서 서비스에 등록되어 있는 번호인지 확인을 하는 방법도 사용할 수 있다. 만약 이 경우에는 무작위로 입력할 수 없도록 몇 회 이상 입력 시 다시 입력할 수 있는 시간을 부여하는 등의 방법을 사용해야 한다.
- 기존 아이디와 연동 -> 서비스 유저의 아이디를 입력 -> 이메일 또는 전화번호로 인증 선택창 -> 인증 번호 메시지 전송 -> `****@gmail.com`으로 메일을 보냈습니다. 또는 `***-****-**23으로 SMS를 송신했습니다.` 등의 메시지를 표기한다. -> 인증 번호를 입력하는 창을 보여준다. 이 때 인증 번호를 입력하는 제한 시간이 존재한다. -> 유저가 인증 번호가 입력 한다. -> 기존 서비스의 아이디와 연동되었습니다.

#### 라인에 등록된 정보를 통해 인증

- 라인에서 이메일에 대한 정보를 제공하기 때문에 기존 서비스에 등록된 이메일과 라인의 이메일이 일치할 때 인증을 하는 방법이 있다. 하지만 라인에 등록된 이메일이 아무런 인증 없이 등록한 이메일일 수 있기 때문에 별도의 인증방식을 추가해야 한다. 그렇다면 위에서 설명한 서비스 유저에 등록된 이메일로 메일을 보내면 되기 때문에 이 방식의 이점은 없다.

#### 웹 서비스에서 인증번호 발급

- 웹 서비스에 로그인 후 '라인 연동' 인증번호를 발급한다.
- 유저가 LIFF의 특정 화면으로 접근을 하여 인증 번호를 발급하는 곳에 발급된 번호를 넣고 인증한다.
- LIFF의 메뉴에서 '기존에 서비스에 등록된 아이디와 연동'이라는 메뉴를 추가한 후 이 페이지에 접근한 유저에게 서비스의 어떤 메뉴의 어떤 버튼을 클릭해서 인증 번호를 받을지에 관한 안내를 하는 페이지를 만든다. 그리고 인증 번호 입력 폼을 만들고 유저가 번호를 입력하면 인증이 되도록 만들도록 하자.