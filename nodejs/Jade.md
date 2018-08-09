# Jade
> Node.js 용으로 만들어진 View 템플릿 엔진 (jade의 이름이 pug로 변경됨)

### 템플릿 엔진
- 동적인 파일과 정적인 파일의 장단점을 결합한 새로운 체계
- 익스프레스는 템플릿 엔진을 이용해 웹 사이트를 생성함.
- 대표적인 템플릿 엔진은 Jade

## Jade 사용하기
1. 템플릿 엔진(Jade) 설치
    - 익스프레스는 템플릿 엔진의 기능을 가지고 있지 않기 때문에 템플릿 엔진을 설치 해주어야함.  
    `npm install jade --save`  
2. Jade와 익스프레스 연결
    - Jade와 Express를 연결하기 위해서 설정이 필요함.   
    - Express 에게 템플릿 뷰 엔진을 알려줌  
        `app.set('view engine'.jade')`
    - Template file이 모여있는 디렉토리를 알려줌  
        `app.set('views','./public')`
3. template 랜더링
    - 템플릿 엔진이 적용된 템플릿 파일을 라우터에 추가함  
    ```
    app.get('/template', function(req, res){
            res.render('temp');
    })
    ```
## Jade 문법
### 구조 표현
Jade는 들여쓰기를 통해 html구조를 표현한다.
```jade
html
    head
        // head 부분
    body
        // body 부분
        p hello,world!
```
### 태그
태그에 옵션도 추가할 수 있다.
```jade
div(id="wrap")
``` 

### context
```js
router.get('/',function(req,res){
    res.render('temp',{'hello':'hello'})
})
```
```jade
p #{hello}
```

### Javascript
jade 내에서는 자바스크립트 문법 사용도 가능함.
```jade
// 변수 설정
- var hello = "hello"

// 조건문 
if hello == "hello"
    p hello

// 반복문 (foreach와 동일)
each item in itmes
    p item

// javascript 코드 작성
script.
    var temp = "temp";
```