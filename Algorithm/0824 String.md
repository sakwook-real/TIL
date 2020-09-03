# 0824 알고리즘 String



### 목차

- 문자열
- 패턴 매칭 - 블루트 포스(brute force-중요), KMP, 보이어-무어, 카프라비?
- 문자열 암호화
- 문자열 압축
- 실습 1,2

---



#### 컴퓨터에서의 문자표현

- 글자 A를 메모리에 저장하는 방법
- 메모리는 숫자만을 저장할 수 있기 때문에 각 문자에 대응되는 숫자를 정해 놓고 메모리에 저장하는 방법
- 영어는 대소문자 합쳐 52개 이므로 6비트 (64가지)로 모두 표현 가능. 이를 코드체계라고 함.
  - ex) 000000 -> 'a' , 000001 -> 'b'



#### 아스키코드(ASCII)

- 모두의 코드체계를 통일하고 혼동을 피하기 위해 표준안으로 만들어진 문자 인코딩 표준
- 7bit 인코딩으로 128문자를 표현하며 33개의 출력 불가능한 제어 문자(enter 값 등)들과 공백을 비롯한 95개의 출력 가능한 문자들로 이루어져 있다.
  - bit 는 0, 1을 표현하는 단위이고, bite는 영문자 한 자를 나타내는 단위.
  - parity bit - 에러를 체크하는 비트



#### 확장 아스키는 표준문자 이외의 악센트문자, 도형문자, 특수문자, 특수기호 등 부가적인 문자를 128개 추가할 수 있게하는 부호이다. (표준은 아님)

- 8bit 를 사용
- 컴퓨터 생산자와 소프트웨어 개발자가 여러가지 다양한 문자에 할당할 수 있도록 하고 있다. 이렇게 할당된 확장 부호는 표준 아스키와 같이 서로 다른 프로그램이나 컴퓨터 사이에 교환되지 못한다. (표준이 아니니까)



#### 오늘날 대부분의 컴퓨터는 문자를 읽고 쓰는데 ASCII형식을 사용

#### 그런데 컴퓨터가 발전하면서 각 국가들은 자국의 문자를 표현하기 위해 코드체계를 만들어서 사용

- 우리나라도 예전에는 한글 코드체계를 만들어 사용했고 조합형, 완성형 두 종류를 가지고 있었다. (표준X)
- 잘못 해석되는 문제 때문에 다국어 처리를 위해 표준인 유니코드를 만들었다.



#### 유니코드도 Character set으로 분류

- UCS-2, UCS-4 (변수의 크기가 2바이트거나 4바이트)
- 유니코드를 저장하는 변수의 크기를 정의
- 바이트 순서에 대해 표준화하지 못했음.
- 파일을 인식 시, UCS-2 인지 UCS-4 인지 인식을 할 수 없어 각 케이스를 구분해서 모두 다르게 구현해야 하는 문제 발생
  - big-endian(단위 큰게 앞에 나오는 방식 - 서버가 사용하기도 함), little-endian(PC가 사용하는 방식)
- 그래서 유니코드의 적당한 외부 인코딩이 필요하게 되었다.



#### 유니코드 인코딩 (UTF : Unicode Transformation Format)

- UTF-8 (in web)
  - MIN : 8bit, MAX : 32bit(1 Byte * 4)
- UTF-16 (in windows, java)
  - MIN : 16bit, MAX : 32bit(2 Byte * 2)
- UTF-32 (in unix)
  - MIN : 32bit, MAX : 32bit(4 Byte * 1)



#### Python 인코딩

- 2.x 버전 - ASCII 
- 3.x 버전 - 유니코드, UTF-8 생략가능

---



### 문자열의 분류

- 고정 길이, 가변 길이



#### java에서 String 클래스에 대한 메모리 배치 예

- 클래스로 구현

#### c언어에서 문자열의 처리

- 문자들의 배열 형태로 구현된 응용 자료형.

#### Python 에서의 문자열 처리

- char 타입 없음
- 텍스트 데이터의 취급방법이 통일되어 있음
- 문자열 기호
  - 홑따옴표, 쌍따옴표, 홀따옴표 3개, 쌍따옴표 3개 등
  - 연결
    - 문자열 + 문자열 : 이어 붙여주는 역할
  - 반복
    - 문자열 * 수 : 수만큼 문자열이 반복
