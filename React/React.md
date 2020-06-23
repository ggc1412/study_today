### JavaScript API

**map**은 fun에서 return한 값으로 배열을 반환
**filter**는 fun에서 true인 값으로 배열을 반환
**forEach**는 각 값에 대해 fun을 실행
**reduce**는 각 값에 대해 누산 계산이 가능, 콜백 반환값을 누적한다.
	빈 배열에서는 에러가 나므로 초기값을 주는 것이 더 안전하다.



### REACT API

**Router**
Path와 Component를 연결해준다.
어떤 Path에서 어떤 Component를 사용할지 지정.

**HashRouter / BrowserRouter** 
router 종류. 
Hash의 경우 Hashtag( # )가 붙는다. ( URL의 hash portion을 사용 )
BrowerRouter는HTML5 history API를 사용한다.

**Composition** 
두 개 이상을 동시에 render하여 보여주기 ex) header랑 footer
**Redirct** from to ~한 경우에 ~로 redirct해라
**Switch** 하나의 Component만 Render해라
**Link** 해당 페이지가 내 어플리케이션에 있으면 JS방식으로 이동함



### css 접근법

1. css 파일을 생성하고 index.js에 import

   - 이 경우, css파일과 js파일이 별개로 생성된다.
   - component를 만드는 이유는 어플리케이션 부분부분을 캡슐화 하는 것이다.

2. componenet 별로 폴더를 만들고, 그 안에 index.js 파일, js 파일, css 파일을 넣는다. 

   - 이 경우 폴더 채로 상위 App.js에 import 해준다.
   - 그럼 App.js에서는 각 폴더의 index.js파일을 우선적으로 읽는다.
   - css는 여전히 글로벌하게 적용이 된다. 
   - 때문에 css 클래스 이름을 기억해야하고, 중복되지 않도록 해야한다.
   - 또한, 매번 파일을 만들고, import 해줘야 한다. 

3.  css 모듈을 사용한다.

   - create-react-app으로 설정한 프로젝트에서는 css이름.modules.css 으로 css파일 이름을 지정하여 class처럼 사용이 가능하다.

   - import styles from "위치/css이름.modules.css" 로 객체 생성하듯 만들고, className에 {styles.클래스이름}을 적어준다. 
   - 이 경우, className을 임의화(randomize)하여  css 적용이 local화 되게 한다
   - 각 Component 별로 class 이름을 구별되게 생성(randomize)해주기 때문에 이름이 중복되지 않게 해준다.
   - 인스타그램이 이런 방식을 사용한다.
   - sass를 사용할 때도 확장자만 sass로 변경하여 사용가능하다.
   - 하지만 여전히 js와 css는 별도이고, className을 기억해야한다.

4.  styled-components를 사용한다.

   - npm에서 styled-components를 설치
   - 이러면 style이 포함되어있는 component를 생성할 수 있다.
   - HTML 태그에 원하는 style 을 오버라이드(?)한 컴포넌트를 생성하여, HTML 태그 대신에 사용한다.

   ```react
   import styled from "styled-components";
   const List = styled.ul`
   	display: flex;
   	&:hover{
   		background-color: blue;
   	}`;
   const Item = styled.li``;
   // 생략
   <List>
   	<Item>
       	<a href="/"></a>
       </Item>
   </List>
   ```

   - react의 component에도 style을 지정할 수 있다.

   ```react
   import { Link } from "react-router-dom";
   const Item = styled.li``;
   const SLink = styled(Link)``;
   // 생략
   	<Item>
       	<SLink to="/"></SLink>
   	</Item>
   ```

   - local로 각 component에 적용이 된다.



#### styled-reset

- npm i styled-reset
- styled-component에서 쓸수 있는 reset - css



#### style props

- style-component에 props를 만들어 줄 수 있다.
- ${} 태그 안에서 값을 받을 수 있다.

```react
import styled from "styled-components";
const Item = styled.li`
	border-bottom: 5px solid ${props => props.current? "#색상": "transparent"}
`;
// 생략
	<Item current={true}></Item>
// current라는 prop를 만들어서 사용
// component 안에 JavaScript 코드를 쓰려면 {} 사용
```



#### withRouter

- react-router-dom의 component
- 다른 component를 감싸는 component이다.
- Router에 정보를 받아 올 수 있다.

```react
const Header = (props) => {
    // 내용
}
export default withRouter(Header);
```

