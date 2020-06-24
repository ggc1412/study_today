- a태그

  - href="#아이디"
  - href="mailto:이메일주소"
  - href="tel:전화번호"

- list 태그 / ul, ol

  - 직계 자식요소로는 li만 사용할 수 있다.
  - div, a 등 다른 태그 사용 불가

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

- dl - Description List 정의 리스트

  - dl로 시작하고 닫는다.
  - dl의 자식요소는 div, dt, dd만 가능하다.
  - dt (Description term) - 이름 (key)값 설정
    - dfn - 정의라는 느낌을 더 강조할 때 사용
  - dd(Description data) - 설명(value)값 설정

  ```html
  <dl>
    <div>
      <dt>사과</dt>
      <dd>사과에 대한 설명</dd>
    </div>
    <div>
      <dt>바나나</dt>
      <dd>바나나에 대한 설명</dd>
    </div>
  </dl>
  ```

- 인용 Quotation

  - blockquote 태그를 사용하여 인용문을 정의한다.
  - 출처를 표시할 때는 cite 태그를 사용한다.
  - q 태그는 문단 안에서 인용문을 표시할 때 사용한다.

  ```html
  <blockquite cite="https://bit.ly/2mvSYrT">
    <p>
      <q>It's not happy news for the emperor penguin,</q> said Hal Castellan of
      the U.S. Woods Hole Oceanographic Institution, a co-author of the study in
      the journal Nature Climate Change.
    </p>
    <cite>
      “Emperor Penguin Population to Slide Due to Climate Change”, Scientific
      American, June 29, 2014, https://bit.ly/2mvSYrT
    </cite>
  </blockquite>
  ```

- div, span은 그룹핑하여 스타일링 할 때 사용하는 태그이다.

- Form 태그

  - input

    - type
    - placeholder / value
    - min / max
    - minlength / maxlength
    - required
    - disabled
    - accept - 유효한 확장자 설정

  - label

    - for="input태그 id"

  - radio / checkbox

    - label 태그로 값을 표시한다.
    - name 속성은 어떤 그룹에 해당하는지를 보여준다.
    - value 속성은 어떤 값인지를 나타낸다.
    - 서버에는 name=value 형태로 전달된다.

    ```html
    <div>
      <input type="radio" id="huey" name="drone" value="huey" checked />
      <label for="huey">Huey</label>
    </div>
    <div>
      <input type="radio" id="dewey" name="drone" value="dewey" />
      <label for="dewey">Dewey</label>
    </div>
    ```

- Table

  - thead / tbody - 이런 태그의 사용이 가독성과 시멘틱에서 더 좋다.
  - scope="row/col" - thead에만 쓸수 있는 것. col인지 row인지 좀 더 명확하게 보여주는 속성

- Media

  - audio

  - video
    - src
    - controls
    - autoplay / loop
    - audio 태그 안에 source 태그를 넣어서 source를 여러개 넣을 수도 있다.
    - 이 경우, 제일 상단의 소스가 재생이 안될 경우 순차적으로 시도함

- iframe

  - html 문서 안에 또 다른 html 문서를 넣을 때 사용
  - Youtube 같은 영상을 넣을 때 사용한다.

- abbr
  - abbreviation / 약어, 준말
  - title 속성을 통해 마우스 오버시 설명이 나오게 할 수 있다.
- address
  - 연락망이라는 것을 표시하는 태그
  - 주소, URL, email, 전화번호, SNS 등
- pre 태그, code 태그

  - preformatted / 텍스트를 미리 지정한대로 출력하도록 하는 태그
  - code의 일부분을 나타날 쓰는 태그
  - 들여쓰기를 표현하기 위해 pre 태그와 같이 쓰는 경우가 많다.

- title

  - 검색최적화를 위해 중요한 태그
  - 키워드 단순 나열은 비추
  - 페이지마다 타이틀이 다르게

- link / script

  - link는 비동기로 처리 / script는 동기로 처리
  - 그래서 script는 마지막에 넣는 것이 좋다.

- meta

  - name / 메타 데이터의 종류
  - content / 메타 데이터의 값
  - 요즘 중요한 메타 데이터는 viewport
    - name="viewport" content="width=device-width, initial-scale=1.0"
    - initial-scale / 처음 보여줄 때 배율
  - 그 외에, author / description / keywords

- html escape code / 헷갈릴 수 있는 문자를 대신 사용

- img 태그가 마크업상으로 아무의미가 없다고 느껴질 때는 img 태그 대신에 css 로 처리할 수도 있다.

- img 태그가 text 정보도 가져야 할 때(ex. 타이틀 로고 이미지) alt 속성으로 text도 함께 표현할 수 있다. 마크업상으로 브라우저가 img의 의미를 잘 파악할 수 있음.

- WAI-ARIA / 인터넷 접근성을 도와주는 API

  - aria-label
    - aria라는 스크린 리더로 읽었을 때는 다른 의미로 읽을 거야
    - 눈으로 보이는 것과는 다르게 읽어줘
  - aria-hidden="true" / 굳이 이거 읽을 필요 없어

- 중요도와 흐름에 따라 html 태그 순서를 배치할 수 있다. 추후 CSS로 보여지는 순서 재정렬

  - flex-direction: row-reverse;

flex

- flex-grow
  - flex-item 요소가 flex-container 요소 내부에서 할당 가능한 공간의 정도를 선언
  - 보통 flex-grow를 사용할 땐, flex-shrink, flex-basis 속성을 함께 사용
  - 일반적으로 모든 값이 설정되었음을 보장하기 위해 flex 속성으로 축약형을 사용
- flex-shrink
  - shrink / 수축
  - flex-item 요소의 크기가 flex-container 요소의 크기보다 클 때 사용
  - 설정된 숫자의 값에 따라 flex-item 요소의 크기가 **축소**
- flex-basis
  - flex-item의 초기 크기를 지정
- flex 속성 값이 한 개일 경우
  - number를 지정하면 flex-grow
  - length 또는 percentage를 지정하면 flex-basis
- flex 속성 값이 두 개일 경우
  - 첫번째 값은 number여야 하며, flex-grow가 됨
  - 두번째 값은 number일 경우, flex-shrink
  - length, percentage, auto를 지정하면 flex-basis
- flex 속성 값이 세 개일 경우
  1. flex-grow의 number
  2. flex-shrink의 number
  3. flex-basis의 length, percentage 또는 auto
- https://developer.mozilla.org/ko/docs/Web/CSS/flex

- 프론트 엔드 엘리먼트들

  - Feature Box / 상품 설명 같이 하나의 박스 형태로 존재하는 요소
  - Breadcrumb&Pagination / 페이지 위치 표시 요소 / 페이지 번호 표시 요소
