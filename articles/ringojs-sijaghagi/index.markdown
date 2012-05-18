{
    "title": "RingoJS 시작하기",
    "author": "frends",
    "date": "2012-05-17T09:00:20.029Z",
    "categories": [
        "Java"
    ],
    "tags": [],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-05-17T09:00:20.029Z",
    "status": "public",
    "important": false,
    "advanced": {}
}

## Introduction

NodeJS와 유사한 프로젝트로, RingoJS라는걸 소개하고자 한다. 이 역시 CommonJS 구현체이고, 구현된 스팩은 아래와 같다.

* Modules 1.1
* JSGI 0.3
* Binary/B
* I/O A
* Filesystem/A (minus globbing)
* Filesystem/A/0
* System 1.0
* Unit Testing 1.0

RingoJS는 NodeJS와 달리 JVM위에서 동작하는 프로젝트이므로, 기존에 Java로 작성된 모듈을 재활용할 수 있고, 라이브러리 또한 재활용 할 수 있다. 필요한 부분만 Wrapper를 만들어서 쓰면 기존에 작성된 모듈과 자바스크립트로 연결이 가능하다.
(물론 Wrapper를 만들어야한다는게 문제라면 문제다.)

## Installation

http://ringojs.org/getting_started 에서는 간단하게 어떻게 설치하는지에 대한 설명이 나와 있는데, 요약하면 다음과 같다.

JDK를 설치한다.
Rhino 1.7R3-SNAPSHOT 버전이 필요하다.(JavaScript Version 1.8을 필요로 하기 때문)
Git를 통해 소스를 내려받고, ant jar 로 빌드한다.
여기서는 이클립스를 사용해 만드는것으로 가정하고, 이미 만들어진 프로젝트를 활용하여 시작하는것으로 하자.

편리한 시작을 위해 https://github.com/kyungw00k/ringojs-eclipse-skeleton 에 프로젝트 뼈대를 만들어 놨다.
Maven Project이니 사용하는 이클립스에 maven project를 사용할 수 없으면 m2eclipse같은걸 설치한 후에 사용하면 되겠다.

## Project Structure

![](./@img/structure1.png)

* /src/main/webapp/WEB-INF/app - RingoJS App이 위치한 경로
* /src/main/webapp/WEB-INF/app/public - static files
* /src/main/webapp/WEB-INF/app/skins - template files
* /src/main/webapp/WEB-INF/packages - commonjs packages. 추가 패키지가 있으면 여기에 넣으면 된다.
* /src/main/webapp/WEB-INF/lib - 자바 라이브러리(JAR형태)가 필요하면 여기에 넣는다.

동작 순서는, 일단 web.xml에 있는 정보대로 JSGI 서블릿을 하나 생성하게 되고, 이 서블릿에서 모든 URL path를 처리하게 되어있다.

보통, nodeJS에서는 node server.js 와 같은 형태로 앱을 실행하지만,
여기서는 jetty와 같이 기존 웹서버를 사용하는 형태로 진행하기에 따로 ringo main.js 와 같은 형태로 가지 않을것이다.
(물론 RingoJS에서도 nodeJS와 마찬가지로 ringo main.js 와 같이 실행한다.)

서버 띄우기 전에 config.js 를 먼저 살펴보자.

```
// URL routing. Using require() here to statically import modules will
// improve performance, but may cause hard to debug cyclic module dependencies
// in case any app module requires this module.
// 아래의 경우는, / 를 actions.js에게 위임한다는 의미다.
exports.urls = [
    ['/', './actions'],
];

// Middleware stack as an array of middleware factories. These will be
// wrapped around the app with the first item in the array becoming the
// outermost layer in the resulting app.
exports.middleware = [
    require('ringo/middleware/gzip').middleware,
    require('ringo/middleware/etag').middleware,
    require('ringo/middleware/static').middleware(module.resolve('public')),
    // require('ringo/middleware/responselog').middleware,
    require('ringo/middleware/error').middleware('skins/error.html'),
    require('ringo/middleware/notfound').middleware('skins/notfound.html'),
];

// The JSGI application. This is a function that takes a request object
// as argument and returns a response object.
exports.app = require('ringo/webapp').handleRequest;

// Standard skin macros and filters
exports.macros = [
    require('ringo/skin/macros'),
    require('ringo/skin/filters'),
];

// Default character encoding and MIME type for this app
exports.charset = 'UTF-8';
exports.contentType = 'text/html';
아래는 actions.js 내용이다.

var {Response} = require('ringo/webapp/response');

exports.index = function (req) {
    return Response.skin(module.resolve('skins/index.html'), {
        title: "It's working!"
    });
};
```

위의 두 파일에서 보면, '/' 액션은 actions.js가 처리하게 했고, actions.js 에서는 index 라는 GET 액션을 처리하고 있다.
(다시말해 /index 를 처리하고 있는 샘)

Response.skin이 조금 어색하긴하지만, 일단 템플릿 엔진으로 생각하면 된다.
여기서는  skins/index.html 을 템플릿으로 사용하고, Object 를 넘겨주는데 이때 title이란 값으로 스트링을 넘겨주고 있다. 
(자세한 사항은 http://ringojs.org/wiki/Tutorial#skins_subskins 부분을 참고하도록 하자.)

그렇다면,  POST나 PUT, DELETE는 어떻게 처리할까?
셈플에는 없어서 간단히 한번 만들어보았다.

```
var {Response} = require('ringo/webapp/response');

exports.index = {}; // Object 로 선언한다.

exports.index.GET = function (req) {
    return Response.skin(module.resolve('skins/index.html'), {
        title: "It's working!"
    });
};

// 나머지 액션은 아래와 같이 처리한다.
exports.index.POST = function (req) { ... };
exports.index.PUT = function (req) { ... };
exports.index.DELETE = function (req) { ... };
```

대충 어떻게 돌아가는지 (대충) 설명했다. 이제 서버를 한번 띄워보자.

```
mvn install

mvn jetty:run
```

위의 두 행으로 서버가 뜨게 된다.

이클립스에서는 일단...

![](./@img/maven_install.png)

이렇게 maven install을 한 후에, Run As로 다시 가서 Run Configurations을 실행시킨다.

![](./@img/run_configuration.png)

Goals에 jetty:run을 넣고 Run으로 실행을 한다.

실행이 되면 서버가 뜰테고, 특별한 설정이 아니라면 8080 포트로 설정이 되어있을꺼다.

아래는 서버 띄운 후 간단한 실행한 화면이다.

![working](./@img/working.png)


RingoJS의 독특한점(?)이라고 한다면 Google App Engine에서 사용할 수 있다는 점이다.

App Engine 위에서 RingoJS Project를 띄우려고 만든 셈플 프로젝트가 있다.
http://ringo-chat.appspot.com 이고, NodeChat을 제가 그냥 RingoJS로 포팅해서 엡엔진에 배포한 프로토타입이다.

소스는 [https://github.com/kyungw00k/ringo-chat](https://github.com/kyungw00k/ringo-chat) 에 있고, 다음 Post에서는 이 소스를 기준으로 간단하게 살펴보도록 하겠다.