- 문자열은 시퀀스 자료형으로 분류되고, 시퀀스 자료형에서 사용할 수 있는 인덱싱, 슬라이싱 연산들을 사용할 수 있음
- 문자열 클래스에서 제공되는 메소드
  - replace(), split(), isalpha(), find()
- 문자열은 튜플과 같이 요소값을 변경할 수 없음 (immutable)



### C와 Java의 String 처리의 기본적인 차이점

- c는 아스키 코드로 저장한다.
- java는 유니코드(UTF-16, 2byte)로 저장한다.
- 파이썬은 유니코드(UTF8) 로 저장

---



### 문자열 뒤집기

- 자기 문자열에서 뒤집는 방법이 있고 새로운 빈 문자열을 만들어 소스의 뒤에서부터 읽어서 카겟에 쓰는 방법이 있겠다.
- 자기 문자열을 이용할 경우는 swap을 위한 임시 변수가 필요하며 반복수행을 문자열 길이의 반만을 수행해야 한다.

- 이를 구현하기 위해

  - c는 위 방법대로 해야한다.

  - java 에서는 StringBuffer 클래스의 reverse() 메소드를 이용하면 된다.
  - Python은 Reverse 함수 혹은 slice notation 을 이용해 구현하면 된다.
    - String 은 immutable 이므로 list(str) 해서 리스트에서 문자열 뒤집기 작업을 하고, "".join(list) 를 통해 str로 다시 바꾸어줘야 함.



---

### 문자열 숫자를 정수로 변환하기

- repr
- atoi (array to integer)



#### str() 함수를 사용하지 않고, itoa()를 구현하기

- 양의 정수를 입력 받아 문자열로 변환하는 함수
- 입력 값 : 변활할 정수 값, 변환된 문자열을 저장할 문자배열
- 반환 값 : 없음
- 음수를 고려하자
  - 123 % 10 -> 3 찾고, 123 // -> 12, 12 % 10 -> 2, 12 // 10 -> 1, 1 % 10 -> 1, 1 // 10 -> 0



### 패턴 매칭에 사용되는 알고리즘들

- 고지식한 패턴 검색 알고리즘
- 카프-라빈 알고리즘
- KMP 알고리즘
- 보이어-무어 알고리즘
  - 뒤에서부터 검색. 패턴이 5글자면 맨 끝 글자가 포함이 아니라면 5칸 뛰어버리는거지 포함되면 길이 맞춰주고



### KMP 알고리즘

- 아이디어 :

  - 불일치가 발생한 텍스트 스트링의 앞 부분에 어떤 문자가 있는지를 미리 알고 있으므로, 불일치가 발생한 앞 부분에 대하여 다시 비교하지 않고 매칭 수행
  - 텍스트에서 abcdabc까지 매치되고, e에서 실패한 상황에서 패턴의 맨 앞 abc와 실패 직전의 abc가 동일함을 이용해보자!

- 패턴을 전처리하여 배열 next[M}을 구해서 잘못된 시작을 최소화함

  - next[M]: 불일치가 발생했을 경우 이동할 다음 위치

  전처리 : 실제로 매턴 매칭을 하기 전, 정보를 미리 계산해두는 것. KMP나 보이어 무어에는 전처리 단계를 거쳐야 한다. 이를 통해 패턴 매칭이 효율적이 된다!

- 시간복잡도 ⇒ O(m+n)



### 보이어-무어 알고리즘

- 대부분의 상용 소프트웨어에서 채택하고 있는 알고리즘

- 오른쪽에서 왼쪽으로 비교

  - 패턴의 오른쪽 끝에 있는 문자가 불일치하고, 이 문자가 패턴 내에 존재하지 않는 경우

    ⇒ 패턴의 길이만큼 뛰어넘어버린다.

  - 패턴의 오른쪽 끝에 있는 문자가 불일치하고, 이 문자가 패턴 내에 존재할 경우

    ⇒ 패턴에서 일치하는 문자를 찾아서 라인을 맞춘다.