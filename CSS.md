- flex-direction: column;
- overflow
  - auto / hidden
  - white-space - 숨길 때 줄 바꿈 등을 어떻게 할거냐
  - text-overflow - text관련하여 처리 방법 설정 가능
- rgba / opacity 차이점 - 비상속, 상속
- styled-components 내에서 & 사용
- img 태그 가운데 정렬
  - inline-block이므로 text-align 사용가능
  - display:block으로 하여 margin 줄 수 있음
  - flex로 가운데 정렬 가능

### positioning

#### static

- 기본 배치
- [1_static-positioning.html](http://mdn.github.io/learning-area/css/css-layout/positioning/1_static-positioning.html)

#### relative

- 기본적으론 static과 동일하다.
- top, bottom, left, right 프로퍼티를 사용하여 위치 조정이 가능하다.
- [2_relative-positioning.html](http://mdn.github.io/learning-area/css/css-layout/positioning/2_relative-positioning.html)

#### absolute

- 독립적으로 배치된다.
- top, bottom, left, right 프로퍼티를 사용하여 위치 조정을 한다.
- [3_absolute-positioning.html](http://mdn.github.io/learning-area/css/css-layout/positioning/3_absolute-positioning.html)
- 조상 엘리먼트에 명시적으로 지정된 position이 없는 경우, the absolutely positioned element는 <html> 엘리먼트 밖에 위치하게 된다.
- 조상 엘리먼트에 명시적으로 지정된 position이 있다면 그 엘리먼트를 기준으로 배치된다.
- [4_positioning-context.html](http://mdn.github.io/learning-area/css/css-layout/positioning/4_positioning-context.html)

#### z-index

- 나중에 position된 엘리먼트가 먼저 position된 엘리먼트 위에 위치한다.
- 명시적으로 position되지 않는다면 ( static ) 명시적으로 position된 엘리먼트들 보다 아래에 위치하게 된다.
- 기본적으로는 0의 값을 가지며, 위로 올라올수록 숫자가 높아진다.
- [5_z-index.html](http://mdn.github.io/learning-area/css/css-layout/positioning/5_z-index.html)

#### fixed

- absolute와 동일하게 동작한다.
- 차이점은 absolute는 <html> 혹은 가장 가까운 position된 조상 엘리먼트를 기준으로 position된다면, fixed는 브라우저 뷰포트 자체를 기준으로 position된다.
- [6_fixed-positioning.html](http://mdn.github.io/learning-area/css/css-layout/positioning/6_fixed-positioning.html)

#### sticky

- 비교적 최신 position으로 relative와 fixed의 혼합이다.
- 기본적으로 relative position이지만 스크롤이 특정 위치를 넘어갈 때 ( 예, 뷰포트 top에서 10px )부터 fixed position으로 변하게 할 수 있다.
- 다른 sticky 엘리먼트가 오면 이전 sticky엘리먼트를 대체한다.
- [7_sticky-positioning.html](
