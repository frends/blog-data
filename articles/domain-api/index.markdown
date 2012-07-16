{
    "title": "Domain module 소개"
    "author": "nanhapark",
    "date": "2012-07-15T15:13:37.509Z",
    "categories": [
        "node.js"
    ],
    "tags": [],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-07-15T15:13:37.509Z",
    "status": "public",
    "important": false,
    "advanced": {}
}

# Domain module 소개

> Domains provide a way to handle multiple different IO operations as a single group. If any of the event emitters or callbacks registered to a domain emit an error event, or throw an error, then the domain object will be notified, rather than losing the context of the error in the process.on('uncaughtException') handler, or causing the program to exit with an error code.

node `v0.8` 새로운 기능 [domain api](http://nodejs.org/api/domain.html), 뭐라 설명하기가 굉장히 애매한 API입니다. 그냥 에러처리기라고 하기도 그렇고... 위 정의가 굉장히 멋있는데, 실무에서 사용한 결과 제 입장에서는 `try ~ catch` 의 또다른 형태라고 정의하였습니다. 아래 소스설명 보시고 나름 이해하시고 정의 내리시면 됩니다. 그리고, 저와 다른 정의를 내린 분은 의견 공유 부탁드려요.


# 배경

nodejs의 callback에서 발생하는 `throw`를 전체 프로세스를 묶은 `try`로 에러를 제어 할 수 없습니다. 예를 들어봅시다.

- 다른 언어에서 `try~catch`는 여러개의 모듈사용 흐름을 하나의 프로세스 흐름으로 생각하고 에러처리를 도와줍니다. 보통 모듈에서 throw를 반환하고 이를 사용하는 로직레벨에서는 `try~catch`로 제어하여 깔끔한 구문을 만들 수 있죠. 이것이 nodejs에서 된다면 굉장히 편할 것입니다.
  아래 소스는 `fs.mkdir`를 `try~catch`로 묶어 보고 싶은 심정의 코드입니다.

        try {
          fs.mkdir(dirname, function(err, data) {
            if (err) {
              throw err;
            }
            cb(null, data);
          });
        } catch (e) {
          console.log(e);
        }

  위의 예제는 catch 구문에서 callback으로부터 throw 에러가 잡히면 깔끔한데, 그렇게 처리되지 않습니다. @.@

- callback 안에서 try... 당연히 catch 잡히죠. 그러나, 쓸모가 없습니다. `try~catch`는 전체적인 흐름을 컨트롤 할 수 있어야 쓸모가 있다고 생각합니다.

        fs.mkdir(dirname, function(err, data) {
          try {
            if (err) {
              throw err;
            }
            cb(null, data);
          } catch (e) {
            console.log(e);
          }
        });


----

  자 이리하여 등장한 [domain api](http://nodejs.org/api/domain.html) 이런 경우에 대해 조금이나마 `해결 혹은 아이디어`를 제공해줍니다. 돌이켜보면 처음에 try~catch 구문을 어떻게서든지 사용할라고 노력했으나, 지금은 `process.on('uncaughtException')`에 기대고 있네요.

# 사용순서

- domain 모듈 `require`

        var domain = require('domain');

- domain `객체의 생성`
        
        var d = domain.create();

- domain에서 체크할 함수 등록

        d.run(function() {});
        var foo = d.bind(function() {});

- domain과 에러 핸들링 선언

        d.on('error', function(err) {});


[domain api](http://nodejs.org/api/domain.html) 에 기타 함수설명들이 있습니다.

# 사용패턴

보통 아래 3가지 경우로 요약할 수 있습니다. 

- callback function에 `domain`을 연결하여 오류 처리

        var domain = require('domain').create();
        domain.bind(function() {});

- 함수를 호출할 때 `domain`을 연결하여 오류 처리

        var domain = require('domain').create();
        (domain.bind(foo))(function() {});

- 함수 정의시 `domain`을 연결하여 오류 처리

        var domain = require('domain').create();
        var fooWrapDomain = domain.bind(foo);
        fooWrapDomain(function() {});



# crash 되지 않도록 하는 일반적인 소스

## a.js

    var foomodule = require('./foomodule');
    //
    // 모든 throw 를 처리
    // 무조건 필수!!!!!!!!!!!!!!
    //
    process.on('uncaughtException', function(err) {
      console.log('uncaughtException ===');
      console.log(err);
    });
    //
    // 모듈호출
    // recursive/directory 전달하여 fs.mkdir에서 처리될 수 없도록 에러발생
    //
    foomodule.foo('recursive/directory', function() {
      console.log('result ===');
      console.log(arguments);
    });

## foomodule.js 

    var fs = require('fs');

    //
    // @param String dirname
    // @paran Function cb
    //
    function foo(dirname, cb) {
      fs.mkdir(dirname, function(err, data) {
        if (err) {
          // 에러발생
          throw err;
        }

        cb(null, data);
      });
    }

    module.exports = {
      foo: foo
    };

## 실행결과

    uncaughtException ===
    { [Error: ENOENT, mkdir 'recursive/directory'] errno: 34, code: 'ENOENT', path: 'recursive/directory' }


이 경우의 단점은 `process.on('uncaughtException', function() {})`으로 모든 `에러`가 처리되기 때문에, 모듈을 호출시 `해당 컨텍스트안`에서 무엇을 처리할 `요구사항`이 생긴다면 멘붕이 다가올 수 있습니다.


# crash 되지 않도록 domain api를 사용하는 소스

초점은 모듈 context 안에서 무언가를 처리하자는 목적입니다.

## a.js

    var foomodule = require('./foomodule');
    //
    // 모든 throw 를 처리
    // 사용안함.
    //
    process.on('xxxxx___uncaughtException', function(err) {
      console.log('uncaughtException ===');
      console.log(err);
    });
    //
    // 모듈호출
    // recursive/directory 전달하여 fs.mkdir에서 처리될 수 없도록 에러발생
    //
    foomodule.foo('recursive/directory', function() {
      console.log('result ===');
      console.log(arguments);
    });

## foomodule.js 

    var fs = require('fs');

    function foo(dirname, cb) {
      //
      // 도메인 require / 객체 생성
      //
      var domain = require('domain').create();

      //
      // domain에서 exception 체크
      //
      domain.on('error', function(err) {
        console.log('domain ===');
        console.log(err);

        // 현재 모듈의 context에서 처리
        cb('wow');
      });

      //
      // callback에 domain을 binding
      //
      fs.mkdir(dirname, domain.bind(function(err, data) {
        //
        // context안에서 에러나면 위 에러핸들러에서 처리됩니다.
        //
        if (err) {
          throw err;
        }

        cb(null, data);
      }));

      //
      // 밑에 혹은 위 callback 안에서 여러개의 함수조작을 하고, 안에 안에 안에 ... context에서 throw가 발생하더라도
      // 모두 domain error handling으로 컨트롤 할 수 있습니다.
      //
    }

    module.exports = {
      foo: foo
    };

## 실행결과

    domain ===
    { [Error: ENOENT, mkdir 'recursive/directory']
      errno: 34,
      code: 'ENOENT',
      path: 'recursive/directory',
      domain_thrown: true,
      domain: { members: [], _events: { error: [Function] } } }
    result ===
    { '0': 'wow' }

- domain과 에러 핸들링 선언한것이 동작했습니다.
- uncaughtException 으로 처리되지 않아 `wow`라는 메세지를 `callback`으로 넘길 수 있었습니다.


# crash 되지 않도록 domain api를 global 로 사용하는 소스

## a.js

    //
    // try ~ catch 하나의 흐름으로 시도
    //
    global.domain = require('domain').create();

    //
    // domain에서 exception 체크
    //
    domain.on('error', function(err) {
      console.log('domain ===');
      console.log(err);
    });

    var foomodule = require('./foomodule');

    //
    // 모듈호출
    // recursive/directory 전달하여 fs.mkdir에서 처리될 수 없도록 에러발생
    //
    foomodule.foo('recursive/directory', function() {
      console.log('result ===');
      console.log(arguments);
    });

## foomodule.js 

    var fs = require('fs');

    function foo(dirname, cb) {
      //
      // callback에 domain을 binding
      //
      fs.mkdir(dirname, domain.bind(function(err, data) {
        //
        // context안에서 에러나면 위 에러핸들러에서 처리됩니다.
        //
        if (err) {
          throw err;
        }

        cb(null, data);
      }));
      
      //
      // 한번더! callback에 domain을 binding
      //
      fs.mkdir(dirname, domain.bind(function(err, data) {
        //
        // context안에서 에러나면 위 에러핸들러에서 처리됩니다.
        //
        if (err) {
          throw err;
        }

        cb(null, data);
      }));
    }

    module.exports = {
      foo: foo
    };

## 실행결과

    domain ===
    { [Error: ENOENT, mkdir 'recursive/directory']
      errno: 34,
      code: 'ENOENT',
      path: 'recursive/directory',
      domain_thrown: true,
      domain: { members: [], _events: { error: [Function] } } }

wow



# 결론

domain api에 대해서 설명을 드렸습니다. 저는 몇가지 아이디어가 떠오르네요. nodejs app을 crash 하지 않기 위해 `process.on('uncaughtException', function() {})`을 애용했는데, `domain api`도 적절히 포함시켜야 겠습니다. 질문은 [http://nodeqa.com](http://nodeqa.com) 이곳으로 
