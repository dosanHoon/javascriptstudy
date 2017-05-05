# How to use Redux without webpack
* 종속관계가 아닌 컴포넌트간 STATE 공유
## What is Redux?
참고 : http://bestalign.github.io/2015/10/26/cartoon-intro-to-redux/
## js lib dependency
* redux lib import in html
    * redux.min.js
    * react-redux.min.js
## redux store를 상속받는 컴포넌트
### render with require
#### redux1.jsx
```jsx
require(['/babel/ko/reduxex/reduxstore.js'], //redux store import
    function () { 
    ReduxTest = connect( //ReduxTest 컴포넌트에 state와 dispatch 연결
        ListState,
        StateDispatch
    )(ReduxTest)

    ReactDOM.render(
        <Provider store={store}> //render시 provider 통해서 스토어 전달
            <ReduxTest />
        </Provider>
        , document.getElementById('redux1'));
})
class ReduxTest extends React.Component {
    constructor() {
        super(...arguments)
        this.state = {
            data:""
        }
    }
    handleRedux(){
        //redux action에 정의된 함수 호출
        this.props.adddata(this.refs.inputte.value) 
    }
    render() {
        return (
            <div>
                <h1>RedexTest 컴포넌트1</h1>
                <p>redux의 state data 를 prop 으로 받아옴</p>
                <p>{this.props.data}</p>//redux의 state data 를 prop 으로 받아옴   
                <p><input type="text" ref="inputte" placeholder="store에전달할값"/></p>
                <p><button onClick={this.handleRedux.bind(this)}>값전달하기</button></p>
            </div>
        )
    }
}
```
#### redux2.jsx
위와 동일함
```javascript
require(['/babel/ko/reduxex/reduxstore.js'], function (tab) {

        ReduxTest2 = connect(
            ListState,
            StateDispatch
        )(ReduxTest2)

        ReactDOM.render(
            <Provider store={store}>
                <ReduxTest2 />
            </Provider>
            , document.getElementById('redux2'));
    })


class ReduxTest2 extends React.Component {
    constructor() {
        super(...arguments)
        this.state = {
            data:""
        }
    }
    handleRedux(){
        this.props.adddata(this.refs.inputte.value)
    }
    render() {
        return (
            <div>
                <h1>RedexTest 컴포넌트2</h1>
                <p>redux의 state data 를 prop 으로 받아옴</p>
                <p>{this.props.data}</p>
                <p><input type="text" ref="inputte" placeholder="store에전달할값"/></p>
                <p><button onClick={this.handleRedux.bind(this)}>값전달하기</button></p>
            </div>
        )
    }
}


```
#### reduxstore.js
```javascript
var createStore = Redux.createStore; //Redux lib Store 정의
var Provider = ReactRedux.Provider; //Redux lib Provider 정의
var connect = ReactRedux.connect; //Redux lib connect 정의

var initialState = { //Redux Store 내 STATE
  data: ""
}

var reducer = function(state, action) { //reducer 실제 구현이 정의되는 부분
  if(state === undefined) {             //정의된 state와 action을 
                                        //인자로 받는다 
    return initialState;
  }
  var newState = state;
  switch(action.type) {
    case 'adddata':      
      newState = {data: action.data}
      break
  }
  return newState
}

var store = createStore(reducer, initialState)  
//react 컨포넌트에 전달한 store 생성

var ListState = function(state) {
  return {
    data: state.data
  }
}

var StateDispatch = function(dispatch) { //react 컴포넌트에서 호출 가능한 
                                        // 함수 정의
  return {
    adddata: function(data) {
      dispatch({
        type: 'adddata',
        data: data,
      })
    }
  }
}
```


### 결론
종속되지 않은 컴포넌트간 동일한 state관리
다중 컴포넌트간 동시간 변하는 state 관리


### 구현 예시

GIT REPO www.funple.com 에서 feature/mockupredux 브랜치
http://l-www.funple.com:8017/main/ko/redux


참고 : https://github.com/firasd/react-redux-tutorial/blob/master/public/scripts/example.js