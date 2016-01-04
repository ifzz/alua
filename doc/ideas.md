# Ideas

## Goal

대부분의 게임 기능을 lua로 구현하면서 성능이 필요한 곳에 C++을 사용할 수 있도록 한다.

lua로 먼저 오브젝트를 만들고 필요한 함수를 구현하고
C++과 연동하여 필요한 기능을 사용할 수 있도록 한다.

C++ 클라이언트나 lua 클라이언트와 Android / iOS / Windows / Linux에서 모두
쉽게 json 기반, binary 기반으로 통신 가능하게 한다.


binary 기반 통신과 텍스트 기반 통신이 모두 가능하려면 binary를 기반으로
텍스트 기반 통신을 포함하도록 해야 한다.

어려운 부분은 lua로 시작하면서 C++에서 모듈 형식의 기능 확장이 쉬운 구조를 만들어야 한다.

## 방향 결정

### Lua / C++

Module을 C++로 개발할 때 C++에서 클래스까지 제공할 수 있다.
이런 방식으로 lua와 협업할 수 있도록 하는 게 가장 단순하고 쉬운 방법이다.

오브젝트 구성과 데이터 복제의 오버헤드, lua에서 C++ 함수 호출 (stack을 통한 전송)의 부담은 있지만
다른 언어에 비해 매우 빠르게 처리된다.

따라서, Lua / C++ 연동은 전통적인 C++에서 클래스 노출을 제공하고
lua에서 이를 사용하는 방식으로 진행한다.

## 통신

luv 기반으로 (libuv가 매우 빠른 ANSI C 구현이라) 하고 프로토콜은 길이 기반
binary 송수신으로 하고 상위 프로토콜은 protocol-buf를 사용하도록 한다.

luv 기반으로 C++ 상에서 메세지를 만들어 전달하는 방식으로 하고
lua에서는 추가 콜백의 지정을 할 수 있도록 한다.

클라이언트를 asio로 한다면 asio 기반의 서버를 만들고 lua로 노출할 수 있다.
C++를 기반으로 하고 자체 라이브러리를 구현하는 것도 하나의 선택으로 보인다.


## 설계

### Lua 버전

LuaJit이 정체되고 있어 Lua 5.3을 사용하고 각종 Lua 모듈들은 별도로 빌드해서 사용한다.

필요한 C++ 기본 기능을 내장한 alua folder를 하면 main.lua에서 시작한다.

모듈은 lua의 모듈 시스템을 사용한다.


### Lua / C++ 연동

C++11 기반으로 나온 간결한 인터페이스를 찾아서 사용한다.

#### C++ 연동 라이브러리

https://github.com/vinniefalco/LuaBridge

https://github.com/tomaka/luawrapper

https://github.com/sniperHW/luawrapper

 매우 좋아 보인다. lua table 접근, C++ 클래스 접근 등 다양한 기능이 있다.
 좋아 보이면 구현도 좋아야 한다. 이를 사용하고 구현의 이슈가 있다면 참여해서 해결한다.

### 통신 모듈

 cluster를 C++로 구현하고 lua로 클러스터 처리를 담당하도록 한다.

 적은 코드로 목표를 달성할 수 있어야 한다.

 protocol buffer를 사용한다.

#### 통신 프로토콜 라이브러리

 protocol-buf lua 모듈들이 많이 있다. 순수하게 lua로 작게 만들어진 라이브러리가 좋다.

 https://github.com/Neopallium/lua-pb

 순수 lua 구현이다. 이를 사용한다.

 주도적인 처리를 Lua로 한다.

 IDE를 만드는 그날까지 화이팅!

### 단위 테스트

 간결하고 좋은 단위 테스트 모듈을 찾는다.

 https://github.com/nbarendt/LuaUnittest

 이 정도면 충분해 보인다.

### 모듈들

 World

    - Entity : Actor

	- Navigation

	- Collision (Physics)   

### DB

 json 연동 NoSQL DB or MySQL?

 mysql w/ redis (as a cache)

#### Redis + Lua

 https://github.com/nrk/redis-lua

### Coroutine as a processing core

 To continue

 To parallelize

 To keep running

 