- 위에서는 withRouter로 감쌌기 때문에 props를 받아올 수 있다.
- Router에서 받아오는 정보는 history, location, match이다.
- location에서 받아온 props를 이용해 css 설정하는 예시

```react
import styled from "styled-components";
const Item = styled.li`
	border-bottom: 5px solid ${props => props.current? "#색상": "transparent"}`;

export default withRouter(({location: { pathname }}) => (
    // 생략
    <Item current={ pathname === "/" }></Item>
));
```



#### Each child in a list should have a unique "key" prop.

- 모든 React element는 유일해야 하는데 list 안에 들어가면 유일성이 사라진다.
- 따라서 구별 가능한 key값이 필요하다.



#### ProbTypes

-  전달 받은 props가 적절한 props인지 확인해준다.
- 미리 props를 설정해 놓고 적절하지 않은 props가 전달되면 콘솔에서 에러메시지를 띄워준다.
- 이를 통해 에러를 확인할 수 있다. 

```react
import PropTypes from "prop-types";

// 항상 propTypes로 불러야 읽을 수 있다.
Component.propTypes = {
    name: PropTypes.string.isRequired,
    picture: PropTypes.string,
    rating: PropTypes.number.isRequired
    // Component의 prop들에 type을 미리 지정해놓는다.
};
```



#### function component / class component

```react
function App() { 
    // function component 
    // return 값으로 render한다.
}

class App extends React.Component{
    // class component
    // state 등 다양한 것을 미리 가지고 있다.
    // component를 만들 때마나 모든 것을 구현하는 것이 아니라
    // React.Component에서 상속받아서 사용한다.
    // render메서드를 가지고 있어서 이걸로 render한다.
    // React는 자동적으로 class component의 render 메서드를 실행
    // state를 사용할 수 있다.
}
```



#### state

- object이다.
- component의 변동 data를 다룰 저장한다.
- 불러올 때는 this로 불러온다. ( class에 접근하기 위해 )

```react
class App extends React.Component{
    state = {
        count: 0
    };
	render() {
        retrun (
            <div>
            	<h1>{this.state.count}</h1>
        		<button onClick={this.add}>Add</button>
            <!--react에서는 HTML태그에 onClick을 바로 쓸 수 있다.-->
            </div>
        )
    }
}
```

- state의 값 변경은 setState() 메서드를 사용해야 한다.
- state의 상태 값을 변경할 때 react가 render메서드를 다시 불러와서 값을 갱신해야하는데 직접 값을 변경하면 react는 render를 refresh 하지 않기 때문이다.
- 하지만 이 경우 모든 부분을 새로 render 하는 것이 아니라, 변경 부분만 reder한다. ( react의 virtualDOM과 관련한 부분 )

```react
add = () => {
    this.state.count = 1
    // 이런 식으로 직접적인 변경은 안된다.
	this.setState({ count: 1 });
    // setState로 state를 다시 생성해줘야한다.
    // state는 object이기 때문에 {}로 넣어준다.
};
```

- state 값을 갱신할 때. 현재 값을 가져오기 위해서는 updater 인자를 사용한다.

```react
// object
state = {
    count: 0
};

// method
add = () => {
    // 다시 state를 불러오는 방식은 바람직하지 않다.
    this.setState({
        count: this.state.count + 1
    })
    this.setState( current => ({
        count: current.count + 1
    }))
};
```

- https://ko.reactjs.org/docs/react-component.html#setstate
- setState는 updater인자를 가지고 있으며 이는 ( state, porps )로 구성되어 있다. 

```react
(state, props) => stateChange
// state는 현재 state에 대한 참조이다.

// props.step만큼 값을 변경하고 싶다면 아래의 예를 따른다.
this.setState((state, props) => {
    return { counter; state.counter + props.step };
});
```



#### Mounting

- 생성
- Cunstroctor로 생성
- render()
- componentDidMount() 처음 rener 되었을때 실행됨



#### Updating

- setState 등으로 props나 state를 변경하였을때
- getDerivedStateFromProps()가 실행되고, shouldComponentUpdate()가 실행됨
- 다음 render()
- 그리고 componentDidUpdate()



#### Unmounting

- 제거
- page를 바꿀 때, component가 제거된다.
- state로 component를 교체할 때 제거된다.
- componentWillUnmount()가 실행됨



