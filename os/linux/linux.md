# Linux

## CLI(Commend line Interface) 명령어

#### pwd
현재 디렉토리

#### cd [이동할 디렉토리 경로 명]
디렉토리를 이동함. (change directory)
```
cd /home/ubuntu  # 절대 경로
cd .. # 상대 경로
```

#### rm [파일]
파일을 지움

##### option
`-r [dir]` 디렉토리를 지움.

#### man [명령어]
명령어에 대한 상세한 메뉴얼을 보여줌.

### mv [이동할 파일] [이동할 디렉토리와 파일명]
파일을 이동함.
```
mv test.txt /home/test.txt
```
같은 디렉토리에 위치하며 이름을 다르게 하면 
파일의 명을 바꿀 수 있음.

## sudo(super user do)
> 관리자 권한

## Package manager
> 패키지를 다운받고 검색할 수 있는 것
#### 리눅스
- apt,yum
##### mac
- home-brew

앱스토어의 역할과 비슷함.

## Shell

### 커널
> 하드웨어를 제어해서 기계가 동작할 수 있도록 함.

### 쉘
> 사용자의 명령어를 커널이 이해할 수 있도록 함.(커널을 제어)

현재 쉘 확인  
`$ echo $0`

### 쉘 스크립트
> 명령어들의 모음, 순차적으로 진행되어야할 쉘 명령어를 모아놓은 파일

쉘 스크립트를 통해 자동화된 작업을 할 수 있음

backup 스크립트
```shell
#!/bin/bash
# 쉘 스크립트라는 것을 명명.(밑에 있는 명령어들은 bin/bash라는 프로그럄에 의해 해석되어야한다.)

if ! [ -d bak ]; then # 디렉토리 bak 이 없다면
    mkdir bak # 디렉토리 bak을 만든다.
fi # if문 종료

cp *.log bak # 현재 디렉토리의 확장자가 .log인 파일을 bak폴더에 복사한다.
```

#### 실행 권한
`chmod +x backup`

#### 쉘 스크립트 실행
`./backup`

