# react로 move_app 만들기
## JSX
> 개발자가 자바스크립트 내부에 마크업 코드를 작성해 줄 수 있게 해줍니다.
[출처](http://blog.sonim1.com/175)
- 문법은 html과 비슷하지만 다른 부분도 있다.(className,주석 등)

### className 
- JSX는 태그에 class를 달때 class가 아닌 className 속성을 사용한다. 기능은 똑같다.

### 주석
- 주석은 {/* 내용 */} 이런식으로 사용한다. 

        class App extends Component{
            render(){
                return(
                    <div className="App">
                        <h1>Hello! JSX</h1>
                        {/* 이것은 주석 */}
                    </div>
                );
            }
        }


## props
> props는 parent로부터 받는 데이터이며 (자식 컴포넌트의 입장에서는) 불변성 데이터, 즉 값을 바꿀 수 없는 데이터라고 생각해도 됩니다
- props는 부모 컴포넌트에서 자식 컴포넌트로 전달되는 값이다. 
- 모든 정보를 메인에 넣고  각각 컴포넌트에 props를 이용하여 정보를 출력한다 -> 데이터 플로우 
- 큰 컴포넌트가 전체 정보를 각각의 칠드런에게 전달.
한개의 데이터를 가지고 컴포넌트별로 나눠 출력하면 됨.
- 부모 컴포넌트는 자식 컴포넌트에 속성처럼 props를 전달하고 자식은 this.props.props_name 으로 값을 받는다.
- 자바스크립트의 property에서 나온 단어같다.(개인적인 생각)

App.js(부모 컴포넌트)
    
    const movie = [
        '겨울왕국',
        '신과함께'
    ]

    class App extends Component{
        render(){
            return(
                <div className="App">
                    <Movie title={movie[0]}/>
                    <Movie title={movie[1]}/> {/* Movie (자식 컴포넌트)로 props 전달 */}
                </div>
            );
        }
    }

Movie.js(자식 컴포넌트)

    class Movie extends Component{
        render(){
            return(
                 <h1>{this.props.title}</div> {/* 부모로 부터 받은 props 사용*/}
            );
        }
    }


## map()
> map() 메소드는 파라미터로 전달 된 함수를 통하여 배열 내의 각 요소를 프로세싱 하여 그 결과로 새로운 배열을 생성합니다..
- arrays.map(array=> array의 기능(내용)) 이런식으로 arrays를 참조하여 array를 만든다고 보면 된다.
- array 선택,해당 항목 맵핑 -> 리스트 만듬(컴포넌트 리스트)

ex)

        const movies = [
            {
                title:title,
                poster:poster
            }
        ]

        // ...component 내부

        render(){
            return(
                {
                    this.movies.map(movie => <Movie title=movie.title poster=movie.poster/>
                    {/* movies array를 참조하여 movie array를 만들었다.
                    movie는 movies를 순차적으로 접근한다. */}
                    {/*
                        아래 코드는 위 코드를 풀어놓은 것과 같다.
                        [
                             <Movie title={movies[0].title} poster={movies[0].poster }>
                             <Movie title={movies[1].title} poster={movies[1].poster }>
                        ]
                        
                    */}
                }
            );
        }


### map error
> error
Each child in an array should have a unique "key" prop.
Check the render method of TableComponent.'
- map에서 새로운 array를 만들때 앨리먼트가 많을 때 유니크한 키(key)를 넣어줘야한다. 

        <div className="App">
            {movies.map((movie,i) => {
            return <Movie title = {movie.title}poster={movie.poster} key={i} />
            })}
        </div>

- 사실 component의 key는 map의 인덱스를 사용하지 않는 것이 좋음 왜? 느림

## prop-type
- 컴포넌트가 얻게되는 정보(props)의 타입을 확인함.
- 컴포넌트가 prop를 받을 때 정해진 type만 받고 싶을 경우 사용함.
- 이름이 title인 데이터는 string type 이면서 필수로 값을 받아야 할때 

       // class component 내부에서 정의 

        static propTypes = {
            title:propTypes.string.isRequired
        }

- propTypes는 따로 install 해주어야 함.

    npm install prop-types

- functional component에서 prop-type 사용

    Movie.propTypes = {
        title:propTypes.string.isRequired
    }


## Component Lifecycle
> 컴포넌트가 DOM 위에 생성되기 전 후 및 데이터가 변경되어
상태를 업데이트하기 전 후로 실행되는 메소드 들
- 컴포넌트는 여러 기능(메소드)를 정해진 순서대로 실행한다. (cycle 자동으로 발생)
- render: componentWillMound()->render()->componentDidMount()
- update: componentWillReceiveProps()->shouldComponentUpdate()             ->componentWillUpdate()->redner()-> componentDidUpdate() 
    + shouldComponentUpdate() : old props 와 새로운 props가 다를 경우 update==true 형식으로 되어 업데이트 진행함.
- 컴포넌트를 만드는데 도움이 됨.

## state
- react component 안의 object
- component 안의 state가 바뀔 때 마다 render가 발생함(update할때마다 새로운 sate와 함께)
> 컴포넌트에서 유동적인 데이터를 다룰 때, state 를 사용합니다
ex)

    // component 내부

    state = {
        movies:[
            {
                title:'겨울왕국'
            },
            {
                title:'신과함께'
            }
        ]
        greeting:"Hello"
    }

    render(){
        return(
            <div>
                {this.state.greeting} {/* state 접근 하는 법 Hello 출력*/}
            </div>
        );
    }

