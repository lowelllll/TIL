# AWS 인프라를 제어하는 방법들
### Management console
> GUI 방식. 마우스,아이콘으로 제어하는 것

#### 장점
- 익숙함.
- 보이는 대로 조작이 가능함.

### CLI
> `Commend Line Interface` 명령어로 입력해서 컴퓨터를 제어하는 것

#### 장점
- 사용하는 방법을 익히면 GUI보다 편하게 제어할 수 있음.
- 일련의 연속적인 작업을 한꺼번에 할 수 있음.

#### 단점
- 명령어를 외우기 힘듬.
### SDK
> `Softwear Development Kit` 프로그래밍을 통해 제어하는 것

- 프로그래밍 언어(C,Java,Python 등)별로 AWS 인프라를 제어할 수 있는 명령어의 세트를 제공함.
```js
var aws = require('aws-sdk')...
```

### API
> `Application Programing Interface` WEB을 통해 제어하는 것

- WEB을 통해 AWS 인프라를 제어할 수 있음.
- 가장 기초적인 방식.
- 어떤 언어를 사용하건 상관없이 AWS 인프라를 제어할 수 있음.
- AWS는 API를 통해 AWS 인프라를 제어하는 것이 어렵기 때문에 API를 이용하는 SDK를 제공함.

```
https://ec2.amazonaws.com/?Action=DescribeInstances
```
