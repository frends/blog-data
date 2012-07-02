{
    "title": "beautiful logging winston (nodejs 프로그래밍에 필수적인 프로그래밍 과정 logging)",
    "author": "nanhapark",
    "date": "2012-06-30T15:13:37.509Z",
    "categories": [
        "html5"
    ],
    "tags": [],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-06-30T15:13:37.509Z",
    "status": "public",
    "important": false,
    "advanced": {}
}

# 소개

이번 cookbook은 <code>logging</code>에 대한 내용입니다. node는 독립적인 어플리케이션 혹은 웹서버가 될 수 있습니다. apache, nginx등 모든 솔루션에는 기본적으로 logging  을 하고 있습니다. 이것이 귀찮아서 console.log으로 남기고, stdout + logrotate 으로 땜방하는 형식으로 logging 하는 경우를 저도 처음에 겪어 보았습니다. 하지만, 이래서는 안되겠더군요.

자 그럼 <code>multiple transport (Console, File, remote...)</code>를 지원하는 <code>winston</code> 에 대해서 알아보겠습니다. 참고로 <a href="https://github.com/flatiron/winston" target="_blank">winston wiki</a>에서 실무에 실제 사용중인 부분과 부가적으로 유용한 부분을 추출하여 설명하겠습니다.


# 설치

`npm install winston`


# 사용방법 미리보기

사용방법에는 일반적으로 console.log 와 같이 사용하는 방법과 winston.Logger 를 사용한 인스턴스화한 후 확장적인 방법으로 사용할 수 있는 2가지가 있습니다.


## 일반적인 방법

    var winston = require('winston');

    winston.log('info', 'Hello from Winston!');
    winston.info('This also works');

    // 마지막값으로 meta object 를 넘길 수 있고
    // 마지막값으로 meta object 를 넘길 수 있고
    // 확장적인 logging 을 할 수 있습니다.
    winston.log('info', 'Test Log Message', { anything: 'This is metadata' });


## 인스턴스화 방법

    winston.Logger(Object options);

    // winston.Logger 객체를 생성
    var logger = new (winston.Logger)({
        // 아래에서 설명할 여러개의 transport를 추가할 수 있다.
        transports: [
            // Console transport 추가
            new (winston.transports.Console)(),

           // File transport 추가
           new (winston.transports.File)({
               // filename property 지정
               filename: 'somefile.log'
           })
        ]
    });

    // 생성한 객체를 사용하여 일반적인 방법과 동일하게 사용
    logger.log('info', 'Hello distributed log files!');
    logger.info('Hello again distributed logs');


# Transport

logging하는 방식을 `transport`라고 칭하고 있으며, 우리가 많이 사용하는 Console, File 그리고 외부 db,mail을 사용할 수 있도록 지원하고 있습니다.


## Console
console.log 과 동일하며 사용방법은 아래와 같습니다.

### winston.add

    winston.add(winston.transports.Console, options)

