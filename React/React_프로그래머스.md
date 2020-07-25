#### Parcel로 프로젝트 구성하기

cra로 구성해도 문제가 없지만, webpack 기반으로 결국 webpack을 밖으로 빼면서 입맛대로 엄청바꾸게 된다. 하지만 Parcel을 이용하면 그런 부분에서 독립적이다.

##### prettier 설정

scripts로 미리 작성해놓고, 편하게 사용하면 좋다.
특히, prettierrc로 prettier를 설정하고, 코드 포맷 세팅을 같이 가져가는 것도 협업에 좋다.

vscode의 경우, Prettier:Require Config 설정을 통해 설정파일이 있을 경우에만 사용이 되도록 할 수도 있다. format on save는 저장할 때마다 적용되게 하는 옵션이다.

##### eslint 설정

JavaScript는 인터프리터 언어이기 때문에, 실행해야지만 오류가 있는지 알 수가 있다. 하지만 eslint를 이용하여 작성 중에도 문제가 있는지 확인할 수 있다.

설정은 eslint를 검색하여 create eslint configuration에서 몇가지 설정을 하면 기본 설정을 만들 수 있다.

prettier와 함께 쓰기 위해, eslint-config-prettier를 설치하여 충돌되는 룰들을 모두 비활성 시켜준다. ( eslintrc 파일에서 extends에 prettier를 추가해줘야 함 )

```json
"extends":["eslint:recommended", "prettier"]
```

##### Parcel 번들러 추가

import 등 모듈 시스템이나 번들링을 위해서 번들러가 필요하다.

```shell
yarn add -D parcel-bundler
```

Parcel 번들러에게 시작지점을 알려주어서 번들링할 수 있다.

```json
"scripts": {
 "dev":"parcel index.html",
 "build":"parcel build index.html"
}
```

build의 경우, dist라는 폴더를 만들어서 번들링된 파일들을 저장해준다.

#### React 설치

일반 eslint는 React 코드를 이해 못하므로 React용 eslint를 따로 설치해야 한다.

1. eslint 플러그인 추가

   ```shell
   yarn add -D eslint-plugin-import eslint-plugin-react
   ```

2) .eslintrc.json 수정

   ```json
   {
     "env": {
       "browser": true,
       "es2020": true
     },
     "extends": ["eslint:recommended", "prettier", "plugin:react/recommended", "prettier/react"],
     "parserOptions": {
       "ecmaVersion": 11,
       "sourceType": "module",
       "ecmaFeatures": {
         "jsx": true
       }
     },
     "plugins": ["react", "import"],
     "rules": {
       "no-console": "warn"
     },
     "settings": {
       "react": {
         "version": "detect"
       }
     }
   ```

3) 바벨 설치
   클래스 속성은 새로 추가된 스펙이기 때문에 바벨을 이용하여 사용할 수 있다. 기본적으로 parcel은 바벨을 사용하는데 우리가 바벨 설정을 오버라이드 해서 원하는 스펙을 추가할 수 있다.

   ```shell
   yarn add -D babel-eslint @babel/core @babel/preset-env @babel/plugin-proposal-class-properties @babel/preset-react
   ```

   ```json
   // .babelrc 설정
   {
     "presets": ["@babel/preset-react", "@babel/preset-env"],
     // preset-env에 plugin이 추가되어 있어서 따로 추가할 필요없다.
     "plugins": []
   }
   ```

   ```json
   // eslint에 추가
   {
     …
     "parser": "babel-eslint",
     …
   }
   ```

   바벨이 jsx 코드들을 javaScript로 바꿔준다. 그래서 jsx 코드들이 element가 된다. Component는 여러 element들을 묶어서 쓰는 것이고, 여기서 element들은 DOM element 같은 형태의 element들이다. ( 실제 DOM element는 아님. 리액트에서 만들어준 element )

   ```react
   // babel이 바꿔주는 jsx 코드
   export default class App extends Component{
       render() {
           return <div>Hello</div>
       }
   }

   // 바뀐 javaScript 코드
   render(){
       return React.createElement("div", {}, "Hello")
   }
   ```

   결국 App Component가 반환해주는 것은 React Element 이다.

   ```react
   ReactDOM.render(<App />, document.getElementById('root'));
   ```

   그리고 여기서 App 클래스를 알아서 실행해서 App클래스가 반환해주는 React Element를 DOM으로 render해주는 것이 ReadtDOM이다. ( Virtual DOM )

   element 객체를 가지고 변화가 있을 때 UI 전체를 Virtual DOM으로 렌더링 한다. 메모리 상에 존재하는 순수 JavaScript 데이터 구조로 reflow나 repaint 되는 것이 아니다. ( no reflow, no repaint )

   DOM은 조작할때마다 매번 reflow, repaint 되기 때문에 매번 조작하는 것보다 element 객체를 가지고 렌더링후 DOM은 한번에 변경하는 것이 좋다.

   여기서 React가 나머지는 다 해주고, 우리는 그려질 element만 전달해주면 된다.

#### React의 사용

퍼블리셔가 준 HTML 파일을 보고 어떻게 컴포넌트를 나눌지 고민해야한다. 재사용성이 최대한 높게 컴포넌트를 나눠야 하는데, 최대한 작게 나누는 것을 권한다.

##### 스타일링

JavaScript 안에 css를 포함(css in JS)하여서 한번에 컴포넌트화하여 따로따로 만들어 놓고 사용할 수도 있다. 미리 만들어 놓은 css 파일들이 있다면 굳이 css 까지 컴포넌트화하여 쓰지 않는다. 그때그때 회사의 스타일에 따라 사용할 수 있다.

공통 컴포넌트로 따로 빼놓고 사용한다면 이것들을 Monorepo, Storybook 이나 별도 프로젝트로 빼서 재사용이 가능한 독립적인 프로젝트처럼 만들어 쓸 수 도 있다. 많은 리액트 라이브러리들이 Storybook을 제공한다.
@ 디자인 시스템으로 공통 컴포넌트를 뽑기

##### propTypes

컴포넌트 안에서 prop들의 type을 지정한다. 모든 컴포넌트들에 대해서 굳이 만들 필요는 없고, 재사용되는 컴포넌트들에 대해서 쓰는 것도 권장된다. 아예 typeScript를 쓰는 것도 좋다.
