# React setting 
React를 셋팅하고 웹 앱을 만들어 본다.
## create-react-app(CRA)
- react 작업환경을 모듈번들러 사용없이 한번에 손쉽게 만들어 주는 도구 
- react를 작업하려면 Webpack같은 모듈 번들러를 사용해야하는데 복잡하고 어렵기 때문에 처음 입문자에게 추천

### 설치
(create-react-app를 설치하기 위해선 node.js와 npm이 설치되어있어야함.)

    > npm install -g create-react-app

## react app 만들기
앱을 만들 위치로 이동하여 react app을 만든다.

    > create-react-app movie_app

### 서버 start

    > npm start

CRA는 configuration을 하지 않아도 개발 서버가 구현이 되어있기 때문에
그냥 서버를 켜주면 됨.

    url:localhost:3000

서버는 app 코드를 수정하게 되면 자동으로 컴파일을 거쳐 새로고침하여 반영한다.

- 서버 실행(호출?) 과정
    + index.html -> index.js -> app.js(컴포넌트)
    
index.html

    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="theme-color" content="#000000">
  
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json">
    <link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico">
    <title>Movie App</title>
    </head>
    <body>
        <noscript>
        You need to enable JavaScript to run this app.
        </noscript>
        <div id="root"></div>
    </body>
    </html>

index.js

    import React from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';
    import App from './App';
    import registerServiceWorker from './registerServiceWorker';

    ReactDOM.render(<App />, document.getElementById('root'));
    registerServiceWorker();
    // reacDOM이 id가 root인 곳에 app을 출력 

App.js

    import React, { Component } from 'react';
    import './App.css';

     class App extends Component{
        render(){
            return(
                <div className="App">
                    Hello!
                </div>
            )
        }
    }

index.html에는 Hello!가 출력됨.

## component
App.js
    
    class App extends Component{
        render(){
            return(
                <div className="App">
                    <Movie/> {/* movie 컴포넌트를 부른 것 */}
                </div>
            )
        }
    }

Movie.js

    class Movie extends Component{
        render(){
            return(
                <div className="Movie">
                    Movie! {/* Movie! 출력 */}
                </div>
            )
        }
    }


- react는 컴포넌트 기반으로 웹의 모든 기능을 쪼개어  각각의 컴포넌트로 분류하여 개발한다. (레고 조각처럼)
- 컴포넌트는 render function을 꼭 가지고 있어야한다.
(render html을 출력해주는 기능을 함)
- 컴포넌트가 생성되고 출력되는 과정
    1. 컴포넌트  생성
    2. render
    3. return 
    4. html
- 큰 컴포넌트 안에 작은 컴포넌트 안에 더 작은 컴포넌트 더 더 작은 컴포넌트 ... 
- 자신이 만들고 싶은 웹을 기능별로 잘게 잘게 쪼개어서 기능을 마지막에 합친다고 보면 됨. 그럼 하나의 웹 페이지 짜잔! 


## React,ReactDom,ReactNative
- React
    + 자바스크립트 ui 라이브러리
- ReactDom
    + React를 웹 사이트로 출력하는 것을 도와주는 것
    + 1개의 컴포넌트를 출력해줌.
- ReactNative
    + React를 모바일 앱에 출력하는 것을 도와주는 것


## Refernce
[노마드코더 리액트 웹](https://www.youtube.com/watch?v=sM2p1EqTlw4&t=10s),여러 블로그..