### 인스턴스화
    var logger = new (winston.Logger)({
        transports: [
            // Console transport 추가
        transports: [
            // Console transport 추가
            new (winston.transports.Console)(options)
        ]
    });

options 값으로 여러가지 환경설정을 할 수 있다.
    
- level
    로그 메세지의 level (debug, info...) 을 지정하여 원하는 level 이상만 logging 할 수 있는 환경설정입니다. (default debug)
- colorize
    terminal화면에 비추어질때 각 로그의 level 을 표시하는 부분을 색상을 입힐것이냐? (default false)
- timestamp
    logging prefix으로 시간값을 첨부하느냐 입니다. (default false)
            
저는 colorize 만 true으로 사용중입니다. console.log 같은경우는 timestamp가 굳이 필요하진 않겠죠.
           
           
## File        
               
fs.createWriteStream 방법을 지원하는 transport입니다.
        
사용법은 아래와 같습니다.

### winston.add 

    winston.add(winston.transports.File, options) 
    
### 인스턴스화

    var logger = new (winston.Logger)({
      transports: [
        // file transport add
        new (winston.transports.File)(options)
      ]
    });


options 값으로 여러가지 환경설정을 할 수 있는 기능이 있습니다.

- level: 위와 상동
- colorize: 위와 상동
- filename: log filename 입니다.
- maxsize: logrotate기능을 대신할 수 있고, 파일당 최대 size 입니다. 이것이 초과되면 새로운 파일을 생성합니다.
- maxFiles: maxsize를 초과할 때 생성되는 파일의 갯수입니다. 이 부분은 정확히 파일 갯수가 유지되지 않고, 5개라 설정하면 7개정도 logrotate되는데 뭔가 문제가 있어보입니다.
        
            
## Remote Webhook

외부 web server으로 logging 을 POST 방식으로 전송할 수 있습니다.

사용방법은 아래와 같습니다.

    var logger = new (winston.Logger)({
       transports: [
           // console transport
           new (winston.transports.Console)(),

           // webhook transport
           new (winston.transports.Webhook)({
               host: 'localhost',
               port: 8081,
               // 기본적으로 지정하지 않으면 /winston-log route되어짐
               path: '/collectdata'
           })
       ]
    });

    logger.log('info', 'Hello webhook log files!', { 'foo': 'bar' });


express에서 request.body 값을 찍어본 실제 결과

    express server listening on port 8081 in development mode
    { method: 'log',
       params:
           { timestamp: '2012-01-29T05:14:10.415Z',
               msg: 'Hello webhook log files!',
               level: 'info',
       meta: { foo: 'bar' } } }


참고로 winston transport기능중 webhook.js에 대한 constructor 첨부합니다.

    source path: winston/lib/winston/transports/webhook.js

    var Webhook = exports.Webhook = function (options) {
        Transport.call(this, options);
    var Webhook = exports.Webhook = function (options) {
        Transport.call(this, options);
        this.name   = 'webhook';
        // 기본설정
        this.host   = options.host   || 'localhost';
        this.port   = options.port   || 8080;
        this.method = options.method || 'POST';
        this.path   = options.path   || '/winston-log';
     
        if (options.auth) {
            this.auth = {};
            this.auth.username = options.auth.username || '';
            this.auth.password = options.auth.password || '';
        }  
           
        if (options.ssl) {
            this.ssl      = {};
            this.ssl.key  = options.ssl.key  || null;
            this.ssl.cert = options.ssl.cert || null;
            this.ssl.ca   = options.ssl.ca;
        }
    };

    


## Remote (CouchDB)

`couchdb`으로 logging하는 방법입니다. http://iriscouch.com 에 무료계정을 생성하여 테스트하였습니다.
    
사용방법은 아래와 같습니다.
           
    var logger = new (winston.Logger)({ 
        transports: [ 
           new (winston.transports.Console)(),
           new (winston.transports.Couchdb)({
               // host
               host: 'nanha.iriscouch.com', 
               // db name
               db: 'log' 
               // 아직 인증부분은 TODO으로 남겨져 있습니다.
           }) 
        ] 
           })
        ]
    });

    logger.log('info', 'Hello webhook log files!', { 'foo': 'bar' });


이외 mongodb, loggly, simpledb, mail관련 transport도 있습니다.



# Exception

node에서 필수적인 exception handler 추가하는 방법입니다. 일반적으로 process.on('uncaughtException', function) 을 사용하는데, 이를 사용하지 않아도, winston안에 위 구문이 처리가 되어 있어서 사용하기 수월합니다.

사용방법은 아래와 같습니다.

## 일반적인 방법
    winston.handleExceptions(new winston.transports.File({
        filename: 'exceptions2.log'
    }));
    winston.log('에러');
## 인스턴스 추가하여 사용하는 방법
    var logger = new (winston.Logger)({
        transports: [
            new (winston.transports.Console)(),
        ],
        // 기존 위와 같이 transports property를 설정했듯이
        // exceptionHandlers Property를 설정하시고
        // 기존과 같이 transports를 추가하면 됩니다.
        exceptionHandlers: [
            new winston.transports.File({
                filename: 'exceptions.log'
            })
        ]
    });

    logger.info('에러');


