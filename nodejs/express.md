# Express
> Node 상에서 동작하는 웹 프레임워크

### 특징
- HTTP 통신 요청에 대한 핸들러를 만듬.
- 템플릿에 데이터를 넣어 응답을 만들기 위해 view의 렌더링 엔진과 결합함.
- 접속을 위한 포트나 응답 렌더링을 위한 템플릿 위치같은 공통 웹 애플리케이션 세팅을 함.
- 핸들링 파이프라인 중 필요한 곳에 추가적인 미들웨어 처리 요청을 추가함

###  설치
```
npm install express --save
```
### 예제
```javascript
const express = require('express')
const app = express()

// routing 
// path를 정하고 그에 맞는 함수 작성
app.get('/',(req,res)=>req.send('Hello World!'))

// 3000 port에 listenling.. 
app.listen(3000,()=> console.log('Example app listening on port 3000!'))
```