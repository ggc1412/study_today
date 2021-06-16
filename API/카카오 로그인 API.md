# 카카오 로그인 API

## 개요

### 1. 로그인을 통해 두 가지 토큰을 발급받는다

- 카카오 api 호출 권한을 부여하는 **액세스 토큰 (Access Token)**

- 액세스 토큰을 갱신하는데 쓰는 **리프레시 토큰 (Refresh Token)**

### 2. 로그아웃 & 연결 끊기

- 로그아웃 - 로그인을 통해 발급 받은 사용자 토큰을 만료 처리

- 연결 끊기 - 카카오 계정과 앱의 연결을 끊는 기능. 연결 끊기를 하면 앱에서 더 이상 해당 사용자 정보로 API를 호출할 수 없고, 카카오 플랫폼에서도 해당 앱에 대한 해당 사용자의 데이터가 모두 지워짐

- 연결 끊기의 경우, 기존 이용 정보는 남아 있지 않으며 복구도 불가능

### 3. 연결 과정

- 사용자가 로그인을 요청하면 **인증 코드(Authorization Code)** 가 발급

- 이 코드는 앱 정보의 Redirect URI에 전달됨

- 앱은 전달받은 인증 코드를 기반으로 사용자 토큰을 요청하고 받음

- 인증코드로 **액세스 토큰**과 **리프레시 토큰**을 발급 받음

- 액세스 토큰은 사용자를 인증하고 카카오 API 호출 권한을 부여

- 리프레시 토큰은 사용자가 매번 카카오 계정을 입력하지 않아도 액세스 토큰을 발급 받을 수 있게함

- 요청 정보 문제로 로그인에 실패하면 카카오 서버는 실패 원인을 응답으로 전달함. 이때, 응답코드를 참고해 원인을 찾아 수정할 수 있음

