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
