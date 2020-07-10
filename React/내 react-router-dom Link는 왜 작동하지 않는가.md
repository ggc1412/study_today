#### 내 react-router-dom Link는 왜 작동하지 않는가?



##### 문제 발생

```react
<Link to={isMovie ? `/movie/${item.id}` : `/show/${item.id}`}>
```

영화 앱을 만들던 중, 콘텐츠 아이디 값에 해당하는 URL을 찾아갈 수 있도록 링크를 걸었다. 그런데 링크는 제대로 걸려 있는 것 같은데, 링크를 눌러도 아무런 반응이 없었다. 확인해보니 URL은 제대로 변경이 되는데, 해당하는 URL로 렌더링이 되지 않는 것이었다.



##### 잘 모를 땐 구글링을 해보자.

구글링을 해보니 이미 나같은 사람들이 꽤 많이 있었다! 
그런데 문제는 원인이 다양해서, 딱 내 코드에 맞는 것을 찾기가 힘들다는 것이었다.
다만 대부분은 route의 세팅 문제였고, 어쨌든 내가 react-router-dom을 제대로 모르고 있다는 것은 사실인듯 했다.



##### 그럼 내 문제는??

그러다 문득 든 생각. 
주소가 변했다는 것을 인식을 못하는 건가??

내 Router는 이렇게 설정되어 있었다.

```react
  <Router>
    <Switch>
      <Route exact path="/" component={Home} />
      <Route path="/tv" component={TV} />
      <Route path="/search" component={Search} />
      <Route path="/movie/:id" component={Detail}/>
      <Route path="/show/:id" component={Detail}/>
    </Switch>
  </Router>
```



그럼 테스트를 해보자.
원래는 "/movie/5235" 이런 식으로 받는 것을 링크 주소를 바꿔 보았다.

"/search"의 경우, 작동이 잘된다.
"/show/5235"의 경우, 작동이 안된다.

주소 변경은 인식한다. 그런데 so what?
"/movie/:id"와  "/show/:id"는 같은 컴포넌트를 렌더링하고 있다. 이것을 같은 것으로 생각하기 때문에 렌더링의 필요성을 못 느끼는 것이다. 
같은 건데 굳이 해야돼?? 이런 느낌.
결국 이게 다르다는 것을 Router에게 가르쳐줘야한다.
컴퓨터란 역시... ~~부지런하지만 멍청한 친구다.~~



##### 그래서 어떻게 고쳐야 하는데??

원인을 알았으니 이제 제대로 구글링을 할 수 있다. (??!!)
그리고 그 결과, 컴포넌트에 key값을 주면 다르게 인식하게 할 수 있다는 것을 알게 되었다.

>  If you do need a component remount when route changes, you can pass a unique key to your component's key attribute (the key is associated with your path/route). So every time the route changes, the key will also change which triggers React component to unmount/remount. I got the idea from [this answer](https://stackoverflow.com/questions/31813512/is-it-possible-to-only-remount-only-the-new-child-components-on-react-router-tra#answer-31816415)

출처: https://stackoverflow.com/questions/32261441/component-does-not-remount-when-route-parameters-change



오 이런 방법이!
그럼 코드를 고쳐본다.

```react
<Route path="/movie/:id" render={(props) => (
  <Detail key={props.match.params.id} {...props}/>
)}/>
<Route path="/show/:id" render={(props) => (
  <Detail key={props.match.params.id} {...props}/>
)}/>
```

작동이 잘된다. :)

그리고 좀 더 찾아보다가 놀라운 것을 발견했다.



> If the same component is used as the child of multiple `<Route>`s at the same point in the component tree, React will see this as the same component instance and the component’s state will be preserved between route changes. If this isn’t desired, a unique `key` prop added to each route component will cause React to recreate the component instance when the route changes.

https://reactrouter.com/web/api/Route/route-render-methods

무려 공식 문서 Route 항목에 정확히 동일한 부분에 대해서 설명을 하고 있었다.
( 다시 한 번 느끼는 공식문서의 위엄 ) 
위의 스택오버플로 댓글을 보면 알겠지만, 답변에 대해 Brilliant!! 이러고 난리가 났다.
~~여기나 저기(?)나 사용설명서 안읽어보는 건 똑같은가보다.~~ 

그리고 공식문서를 좀 더 살펴보고 수정한 최종 route 설정.

