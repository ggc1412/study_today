#### Google api node-js



##### google api 라이브러리를 사용해보자.

google api 편하게 사용할 수 있도록 미리 세팅이 된 라이브러리가 있다.
https://developers.google.com/api-client-library

그런데 라이브러리를 설치하고 import하여 실행하는 순간 오류가 발생한다.

```shell
./node_modules/googleapis-common/build/src/http2.js
Module not found: Can't resolve 'http2' in 'C:\..생략..\node_modules\googleapis-common\build\src'
```

http2 모듈을 못 찾는다는 건데, http2는 nodejs 설치 시에 core에 같이 설치 되는 것이기 때문에  설치가 안되어 있을 수가 없다. 실제로 확인해보아도 정상작동 확인 가능하다.

```shell
node -p http2
```



##### 해결을 위한 삽질

http2 모듈을 설치해보라는 댓글도 있으나, http2 모듈을 따로 설치시 의존성에 문제가 생기는지 다른 모듈들이 깨진다. 해결하려고 깨진 모듈을 하나하나 다시 설치하면 다시 원점으로 돌아와서 http2 모듈을 찾을 수 없다는 오류가 발생한다.



##### 그럼 왜 오류가 발생하는가?

오류 메시지를 검색해보았을때 다음과 같은 페이지를 찾을 수 있었다.
https://github.com/googleapis/nodejs-dialogflow/issues/314

> aha! I should have read a bit closer, because of the `node-pre-gyp` dependency, I don't believe you will be able to include this in the frontend react component -- I believe it might be webpack generating these error messages.
>
> I think your best bet at this time would be running a service that the react application interacts with, which in turn runs `dialogflow`.

웹팩이 문제이고, 리액트와 호환가능한 서비스를 이용하라고 한다. (맞나?)

실제로 웹팩(정확히는 react-scrips)를 사용하지 않고 node 내에서만 실행한 결과, 정상 실행되는 것을 확인 할 수 있다.

```shell
node .\src\api.js
```



##### 그럼 어떻게 해결할 것인가?

실제로 리액트에서 gapi를 사용하는 예를 찾아보면, 모듈로 import 하지 않고, JavaScript 처럼 script태그안에 넣어서 불러오는 것을 볼 수 있다.

```javascript
  loadYoutubeApi() {
    const script = document.createElement("script");
    script.src = "https://apis.google.com/js/client.js";

    script.onload = () => {
      gapi.load('client', () => {
        gapi.client.setApiKey(API_KEY);
        gapi.client.load('youtube', 'v3', () => {
          this.setState({ gapiReady: true });
        });
      });
    };
 //출처 : https://gist.github.com/mikecrittenden/28fe4877ddabff65f589311fd5f8655c 
```

하지만 아래 댓글을 확인해보면, onload로 이벤트를 걸었음에도 gapi를 바로 사용할 수 없는 이슈가 발생하는 것을 알 수 있다. 그래서 그 해결책으로 load 되는 것을 0.1초마다 확인하는 함수를 제시하였다.

```javascript
loadClientWhenGapiReady = (script) => {
    console.log('Trying To Load Client!');
    console.log(script)
    if(script.getAttribute('gapi_processed')){
      console.log('Client is ready! Now you can access gapi. :)');
      if(window.location.hostname==='localhost'){
        gapi.client.load("http://localhost:8080/_ah/api/discovery/v1/apis/metafields/v1/rest")
        .then((response) => {
          console.log("Connected to metafields API locally.");
          },
          function (err) {
            console.log("Error connecting to metafields API locally.");
          }
        );
      }
    }
    else{
      console.log('Client wasn\'t ready, trying again in 100ms');
      setTimeout(() => {this.loadClientWhenGapiReady(script)}, 100);
    }
```

그런데 이렇게 많은 코드를 쓰면서까지 라이브러리를 써야하는 것일까.



##### 결국은 REST 호출

결국 fetch나 axios 등을 이용하여 api 호출을 하는 것이 제일 간편하다는 것을 깨달았다.  gapi가 restful 방식으로 잘 만들어져 있기 때문에 굳이 라이브러리의 메소드를 사용하지 않고도 충분히 원하는만큼 사용이 가능했다. google client 라이브러리는 더이상 업그레이드가 없을 것이라 하였기 때문에, 특별히 문제가 발생하지 않는 한 앞으로도 리액트에서 gapi를 쓸 때는 axios를 이용할 것 같다. 



##### **첨부. 기타 의견

*Mock관련 문제가 있어서, 사용이 안된다는 의견도 있었다.
https://github.com/facebook/create-react-app/pull/5686
하지만 이에 대해 test문제는 아니고, 브라우저에서 해당 모듈에 HTTP2를 지원하지 않기 때문이라는 것으로 종결되었다. (의역)

> This is probably as "temporary" as it gets, it papers over a problem that really lies in the usage of certain modules -- as HTTP2 is not implemented in the browser, if you're using a module that depends on it, either (a) you should not be using it or (b) the module authors themselves should be taking steps to making their module web-compatible, whether that means conditionally requiring `http2` or something more involved.



*Mock

의존성이 강한 경우에 단위 테스트가 어렵기 때문에 이를 돕기 위해 쓴다.
실제 객체를 만들어 사용하기에 시간, 비용 등의 Cost가 높거나 혹은 객체 간의 의존성이 강해 구현하기 힘들 경우, 가짜 객체를 만들어 사용하는 방법이다.

<img src="https://k.kakaocdn.net/dn/0jefm/btqwO7gyMd4/mkwx6tsab9MzVKfi5B9tVK/img.png" alt="img" />