- 토큰까지 발급받았다면 사용자 정보 요청을 통해 현재 로그인한 사용자의 정보를 확인하여 서비스에 활용할 수 있음

 ![img](https://developers.kakao.com/docs/latest/ko/assets/style/images/kakaologin/kakaologin_process.png)

 > 위에서 카카오 서버가 하나로 표현되어 있지만, 사실 카카오 서버는 인증 서버와 API 서버 두 가지로 나뉜다. REST  API 요청 시에도 카카오 로그인과 다른 API의 HOST가 다름을 알 수 있다. 카카오 로그인은 인증서버가, 사용자 정보 요청 등 다른 기능은 API 서버들이 각각 요청을 받고 작업을 수행한다.

### 4. 사용자 인증

- 카카오톡으로 로그인은 REST API를 활용할 경우 불가능. 이때는 계정 정보를 웹페이지에 입력해야 함

- JavaScript SDK의 경우 모바일에서는 카카오톡으로 로그인 가능

### 5. 로그아웃

- 사용자 토큰을 만료시켜서 카카오 API를 호출할 수 없게 함

- 서비스 회원 로그아웃은 자체 구현해야 함

- 카카오 계정과 함께 로그아웃 추가 기능을 사용하여 서비스와 카카오 계정을 동시에 로그아웃 처리할 수 있음.

- 이 경우 토큰 만료 방식이 아닌 서비스의 세션과 카카오 로그인의 세션을 만료하여 로그아웃 처리함.

- 이 기능을 사용하려면, 사용자가 로그아웃하고 리다이렉트될 Logout Redirect URI를 등록하고, 서비스 서버에서 서비스 세션을 만료시켜야 함

- REST API로 구현할 수 있음

- 같은 계정으로 여러 기기에 로그인한 상태라면, 로그아웃을 요청한 기기에서만 로그아웃이 됨

### 6. 연결 & 연결 끊기

- 연결이란 카카오 계정의 서비스 이용 상태를 의미함. 카카오와 서비스의 연결.

- 사용자가 서비스에 로그인할 때 발생하며, 연결을 통해 서비스는 사용자 토큰 기반의 API를, 사용자는 해당 서비스에서 카카오 API를 각각 이용할 수 있게 됨

- 연결은 사용자가 카카오 로그인 시 자동으로 이루어지는 것

- 연결 끊기는 서비스에서도 가능. ( REST API )

- 사용자는 서비스를 이용하고자 카카오 계정 정보를 제공한 것이므로, 사용자가 탈퇴하면 서비스에서 연결 끊기 처리를 해야함

- 연결 끊기는 사용자가 서비스에서 탈퇴하지 않아도 필요에 따라 호출할 수 있음

- 사용자가 카카오톡 설정의 연결된 서비스 관리 등을 통해 연결 끊기를 요청한 경우, 카카오에서 서비스 서버에 알림을 보내 연결 끊기 및 사용자 정보 처리를 요청함

  > 서비스 회원 가입은 각 서비스 회원 정보에 카카오 계정 사용자 정보를 회원으로써 저장하는 일. 연결과는 별개. 카카오 계정으로 로그인한 사용자 정보를 서비스 서버에 회원 가입 처리하지 않으면 정상적인 가입 처리가 되지 않음.

### 7. 토큰 관리

- 사용자 토큰은 매번 인증을 거치지 않고도 일정 기간 카카오 API를 사용할 수 있도록 하는 권한 증명

- 카카오 SDK는 사용자 토큰 관리 기능을 가지고 있음

- REST API 사용 시에는 필요에 따라 토큰 정보 확인이나 갱신을 위한 요청을 해야 함

- 사용자 토큰은 유효기간이 짧은 권한 증명용 액세스 토큰과 액세스 토큰을 갱신할 수 있게 해주는 리프레시 토큰. 2가지이다.
  - 액세스 토큰 유효 기간: 안드로이드, IOS (12시간),  JavaScript (2시간), REST API (6시간)
  - 리프레시 토큰 유효 기간: 2달 (1달 남은 시점부터 갱신 가능)
  - SDK 지원 범위.
    - 로그인, 로그아웃, 연결 끊기, 토큰 정보 보기, 토큰 갱신하기 ( 갱신은 JavaScript SDK는 지원 안함 )

## JavaScript SDK 사용하기

### SDK 초기화

1. SDK import

   ```html
   <script src="YOUR SDK FILE PATH"></script>
   ```

2. 앱 키 초기화

   ```javascript
   Kakao.init('JavaScript 키'); // 초기화
   Kakao.isInitialized(); // 초기화 확인
   ```

### 하이브리드 앱에 적용하기

- 하이브리드 앱 내 웹 페이지에서 JavaScript SDK를 사용할 경우, 카카오 SDK 가 올바르게 동작하도록하기 위한 웹뷰(WebView) 개발 방법

  > 하이브리드 앱
  >
  > 모바일 웹 + 모바일 플랫폼 네이티브 앱 (Android, iOS)
  >
  > 네이티브 앱을 통해 모바일 웹 기반으로 개발된 콘텐츠를 불러와 사용하는 방식으로 동작

- 카카오 로그인과 카카오 링크 사용 시 기술적인 한계로 인해 아래와 같은 처리가 필요
  - Custom URL에 의한 카카오톡 실행
  - 팝업 웹뷰 띄우기

### 카카오톡 실행하기

<https://developers.kakao.com/docs/latest/ko/getting-started/sdk-js#run-kakaotalk>

JavaScript SDK는 카카오 로그인이나 카카오링크 메시지 전송(공유)을 위해 카카오톡을 실행하는 URL을 만들어 호출한다. 상용 브라우저에서 해당 기능들이 실행될 때는 브라우저가 URL을 통한 앱 실행을 할 수 있어서 정상적으로 카카오톡이 실행되지만, 아래와 같이 처리하지 않은 웹뷰에서는 앱을 실행하지 못하고 에러가 발생하므로 추가 개발이 필요하다.

- Android

  Intent URI를 이용
  
  Javscript SDK가 카카오톡 실행을 위한 `Intent URI`를 만들어서 호출합니다.
  
  웹뷰에서는 [WebViewClient#shouldOverrideUrlLoading](<https://developer.android.com/reference/android/webkit/WebViewClient#shouldOverrideUrlLoading(android.webkit.WebView>, android.webkit.WebResourceRequest)) 메소드를 오버라이딩(Override)하여 `intent`를 파싱하고 해당 `Activity`를 실행해야 합니다.

- iOS
  
  iOS에서는 커스텀 스킴(Custom Scheme) 또는 유니버셜 링크(Universal Links)를 이용하여 앱을 실행합니다. 유니버셜 링크가 호출되었을 때는 별도의 처리 없이 앱 실행이 가능하지만, 커스텀 스킴이 호출된 경우 해당 URL을 웹뷰에서 `openURL` 메소드로 열어야 앱이 실행됩니다

### 팝업 웹뷰 띄우기

- 경우에 따라 팝업 윈도우를 사용한다. 플로우가 정상적으로 이루어지려면 `window.open()`, `window.close()` 호출에 맞춰 팝업에 해당하는 웹뷰가 생성 및 제거되어야 한다.
- Android
- iOS

### 로그인

*Kakao.Auth.authorize* 함수를 호출하면 카카오 로그인 동의 화면을 띄울 수 있다.
동의 화면에서 사용자가 '동의하고 시작하기' 버튼을 누르면 인증 코드가 발급된다.

인증 코드가 발급되면 서비스 서버의 Redirect URI로 로그인 요청이 Redirect 된다.
서비스 서버(Redirect URI)에서 인증 코드를 이용해 사용자 토큰 발급 요청을 하여 로그인을 완료한다.

> JavaScript SDK로 Kakao.Auth.authorize 함수를 호출할 때는 SDK 초기화 시 사용된 JavaScript 앱 키를 사용한다. 하지만 **REST API로 사용자 토큰 발급을 요청할 때는 REST API 키를 사용**한다.
> 카카오 로그인 후 필요한 회원가입 처리나 회원 정보 갱신은 서버에서 인증코드 및 사용자 토큰을 받은 후 구현해야 한다.

```javascript
Kakao.Auth.authorize(PARAMETER);
```

### PARAMETER

| Key         | Type    | Description                                           | Required |
| :---------- | :------ | :---------------------------------------------------- | :------- |
| redirectUri | String  | 인증코드를 받을 URI                                   | O        |
| state       | String  | 인증코드 요청과 응답 과정에서 유지할 수 있는 파라미터 | X        |
| scope       | String  | 추가 동의 받을 항목의 키                              | X        |
| throughTalk | Boolean | 간편 로그인 사용 여부 (default: true)                 | X        |

### 사용자 토큰 할당

로그인을 완료하여 토큰 값이 서비스 서버로 전달되면, 서비스 서버에서 사용자 토큰을 받아 사용자 정보 요청 등 카카오 API를 호출할 때 사용할 수 있다. 만약 로그인 이외에 카카오 API를 SDK를 통해 호출하려면 사용자 토큰 할당을 해야 한다.

```javascript
// 액세스 토큰 값을 SDK에서 사용할 수 있도록 설정
Kakao.Auth.setAccessToken(ACCESS_TOKEN);
```

### 로그아웃

사용자 토큰을 만료 시킨다.
*Kakao.Auth.logout* 함수를 호출하면 사용자 토큰을 만료시킬 수 있다.
로그아웃 성공 시, 콜백 함수를 통해 서비스의 로그아웃 로직을 수행하도록 구현한다.

```javascript
if (!Kakao.Auth.getAccessToken()) {
  console.log('Not logged in.');
  return;
}
Kakao.Auth.logout(function() {
  console.log(Kakao.Auth.getAccessToken());
});
```

### 연결 끊기

앱과 사용자의 연결을 끊기 위해 사용하는 함수이다.
*Kakao.API.request* 함수의 파라미터인 url에 '/v1/user/unlink'를 할당하여 사용한다.

> 서비스 탈퇴 처리는 연결 끊기 후 직접 구현해야 한다.

### Parameter

| Key     | Type             | Description                                       | Required |
| :------ | :--------------- | :------------------------------------------------ | :------- |
| url     | String           | 연결 끊기 API 사용 시 '/v1/user/unlink' 로 고정   | O        |
| success | Function(Object) | API 호출이 성공할 때 실행되는 콜백 함수           | X        |
| fail    | Function(Object) | API 호출이 실패할 때 실행되는 콜백 함수           | X        |
| always  | Function(Object) | API 호출 성공 여부에 관계 없이 항상 호출되는 함수 | X        |

```javascript
Kakao.API.request({
  url: '/v1/user/unlink',
  success: function(response) {
    console.log(response);
  },
  fail: function(error) {
    console.log(error);
  },
});
```

### Kakao.Auth.login

- 카카오 로그인 동의 화면을 팝업으로 띄우거나 클라이언트에서 모든 인증처리를 하고 싶은 경우 *Kakao.Auth.login* 함수를 사용할 수 있다.

### 인증코드 받기

- 카카오 로그인 동의 화면을 호출하고, 사용자 동의를 거쳐 인증코드 발급을 요청하는 API이다.
'동의하고 시작하기' 버튼을 누르면, 카카오 인증서버가 해당 사용자에게 인증 코드를 발급해 서비스의 redirect_uri에 전달한다.

- 인증코드 요청의 응답은 redirect_uri로 HTTP 302 Redirect 되며, Location에 인증 코드가 담긴 쿼리 스트링 또는 에러 메시지를 포함한다. 사용자가 취소 버튼을 클릭한 경우에는 에러 메시지를 담은 쿼리 스트링이 redirect_uri로 전송된다. 받은 인증 코드는 사용자 토큰 받기에 사용한다.

### 사용자 토큰 받기

- 필수 파라미터 값들을 담아 POST로 요청한다. 요청 성공시, 응답은 JSON 객체로 Redirect URI에 전달되며 두 가지 종류의 사용자 토큰 값과 타입, 초 단위로 된 만료시간을 표함하고 있다.

- 사용자 토큰은 사용자 정보 요청과 같은 사용자 기반 API를 호출할 때 사용한다. 사용자 정보 요청을 통해 필요한 사용자 정보를 받아 서비스 회원 가입 및 로그인 등을 처리한다.

- 토큰 정보 보기로 사용자 토큰 유효성을 검사하거나 토큰을 갱신할 수 있다.

### 토근 정보 보기

- 사용자 토큰을 헤더에 담아 GET 메서드로 요청한다.
- 응답은 JSON 객체로 토큰 상세 정보를 받는다.
- 응답 에러 코드가 -1이라면 카카오 플랫폼 서비스의 일시적 내부 장애로, 사용자 토큰을 강제 만료 또는 로그아웃 처리하지 않는 것이 좋다.
- 그외의 에러는 로그 아웃 처리를 권장한다.

### Response

| Key        | Type    | Description               |
| :--------- | :------ | :------------------------ |
| id         | Long    | 회원번호                  |
| expires_in | Integer | 액세스 토큰 만료 시간(초) |
| app_id     | Integer | 토큰이 발급된 앱 ID       |