그러나 이게 단점이 있습니다. 에러 stack 이 한줄로 표시됩니다. 이런 쒸지브이....
        
    
    error: uncaughtException pid=42365, uid=501, gid=20, cwd=/Users/nanhap/Sites/test, execPath=/usr/local/bin/node, version=v0.6.6, argv=[node, /usr/local/lib/node_modules/mocha/bin/_mocha, -R, list, winston.test.js], rss=21655552, heapTotal=15324160, heapUsed=6706944, loadavg=[1.33349609375, 1.322265625, 1.3095703125], uptime=597669, trace=
    ............
    ............
    [column=5, file=/Users/nanhap/Sites/test/winston.test.js, function=,
    function=Function._load, line=310, method=_load, native=false, column=10, file=module.js, function=Array.0, line=470, method=0, native=false, column=40, file=node.js, function=EventEmitter._tickCallback, line=192, method=_tickCallback, native=false], stack=[ReferenceError: non is not defined,     at Object.<anonymous> (/Users/nanhap/Sites/test/winston.test.js:26:5),     at Module._compile (module.js:432:26),     at Object..js (module.js:450:10),     at Module.load (module.js:351:31),     at Function._load (module.js:310:12),     at Module.require (module.js:357:17),     at require (module.js:368:17),     at /usr/local/lib/node_modules/mocha/bin/_mocha:226:27,     at Array.forEach (native),     at load (/usr/local/lib/node_modules/mocha/bin/_mocha:223:9),     at Object.<anonymous> (/usr/local/lib/node_modules/mocha/bin/_mocha:214:1),     at Module._compile (module.js:432:26),     at Object..js (module.js:450:10),     at Module.load (module.js:351:31),     at Function._load (module.js:310:12),     at Array.0 (module.js:470:10),     at EventEmitter._tickCallback (node.js:192:40)]


제일 필요한 아래 stack 구문이 보이긴 하는데 가독성이 떨어져서 이를 해결하기 위해 winston 모듈을 수정해도 되겠지만, 저는 실무에 기존 방법인 oncaughtException 핸들러를 사용하여 console.log으로 남기고, transport에 exception용도로 지정한 File transport 으로 logging 되도록 사용하고 있습니다.

    
# Custom log level
    
기본적인 debug, info, warn 인 log level 말고 더 세분화하여 syslog의 log level도 간단하게 지원하고 있습니다. 저는 syslog level를 강추합니다.

사용방법은 아래와 같습니다.
        
    winston.setLevels(winston.config.syslog.levels);
    logger.setLevels(winston.config.syslog.levels);
        
위와 같이 선업해주면 syslog 는 다음과 같은 심각도 레벨을 가집니다. 따라서 값이 낮을수록 중요한 메시지라고 보시면 됩니다.
        
    0 Emergency
    1 Alert 
    2 Critical  
    3 Error 
    4 Warning
    5 Notice
    6 Informational
    7 Debug

위와 같은 함수를 모두 사용할 수 있습니다. 대문자에 혼동이 있을 수 있는데, 아래 키값을 사용하시면 됩니다.

## winston 모듈내에 syslog 환경설정부분

    var syslogConfig = exports;

    syslogConfig.levels = {
      debug: 0,
      info: 1,
      notice: 2,
      warning: 3,
      error: 4,
      crit: 5,
      alert: 6,
      emerg: 7
    };

    syslogConfig.colors = {
      debug: 'blue',
      info: 'green',
      notice: 'yellow',
      warning: 'red',
      error: 'red',
      crit: 'red',
      alert: 'yellow',
      emerg: 'red'
    };

## 사용방법

    winston.debug('log');
    winston.crit('log');
    winston.alert('log');
    winston.emerg('log');

# Async Callback style

참고로 console 명령어는 sync 한 모델입니다. winston에서는 이것까지 생각하여 transport를 생략하고, 이와같이 event emitter의 영향을 받아 별도로 logging을 사용자가 처리할 수 있도록 event listener를 지원합니다.

    logger.on('logging', function (transport, level, msg, meta) {
        // [msg] and [meta] have now been logged at [level] to [transport]
    });
    });
     
    logger.on('error', function (err) {
        // handle an error
    });
     
    logger.info('CHILL WINSTON!', { seriously: true });
      
      
      
# 결론
      
winston 은 굉장히 구조적이고 확장적인 logging module 입니다. 필히 node로 제작된 어플리케이션이 production level에 배포하는 서비스라면 logging과정은 필수이니 필히 winston을 사용하여 아름다운 logging을 하시
기 바랍니다. 
      
KIN 플~
