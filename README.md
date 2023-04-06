# 202030236 최용호
## React 강의 전용 Repository

---

### 강의 목록 
###### ※ 본 강의 목록은 *내림차순*으로 정렬되어있음.
1. [6주차](#6주차)
2. [5주차](#5주차)
3. [4주차](#4주차)
4. [3주차](#3주차)
5. [2주차](#2주차)
6. [1주차](#1주차)

---

## 6주차
### 2023.04.06 목요일
### 강의
##### 컴포넌트 추출
- 복잡한 컴포넌트를 쪼갯 여러 개의 컴포넌트로 나눌 수도 있음.
- 큰 컴포넌트에서 일부를 추출해서 새로운 컴포넌트를 만드는 것.
- 실무에서는 처음부터 1개의 컴포넌트에 하나의 기능만 사용하도록 설계하는 것이 좋음.
```js
function Avatar(props) {
    return (
        <img className="avatar"
             src={props.user.avatarUrl}
             alt={props.user.name}
        />
    );
}

function Comment(props) {
    return (
        <div className="comment">
            <div className="user-info">
                <Avatar user={props.author} />
                <div className="user-info-name">
                    {props.author.name}
                </div>
            </div>

            <div className="comment-text">
                {props.text}
            </div>

            <div className="comment-date">
                {formatDate(props.date)}
            </div>
        </div>
    );
}
```
- Comment는 댓글 표시 컴포넌트.
- 내부에는 이미지, 이름, 댓글과 작성일이 포함됨.
- 첫 번째 이미지 부분을 Avatar 컴포넌트로 출력해봄.
```js
function Comment(props) {
    return (
    <div className="comment">
        <UserInfo user={props.author} />
        <div className="comment-text">
            {props.text}
        </div>
        <div className="comment-date">
            {formatDate(props.date)}
        </div>
    </div>
);
}
```
- 추출 후 다시 결합한 UserInfo를 Comment 컴포넌트 반영하면 다음과 같은 모습이 됨.
- 처음에 비해서 가독성이 높아진 것을 확인할 수 있음.
```js
function UserInfo(props) {
    return (
        <div className="user-info">
            <Avatar user={props.user} />
            <div className="usr-info-name">
                {props.user.name}
            </div>
        </div>
    );     
}
```
##### State란?
- state는 리액트 컴포넌트의 상태를 의미함.
- 상태의 의미는 정상인지 비정상인지가 아니라 컴포넌트의 데이터를 의미함.
- 정확히는 컴포넌트의 변경가능한데이터를 의미함.
- State가 변하면 다시 렌더링이 되기 때문에 렌더링과 관련된 값만 state에 포함시켜야 함.
##### State의 특징
- 리액트 만의 특별한 형태가 아닌 단지 자바스크립트 객체일 뿐.
- 예의 LikeButton은 class 컴포넌트임.
- constructor는 생성자이고 그 안에 있는 this.state가 현 컴포넌트의 state임.
```js
class LikeButton extends React.Component {
    constructor(props) {
        super(props);
        
        this.state = {
            liked: false
        };
    };
}
```
※ *함수형에서는 userState()라는 함수를 사용함.*  
##### 생명주기에 대해 알아보기
- 생명주기는 컴포넌트의 생성 시점, 사용 시점, 종료 시점을 나타내는 것.
- constructor가 실행되면서 컴포넌트가 생성됨.
- 생성 직후 componentDidMount() 함수가 호출됨.
- 컴포넌트가 소멸하기 전까지 여러 번 렌더링 함.
- 렌더링은 props, setState(), forceUpdate()에 의해 상태가 변경되면 이루어짐.
- 그리고 렌더링이 끝나면 componentDinUpdate() 함수가 호출됨.
- 마지막으로 컴포넌트가 언마운트 되면 componentWillUnmount() 함수가 호출됨.
### 실습
Comment, CommentList 작성


---

## 5주차
### 2023.03.30 목요일
### 강의
##### 엘리먼트의 정의
- 리액트앱의 가장 작은 빌딩 블록들.
- 일반 객체이며, 쉽게 생성할 수 있음.
 
##### 엘리먼트의 생김새
- 리액트 엘리먼트는 자바스크립트 객체의 형태로 존재.
- 컴포넌트, 속성, 및 내부의 모든 children을 포함하는 일반 JS객체.
- 이 객체는 마음대로 변경할 수 없는 불변성을 갖고 있음.

##### 엘리먼트 렌더링  
- 아래의 코드는 리액트에 필수로 들어가는 아주 중요한 코드
- 이 div태그 안에 리액트 엘리먼트가 렌더링 되며, Root DOM node라 부름.
```jsx
<div id="root"></div>
```
##### 렌더링된 엘리먼트 업데이트
- 다음 코드는 tick()함수를 정의하고 있음  
- 이 함수는 현재 시간을 포함한 element를 생성해서 root div에 렌더링 해줌  
- 그런데 라인12에 보면 setInterval()함수를 이용해서 위에서 정의함 tick()을 1초에 한번씩 호출하고 있음  
- 결국 1초에 한번씩 element를 새로 만들고 그것을 교체하는 것  
- 다음 코드를 실행하고, 크롬 개발자 도구에서 확인해 보면 시간 부분만 업데이트 되는 것을 확인할 수 있음
```jsx
function tick() {
    const element = (
        <div>
            <h1>안녕, 리액트!</h1>
            <h1>현재 시간: {new Date().toLocaleTimeString()}</h1>
        </div>
    );

    ReactDOM.render(element, document.getElementById('root'));
}
```
##### 컴포넌트
- 2정에서 설명한 바와 같이 리액트는 컴포넌트 기반의 구조를 가짐.
- 컴포넌트 구조하는 것은 작은 컴포넌트가 모여 큰 컴포넌트를 구성하고, 다시 이런 컴포넌트들이 모여서 전체 페이지를 구성한다는 것을 의미.
- 컴포넌트 재사용이 가능하기 때문에 전체 코드의 양을 줄일 수 있어 개발 시간과 유지 보수 비용도 줄일 수 있음.
- 컴포넌트는 자바스크립트 함수와 입력과 출력이 있다는 면에서 유사함.
- 다만 입력과 출력은 입력은 Props가 담당하고, 출력은 리액트 엘리먼트의 형태로 출력됨.
- 엘리먼트를 필요한 만큼 만들어 사용한다는 면에서는 객체 지향의 개념과 비슷함.
##### Props의 개념
- props는 prop(property : 속성, 특성)의 줄인말.
- 이 props가 바로 컴포넌트의 속성.
- 컴포넌트에 어떤 속성, props를 넣느냐에 따라서 속성이 다른 엘리먼트가 출력됨.
- props는 컴포넌트에 전달할 다양한 정보를 담고 있는 자바스크립트 객체.
- Airbnb의 예도 마찬가지.
##### Props의 특징
- 읽기 전용.
- 속성이 다른 엘리먼트를 생성하려면 새로운 props를 컴포넌트에 전달하면 됨.
##### Pure 함수 vs. Impure 함수
- Pure 함수는 인수로 받은 정보가 함수 내부에서도 변하지 않는 함수.
- Impure 함수는 인수로 받은 정보가 함수 내부에서 변하는 함수.
##### 컴포넌트 추출
- 복잡한 컴포넌트를 쪼갯 여러 개의 컴포넌트로 나눌 수도 있음.
- 큰 컴포넌트에서 일부를 추출해서 새로운 컴포넌트를 만드는 것.
- 실무에서는 처음부터 1개의 컴포넌트에 하나의 기능만 사용하도록 설계하는 것이 좋음.
### 실습
Clock 예제 작성 및 실행


---

## 4주차
### 2023.03.23 목요일
### 강의
리액트 JSX 특징  
##### 장점
1. 코드가 간결해짐.
2. 가독성이 향상됨.
3. Injection Attack이라 불리는 해킹 방법을 방어함으로 보안에 강함.
##### JSX 사용법
1. 모든 자바스크립트 문법을 지원함
2. 자바스크립트 문법에 XML과 HTML을 섞어서 사용함
3. 아래 코드의 2번 라인처럼 섞어서 사용하는 것
4. 만일 html이나 xml에 자바스크립트 코드를 사용하고 싶으로 중괄호를 사용함
 ```jsx
const name = "소플";
const element = <h1>Hello, {name}</h1>;
```

### 실습
리액트 프로젝트 디렉토리를 Github 업로드 및 클론.  
Book.jsx, Library.jsx 작성

---

## 3주차
### 2023.03.16 목요일
### 강의
README 파일 작성 방법  
JavaScript의 라이브러리 React  
##### 리액트의 장점
1. 동기식(DOM), 비동기식(Virtual DOM)
2. 컴포넌트 기반 구조
3. 재사용성 - 반복적인 작업 ↓
4. 든든한 지원군 - 메타에서 지속적인 프로젝트 관리
5. 활발한 지식 공유 & 커뮤니티
6. 모바일 앱 개발가능 - 리액트 네이티브를 통한 Cross-Platform 개발
##### 리액트의 단점
1. 방대한 학습량 - 자바스크립트를 공부한 경우 빠르게 학습 가능
2. 높은 상태 관리 복잡도 - state, component life cycle 등의 개념이 있지만 어렵지 않음.
### 실습
리액트 프로젝트 디렉토리 생성  
리액트 예제(Add React in One Minute) 실행


---

## 2주차
### 2023.03.09 목요일
### 강의
자바스크립트의 역사  
ECMAScript 기준의 역사  
자바스크립트의 자료형  
자바스크립트의 함수
### 실습
깃헙 리포지토리 생성  
README.md 파일 커밋

---

## 1주차
### 2023.03.02 목요일
### 강의
오리엔테이션  
프로그램(Visual Studio Code, Github) 안내

---