```react
class App extends React.Component{
    state = {
        isLoading: true,
        Movie: []
    };

    Loading = () => {
        this.setState({
            isLoading: false
        })
    };

    render(){
        const { isLoading } = this.state;       
        return (
            <div>
                { setInterval(this.Loading, 5000) }
                <h1>{isLoading ? "Loading..." : "We're ready!" }</h1>
            </div>
        );
    }
}
```

- render에 이 setInterval 메서드를 넣게 되면 re-render되면서 setInterval 메서드가 다시 실행된다.
  ( 즉, 무한 반복되게 된다. )

```react
    componentDidMount = () =>{
        setInterval(this.Loading,5000)
    }
```

- componentDidMount에 메서드를 넣으면 reder와는 상관 없기 때문에 ( re-render는 업데이트임 ) 한 번만 실행된다.



###  axios

- "/주소"는 절대 경로, "주소"는 상대경로

```javascript
const api = axios.create({
    baseURL:"URL",
    params:{
        api_key: "api key",
        language: "language"
    }
});
api.get("tv/popular")
```

- "tv/popular"의 api 값을 가져오고 싶을 때, 절대경로로 적으면 baseURL을 덮어쓰기 한다.
- 상대경로로 적어야 baseURL + "tv/popular" 경로에서 api를 가져옴
- encodeURIComponent : text string을 적절한 URI 컴포넌트로 변경
- 띄어쓰기나 특수문자 같은 URI에 적절하지 않은 값이 들어왔을때 이 값을 URI에 전달해줘야 하는 경우 encoding하여 전달해준다.



### Container Presenter Pattern

- Container가 data, state를 가지고 api를 불러와서 모든 로직을 처리
- Presenter는 data를 보여주는 역할을 한다. 
- Presenter는 state, api, class도 없고 그냥 함수형 component이다.
- Presenter는 style, Conatiner는 data.



childrend은 예약된 react prop





#### 분할 정복 기법

1. 메모리제이션 
   - 이전 계산값을 저장해놓았다가 재사용 
   - 탑-다운 방식

2. 상향식계산법 
   - 바텀-업 방식
   - 이것이 더 성능이 좋은 경우가 많다.





##### Component

- 리액트의 데이터는 props와 state를 사용
- props는 부모 component가 자식 component에게 값을 줌
- state는 component 내부에서 선언하여 값을 변경 가능
- 클래스 타입의 경우, {this.props.prop명}으로 사용

```react
class MyName extends Component {
  render() {
    return (
      <div>
        안녕하세요! 제 이름은 <b>{this.props.name}</b> 입니다.
      </div>
    );
  }
}
```



- 함수 타입의 경우, {prop명} = props로 받아서 사용한다.

```react
const MyName = ({ name }) => {
  return (
    <div>
      안녕하세요! 제 이름은 {name} 입니다.
    </div>
  );
};
```

- 둘의 차이점은 state와 LifeCycle이 빠져있다는 점이다.



##### state

- state는 동적인 데이터를 다룰 때 사용한다.
- setState를 통해서만 값을 생성할 수 있다.
- setState로 값을 변경하면 component가 리랜더링 된다.
- 기존 state는 변형(mutate)하지 않고, 새로운 값만 만든다. (불변성 유지)



##### state의 불변성 유지

- push, splice, unshift, pop은 기존 배열을 변형하므로 사용하지 않는다.
- 새 배열을 만들어내는 concat, slice, map, filter 같은 함수들을 사용한다.
- 이를 통해, API 내의 prevProps, nextState 같은 프로퍼티를 사용할 수 있다.



##### 배열의 랜더링시 key값 사용

- key값을 사용하여 성능을 최적화한다.
- 고유한 key값이 있다면 값을 추가하거나 변경할 때 변화를 최소화할 수 있다.

```react
// index를 key값으로 사용 시
<div key={0}>A</div>
<div key={1}>B</div>
<div key={2}>X</div> [C -> X]
<div key={3}>D -> C</div> [D -> C]
<div key={4}>D</div> [새로 생성됨]
```

```react
// 고유한 key값을 사용 시
<div key={0}>A</div>
<div key={1}>B</div>
<div key={5}>X</div> [새로 생성됨]
<div key={2}>C</div> [유지됨]
<div key={3}>D</div> [유지됨]
```

- 중간에 x값을 넣을 경우, 고유한 key값을 가진 경우가 훨씬 더 효율적이다.
- .



#### LifeCycleAPI



#### 초기 생성시

##### constructor

- component 생성자 함수로 component가 새로 만들어질 때마다 호출된다.



##### componentWillMount