- state는 직접적으로 변경하면 안됨.
    + 변경하는 법
        
            this.setState({
                greeting:"hello again"
            })

    + list type의 state를 추가할 때는 ...this.state.movies를 추가해 주어야함.

            this.setState({
                ...this.state.movies,
                movies:[
                    {
                        title:'강철비'
                    }
                ]
            })
            
## loading state 
### 의문
- state에 데이터가 빈 상태에서 render에 데이터를 불려오려고하면 어떻게 될까? 당연히 오류남 왜냐고? 비었으니까^^

        // App component 내부 
        state = {}

        render(){
            return(
                {this.state.movie} {/* ERROR! */}
            );
        }

- 위 같은 코드와 같은 방식으로 api를 사용할 때 api 서버가 엄청 느려서 state에 데이터를 불러오지 못했는데 render가 되면 에러가 나게 된다.
### 해결
- loading state를 사용한다.

        state = {
            
        }
        componentDidMount(){
            this._getMovies(); //1. component가 생성 되었을 경우 데이터를 불러온다. 
        }
  
        _renderMovies = () => {
            const movies = this.state.movies.map(movie => {
            return <Movie
                title = {movie.title} 
                poster={movie.medium_cover_image} 
                key={movie.id}
                genres = {movie.genres}
                synopsis = {movie.synopsis}
                />
            }) // 7.Movie 컴포넌트 출력
        }
    
        _getMovies = async () => {
            const movies = await this._callApi() //2._callapi 메서드 호출
            this.setState({
                movies //4. movies : movies -> state 생성한다. 
                //5. 데이터가 변경되므로 다시 render
            })
        }

        _callApi = () => {
        return fetch('https://yts.am/api/v2/list_movies.json')// 3.api 호출
                .then(response => response.json())// do someting 
                .then(json => json.data.movies)
                .catch(err => console.log(err))
        }

        render(){
            return(
                <div>
                {this.state.movies ? this._renderMovies() : "Loading"}
                // state.movies 가 존재할 경우 _renderMovies() 호출, 아니면 Loading 출력
                // 6.state.movies가 존재하므로 _renderMovies() 호출
                </div>
            );
        }

- 3항 연산자를 활용하여 loading state를 구현한다.

## functional component(멍청 컴포넌트)
- 모든 컴포넌트가 state를 가지고 있는 것은 아니다.
- state를 가지고 있지 않은 stateless component
- 그냥 return을 하기 위해 존재한다. lifecycle같은 메소드나 state 가 없고 prop 데이터를 받고 return 기능만을 가지고 있다.
- this도 없음

        function component_name({props}){
            return(
                {/* functional component 구조 */}
            );
        }

- 지금까지 class App extends Component... 로 만든 컴포넌트는 class component (똑똑 컴포넌트)
- 만들 컴포넌트가 적은 props와 return만 하면 된다면 사용한다.

## fetch
- fetch는 react에서 ajax 통신을 쉽게 할 수 있도록 해준다.

        fetch("요청 URL")

### ajax 
> Ajax(Asynchronous JavaScript and XML, 에이잭스)는 비동기적인 웹 애플리케이션의 제작을 위해 아래와 같은 조합을 이용하는 웹 개발 기법이다.
- url을 자바스크립트로 비동기적으로 불러옴.
     + 비동기적?
        + 첫번째 라인이 끝나던 말던 두번째 라인이 작업함
        + 계속 다른 작업을 스케줄 할 수 있음
        + 모든 작업들은 다른 작업과 상관없이 수행됨.
- 새로고침없이 작업 가능하며 리액트와 작업이 간편함.

## promise
- 코드를 짤때 시나리오를 잡는 방법을 만들어줌
- 성공적으로 수행할 경우, 그렇지 않을 경우 결과물을 catch,then으로 받아볼 수 있음.

    _callApi = () => {

      return fetch('https://yts.am/api/v2/list_movies.json')
      .then(response => response.json())// do someting 
      .then(json => json.data.movies)
      // 위의 라인이 완료되면 무언가를 해라. 근데 라인이 에러가 있을 경우 
      .catch(err => console.log(err))
      // 잡아서 나에게 보여줘
    }


- 대략 이런거임..

## async,await
- promise에서의 then은 콜백지옥으로 빠질 수 있음.
then,then,then....
- async는 이전 라인의 작업이 끝날때 까지 기다리지 않아도 작업이 진행됨

        _getMovies = async () => { // await는 async를 써야 사용가능 
        const movies = await this._callApi() // callApi가 끝나기를 기다림 성공적이든 아니던 작업이 끝나는 것
        작업이 완료될때까지
        this.setState({
            movies
        })
        }


## 느낀점
- 솔직히 fetch부터 잘 이해가 안간다. 다시 복습하면서 개념을 익혀야 겠다. 
- javascript 다시 공부해야될듯... OTL

## Refernce
[노마드코더 리액트 웹](https://www.youtube.com/watch?v=sM2p1EqTlw4&t=10s),여러 블로그..
