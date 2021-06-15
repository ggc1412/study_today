#### **APPLE 로그인** 

##### 기본 설정

##### Finish Setting up Sign in with Apple

Depending on your product, you may need to configure multiple components for Sign in with Apple – From registering domains for Web Authentication to providing email sources to communicate with your users through the Private Email Relay service. [Learn more](https://developer.apple.com/sign-in-with-apple/get-started/)

1. Enable App ID
   - Description : BID
   - Bundle ID : kr.co.dwider.kdtf.브랜드prefix
   - EndPoint : https://srvapp.appdd.co.kr/kdtf/cust/doAppleUpdate.do
2. Create Service ID for Web Authentication
   - kr.co.dwider.kdtf .브랜드Prefix.service
   - 도메인 : srvapp.appdd.co.kr 
   - Return URLs : https://srvapp.appdd.co.kr/kdtf/cust/doAppleAuthentication.do
3. Create Key
4. Register Email Sources for Communication
5. DB에 키 등록
6. KEY 파일 업로드



1. App ID 등록
   - **Certificates, Identifiers & Profiles**
   - **' identifier '** 에서 **App ID** 등록
   - Register an App ID to enable your app, app extensions, or App Clip to access available services and identify your app in a provisioning profile. You can enable app services when you create an App ID or modify these settings later.
   - **EDIT**에서 **Endpoint URL** 설정 
   - 도메인 형식, 기본 443 포트, SSL 적용
   - '이메일 변경, 앱 서비스 해지, 애플 계정 탈퇴' 시에 Endpoint URL로 유저 정보와 이벤트에 대한 PAYLOAD 데이터를 전송
   - Sign in with Apple server to server notifications allow you to receive important updates about your users and their accounts. Notifications are sent for each app group when users change mail forwarding preferences, delete their app account, or permanently delete their Apple ID. Each group of apps can have one URL, which must be absolute and include the scheme, host, and path. TLS 1.2 or higher is required to receive notifications
   - https://kdtfapp.appdd.co.kr/kdtf/cust/endpoint
   - **Team ID**는 **Client Secret**을 생성할 때 필요한 정보
   - **KP97CFV8A7** (App IDs)
   - **co.dwider.kdtf** (Bundle ID)
2. Servies ID 등록
   - 애플 로그인을 진행한 유저의 정보를 전달받기 위해 필요
   - For each website that uses Sign in with Apple, register a services identifier (Services ID), configure your domain and return URL, and create an associated private key.
   - kr.co.dwider.kdtf.services
   - 설정
     - App ID 지정
     - 도메인, 리턴 URL 지정
     - https://kdtfapp.appdd.co.kr/kdtf/cust/doAppleAuthentication.do
   - client_id, aud 값으로 사용
3. Private Key 생성
   - T8CWX9MG4B
   - AuthKey_[KeyID].p8 파일 - Private Key 정보가 들어있음
   - client secret을 생성할 때 필요
4. Email Communication
   - private 처리된 유저의 이메일로 메일을 보내는 기능이 필요할 경우 사용

##### 구현 (Apple JS 버전)

1. Apple JS framework 넣기

   ```html
   <script type="text/javascript" src="https://appleid.cdn-apple.com/appleauth/static/jsapi/appleid/1/en_US/appleid.auth.js"></script>
   ```

2. Authorization Object 설정

   - 간단하게 하려면 default markup setup을 사용

     - meta tag 들을 <header>에 사용하여 설정

     - ```html
       <html>
           <head>
               <meta name="appleid-signin-client-id" content="[CLIENT_ID]">
               <meta name="appleid-signin-scope" content="[SCOPES]">
               <meta name="appleid-signin-redirect-uri" content="[REDIRECT_URI]">
               <meta name="appleid-signin-state" content="[STATE]">
               <meta name="appleid-signin-nonce" content="[NONCE]">
               <meta name="appleid-signin-use-popup" content="true"> <!-- or false defaults to false -->
           </head>
           <body>
               <div id="appleid-signin" data-color="black" data-border="true" data-type="sign in"></div>
               <script type="text/javascript" src="https://appleid.cdn-apple.com/appleauth/static/jsapi/appleid/1/en_US/appleid.auth.js"></script>
           </body>
       </html>
       ```

     - 

   - 아니면 JavaScript 로 설정한다.

     - ```html
       <html>
           <head>
           </head>
           <body>
               <script type="text/javascript" src="https://appleid.cdn-apple.com/appleauth/static/jsapi/appleid/1/en_US/appleid.auth.js"></script>
               <div id="appleid-signin" data-color="black" data-border="true" data-type="sign in"></div>
               <script type="text/javascript">
                   AppleID.auth.init({
                       clientId : '[CLIENT_ID]',
                       scope : '[SCOPES]',
                       redirectURI : '[REDIRECT_URI]',
                       state : '[STATE]',
                       nonce : '[NONCE]',
                       usePopup : true //or false defaults to false
                   });
               </script>
           </body>
       </html>
       ```

   - 메타 tag와 Javascript 두 개를 섞어서 사용할 수 있다. 

   - 초기 설정은 meta tag를 따르지만 JavaScript APIs를 사용하여 override 할 수 있다.

3. Authorization Response 처리

   - 버튼을 클릭하면 framework는 authorization 정보를 Apple 서버에 보낸다.

   - Apple 서버에서 요청을 처리하고, redirectURI로 응답을 보낸다.

   - 정상 응답의 경우, code / id_token / state / user 파라미터를 보낸다.

     - code : 5분간 유효한 인증 코드
     - id_token : 유저 식별 정보를 포함한 JSON web token
     - state : init 함수를 통과한 state
     - user : scope에서 요청한 유저 정보. JSON 타입

   - user 파라미터의 경우, 첫 인증 시에만 보내준다. 

   - 에러의 경우, error / state 파라미터를 보낸다.

     - error : error 코드
     - state : The state passed by the [`init`](https://developer.apple.com/documentation/sign_in_with_apple/authi/3230945-init) function.

   - Currently, the only error code returned is `user_cancelled_authorize`. The framework returns this error code when the user clicks the Cancel button.

   - pop-up 옵션을 사용하면, Apple 서버에서 인증 처리 후, 결과 값을 DOM 이벤트로 보낸다. 

   - ```javascript
     //Listen for authorization success
     document.addEventListener('AppleIDSignInOnSuccess', (data) => {
          //handle successful response
     });
     //Listen for authorization failures
     document.addEventListener('AppleIDSignInOnFailure', (error) => {
          //handle error.
     });
     ```

   - signIn 함수는 인증 처리가 되면 promise 객체를 반환한다.

   - ```javascript
     try {
          const data = await AppleID.auth.signIn()
     } catch ( error ) {
          //handle error.
     }
     ```

   - 성공 시 반환 객체는 다음과 같다.

   - ```javascript
     {
          "authorization": {
            "state": "[STATE]",
            "code": "[CODE]",
            "id_token": "[ID_TOKEN]"
          },
          "user": {
            "email": "[EMAIL]",
            "name": {
              "firstName": "[FIRST_NAME]",
              "lastName": "[LAST_NAME]"
            }
          }
     }
     ```

   - 실패 시 반환 객체는 다음과 같다.

   - ```javascript
     {
         "error": "[ERROR_CODE]"
     }
     ```

     

   - CSRF attacks????

##### Redirect 처리

Client가 인증을 통해 토큰을 가져오면 Backend에서 토큰을 가지고 고객 정보를 가져와 회원가입처리를 한다. Apple에서는 JSON Web Tokens를 주는데 payload 파트에서 필요한 고객정보를 얻을 수 있다. 따라서 추가로 정보 요청을 할 필요 없이 JWT가 변형없이 정상적으로 왔는지 검증만 하면 된다.

Identity Token과 Authorization Code를 받아온다.

[Identify Token]

1. Decoding
   - JWT형태로 오기 때문에 이걸 pasing 해야 한다.
   - BASE64로 디코딩하면 된다.
   - 토큰 검증을 위해 public key를 생성해야 한다. 
   - public key로 토큰 유효성을 검증한다.
2. Validation
   - state 검증
     - 우리가 처음 보낸  state와 돌아온 state가 동일한지 검증한다.
   - ID Token 검증 - JWT
     - Header
     - Payload
       - iss(issuer) : https://appleid.apple.com
       - aud(audience) : client_id
       - exp(expiration) : 현재 시각은 exp 전이어야 한다.
       - iat(issued at) : 토큰이 생성된 시간

[Authorization Code]

token을 발급받기 위해 code를 사용한다. 

1. client_secret 생성

   - alg : ES256
   - kid : Apple 개발자 페이지의 Key ID
   - iss : Apple 개발자 페이지의 Team ID
   - iat : 생성일자. 현재 시간으로 설정
   - exp : 15777000초 ( 6개월 )을 초과하면 안된다.
   - aud : "[https://appleid.apple.com](https://appleid.apple.com/)"
   - sub : App Bundle ID (com.xxx.xxx)
   - 서명 추가 / 이때 p8 파일을 사용한다.

2. 토큰 발급

   - https://appleid.apple.com/auth/token
   - Content-Type: application/x-www-form-urlencoded
   - code : Authorization Code
   - client_id : App Bundle ID (com.xxx.xxx)
   - grant_type : 인증코드 유효성 확인을 위한 것이면 authorization_code, refresh 토큰 유효성 검사면 refresh_token
   - client_secret
   - redirect_uri

   
   
   https://developer.apple.com/forums/thread/118135
   
   - 시행착오...
   - app 사용의 경우 client ID로 번들 ID를 사용하고, 웹 사용의 경우 서비스 ID를 사용한다.
   
   
   
   accss_token : (Reserved for future use) A token used to access allowed data. Currently, no data set has been defined for access.
   
   현재 사용 용도가 없다.
   
   refresh_token : The refresh token used to regenerate new access tokens. Store this token securely on your server. 
   
   

Content-Type: application/x-www-form-urlencoded

- json을 많이 사용하게 됨에 따라 요즘 request의 Content Type은 대부분 application/json인 경우가 많다. 아니면 파일 첨부를 위해 multipart/* 를 사용한다.
- 하지만 여전히 application/x-www-form-urlencoded 를 사용하는 경우가 있다.
- Content-Type은 request에 실어 보내는 데이터의 type정보를 표현한다.
- Text 타입으로 text/css, text/javascript 등이 있다.
- File 타입으로 multipart/formed-data가 있다.
- Application 타입으로 application/json, application/x-www-form-urlencode가 있다.
- 데이터 타입에 따라 적절하게 선택해주면 된다.



##### 애플 회원 탈퇴

- 아이폰에서 애플 ID 사용 중단 하였을 경우
- 노티 받도록 처리. 노티 받으면 로그아웃 시켜야함
- Processing Changes for Sign in with Apple Accounts
- https://developer.apple.com/documentation/sign_in_with_apple/processing_changes_for_sign_in_with_apple_accounts
- https://velog.io/@kkajjeim/%EC%95%A0%ED%94%8C%EB%A1%9C-%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EC%84%9C%EB%B2%84%EB%A1%9C-Apple-ID-%EC%97%B0%EB%8F%99-%ED%95%B4%EC%A0%9C-%EC%95%8C%EB%A6%BC-%EB%B0%9B%EA%B8%B0
- request 값으로 들어오는 param이 빈값.....
- 한번 기다려보고 다시 시도.
- requestParam과 requestBody의 차이점 확인하기!
- requestBody로는 받아짐.

endpoint 등록하기 항목의 설정 페이지에 등록된 endpoint 에는 총 4가지 타입의 event 가 전달된다.

- email-disabled : 유저가 이메일 수신을 중단했을 때
- email-enabled : 유저가 이메일 수신을 활성화했을 때
- consent-revoked : 유저가 Apple ID 연동 해제했을 때
- account-delete : 유저가 Apple ID 를 삭제했을 때

