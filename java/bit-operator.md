# 비트연산자

- `|` (OR연산자)

  피연산자중 한쪽의 값이 1이면 1을 결과로 얻음.  그 외에는 0을 얻음

  - 주로 특정 비트 값을 변경할 때

- `&`  (AND 연산자)

  피연산자 양쪽이 모두 1이어야만 1을 결과로 얻음. 그 외에는 0을 얻음

  - ​    값을 추출할 때 

- `^` (XOR 연산자)

  피연사자의 값이 서로 다를 때만 1을 결과로 얻음 그 외에는 0을 얻음

  |  x   |  y   | x \| y | x & y | x ^ y |
  | :--: | :--: | :----: | :---: | :---: |
  |  1   |  1   |   1    |   1   |   0   |
  |  1   |  0   |   1    |   0   |   1   |
  |  0   |  1   |   1    |   0   |   1   |
  |  0   |  0   |   0    |   0   |   1   |

- `~` 비트 전환 연산자

  피연산자를 2진수로 표현했을 때 0은 1로, 1은 0으로 바꿈.

  - 피연산자의 1의보수를 얻을 수 있음(1의 보수)



### 쉬프트 연산자

`>>` , `<<`

피연산자의 각 자리(2진수)를 오른쪽 또는 왼쪽으로 이동함.

- `8 << 2` 는 8의 2진수를 왼쪽으로 2비트 이동함.

- 자리이동으로 저장범위(비트수)가 벗어난 값들은 버려지고, 빈자리는 0으로 채워짐.

  ​      0 0 0 0 1 0 0 0 = 8 

  0 0 0 0 1 0 0 0 

  ​      0 0 1 0 0 0 0 0  = 32

- `<<` 연산자의 경우 정수는 부호를 유지하기 위해 왼쪽 피연산자가 음수인 경우 빈자리를 1로 채우고(음수)

  양수인 경우는 0으로 채움.

- `X << N`에서 N이 X의 bit수보다 크면 자료형의 bit수로 나눈 나머지 만큼 이동함.

  - int형(32bit) 일 경우 `8 << 32 = 8`  (32 % 32 = 0)

- 쉬프트 연산자의 공식

$$
X << N = 2 * 2^n
$$

$$
X >> N = X  /  2^n
$$

굳이 쉬프트 연산자를 사용하는 이유 ? 속도가 빠름! 