```react
<Router>
  <Switch>
{/* component, render, children 메서드를 사용하는 것은 hooks가 소개되기 전 버전을 위한 것으로 children elements를 사용한 방법이 더 권장된다. */}
    <Route exact path="/">
      <Home />
    </Route>
    <Route path="/tv">
      <TV />
    </Route>
    <Route path="/search">
     <Search />
    </Route>
    <Route path="/movie/:id" render={(props) => (
      <Detail key={props.match.params.id} {...props}/>
    )}/>
    <Route path="/show/:id" render={(props) => (
      <Detail key={props.match.params.id} {...props}/>
    )}/>
  </Switch>
</Router>
```





라우팅

리액트만으로도 라우팅을 할 수 있다.
특정 메뉴을 누름에 따라 렌더링하는 component를 바꿔주면 된다.
다만, 이 경우 몇가지 문제점이 생긴다.

- 즐겨찾기 등록 불가. URL이 동일하기 때문에 
- 뒤로가기 버튼을 누르면 이전 페이지가 아니라 아예 다른 웹 페이지로 이동
- 새로 고침 버튼을 누르면 사용중이던 컴포넌트가 아닌 최초 렌더링설정된 컴포넌트로 이동.

이 같은 문제들을 해결하기 위해, 표준처럼 사용하는 것이 React Router
앱에서 발생하는 라우팅이 location이나 history 같은 브라우저 내장 API와 완벽하게 연동이 된다.

React Router는 코어이고, react-router-dom은 코어를 dom으로 사용하는 라이브러리이다.

Link

a태그와 유사한 기능의 컴포넌트이다. to로 경로를 지정해준다.
클릭하면 주소창의 경로가 지정한 경로로 갱신된다.

Route

현재 주소창의 경로와 매치될 경우, 보여줄 컴포넌트를 지정하는데 사용된다.
path를 통해 매치시킬 경로를 지정하고 component를 통해서 보여줄 컴포넌트를 할당한다.
할당된 컴포넌트로 React Router는 **match, location, history **라는 3개의 prop을 넘겨준다.

```javascript
// match
{
    isExact: true
    params:{
    	id: "475430"
    }
    __proto__: Object
    path: "/movie/:id"
    url: "/movie/475430"
    __proto__: Object
}

// location
{
    hash: ""
    key: "rqdccl"
    pathname: "/movie/475430"
    search: ""
    state: undefined
    __proto__: Object
}

// history
{
  action: "PUSH"
    block: ƒ block(prompt)
    createHref: ƒ createHref(location)
    go: ƒ go(n)
    goBack: ƒ goBack()
    goForward: ƒ goForward()
    length: 36
    listen: ƒ listen(listener)
    location: {pathname: "/movie/475430", search: "", hash: "", state: undefined, key: "rqdccl"}
    push: ƒ push(path, state)
    replace: ƒ replace(path, state)
    __proto__: Object  
}
```

match.url은 Link 컴포넌트를 위해 사용되고, match.path는 Route 컴포넌트를 위해 사용된다.

- url은 실제 매칭된 문자열(ex."/movie/475430")
- path는 매칭에 사용된 경로의 패턴(ex."/movie/:id")



Route render methods

렌더링 방법으로 추천되는 것은 children 엘리먼트를 사용하는 것이나, hooks가 소개되기 이전 버전의 router를 위한 render 메서드들이 있다.

render 메서드에는  `component`, `render`, `children` 이 있다.

component는 React.createElement를 사용하여 새로운 React element를 주어진 Component에 만든다. 이 말은 component에게 inline function으로 주면, 매 render마다 새로운 component를 만든다는 뜻이다. 때문에 inline redering을 위해서는 render 메서드나 children 메서드를 사용하는 것이 좋다.	

render 메서드는 새로운 React 엘리먼트를 만드는 대신 you can pass in a function to be called when the location matches. (무슨 말이냐..?) 아무튼 inline 렌더링을 편하게 해준다.

children는 path가 location에 맞는지 여부를 확인하여 이에 따라 다르게 렌더링 해줄 수 있다.

component는 render, children보다 우선시되니 같이 사용하면 안된다.



인라인 함수

React에서 인라인 함수는 React가 렌더링 될 때, 정의되는 함수이다.
여기서 렌더링은 업데이트가 일어났을 때, 컴포넌트의 render 함수를 호출함으로서 React 엘리먼트를 컴포넌트에서 받아오는 것을 의미한다.





Router

Route와 Link가 유기적으로 동작하도록 묶어주는데 사용한다.




Switch

매치되는 제일 첫번째 컴포넌트만 보여주고, 그 이후에 나오는 컴포넌트는 무시한다.



중첩라우팅



v3에서는 history 객체를 직접 넣어줘야 했음.

v4부터는 Route가 독립적으로 사용가능, 그 말은 index에서 한번에 다 표시하는 것이 아니라 별도의 컴포넌트를 만들 수 도 있다는 뜻.