- component가 화면에 나가기 직전에 호출되는 API 현재(v16.3 이후)는 쓸 일이 없다.



##### componentDidMount

- component가 화면에 나타나게 됐을 때 호출된다.
- D3, masonry처럼 DOM을 사용하는 외부 라이브러리 연동
- component에서 필요한 데이터 요청, Ajax, GraphQL 등
- DOM에 관련된 작업: 스크롤 설정, 크기 읽어오기
- 위와 같은 작업을 할 때 사용한다.



#### 업데이트시

- props의 변화, 그리고 state의 변화에 따라 결정된다.

##### static getDerivedStateFromProps()

- props로 받아온 값에 따라 state 값을 변경할 때 사용한다.

```react
static getDerivedStateFromProps(nextProps, prevState) {
  // 여기서는 setState 를 하는 것이 아니라
  // 특정 props 가 바뀔 때 설정하고 설정하고 싶은 state 값을 리턴하는 형태로
  // 사용됩니다.
  /*
  if (nextProps.value !== prevState.value) {
    return { value: nextProps.value };
  }
  return null; // null 을 리턴하면 따로 업데이트 할 것은 없다라는 의미
  */
}
```

##### shouldComponentUpdate

- 부모 component가 업데이트 되면, 자식 component도 업데이트 된다.
- 리랜더링이 불필요한 경우( 변화가 없는 경우 ), VirtualDOM에도 리렌더링 하는 것을 방지하기 위해 사용한다.



##### getSnapshotBeforeUpdate()

- shouldComponentUpdate를 대체한다.
- 다음 시점에 발생한다.
  1. render()
  2. **getSnapshotBeforeUpdate()**
  3. 실제 DOM 에 변화 발생
  4. componentDidUpdate
- 이 API를 통해서, DOM 변화가 일어나기 직전의 DOM 상태를 가져오고, 여기서 리턴하는 값은 componentDidUpdate에서 3번째 파라미터로 받아올 수 있게 된다.

```react
  getSnapshotBeforeUpdate(prevProps, prevState) {
    // DOM 업데이트가 일어나기 직전의 시점입니다.
    // 새 데이터가 상단에 추가되어도 스크롤바를 유지해보겠습니다.
    // scrollHeight 는 전 후를 비교해서 스크롤 위치를 설정하기 위함이고,
    // scrollTop 은, 이 기능이 크롬에 이미 구현이 되어있는데, 
    // 이미 구현이 되어있다면 처리하지 않도록 하기 위함입니다.
    if (prevState.array !== this.state.array) {
      const {
        scrollTop, scrollHeight
      } = this.list;

      // 여기서 반환 하는 값은 componentDidMount 에서 snapshot 값으로 받아올 수 있습니다.
      return {
        scrollTop, scrollHeight
      };
    }
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    if (snapshot) {
      const { scrollTop } = this.list;
      if (scrollTop !== snapshot.scrollTop) return; 
        // 기능이 이미 구현되어있다면 처리하지 않습니다.
      const diff = this.list.scrollHeight - snapshot.scrollHeight;
      this.list.scrollTop += diff;
    }
  }
```



##### componentDidUpdate

- componentDidUpdate(prevProps, prevState, snapshot) { }
- component에서 render를 호출한 다음에 발생한다.
- 파라미터를 통해 바뀌기 이전의 Props와 state 값을 조회할 수 있다.



#### 제거

##### componentWillUnmount

- 이벤트, setTimeout, 외부 라이브러리 인스턴스 제거



#### input 상태관리

```react
import React, { Component } from "react";

class PhoneForm extends Component {
    state = {
        name: '',
        number: ''
    }

    handleChange = (e) => {
        this.setState({
            [e.target.name]: e.target.value
            // computed property name(계산된 속성명)
            // [] 안에 식을 넣으면, 식의 결과가 속성명으로 사용된다.
        })
    }

    render() {
        return (
            <form>
                <input 
                    placeholder="이름" 
                    value={this.state.name}
                    onChange={this.handleChange} 
                    name="name"
                />
                <input 
                    placeholder="전화번호" 
                    value={this.state.number}
                    onChange={this.handleChange}
                    name="number" 
                />
                <div>{this.state.name}</div>
            </form>
        )
    }
}

export default PhoneForm;
```

- input을 2개 이상, 사용하는 경우 name 속성을 key값으로 사용하여 하나의 메서드에서 처리가 가능하다.
- 이때 key값의 계산은 Computed property name을 이용하여 처리할 수 있다.