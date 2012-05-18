{
    "title": "Cappuccino 프레임워크 시작하기 2",
    "author": "frends",
    "date": "2012-05-18T00:17:36.770Z",
    "categories": [
        "javascript"
    ],
    "tags": [],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-05-18T00:17:36.770Z",
    "status": "public",
    "important": false,
    "advanced": {}
}

## 카푸치노 웹개발환경 구축하기
웹 클라이언트 개발환경. 즉 브라우저 기반 어플리케이션을 개발하는것은 데스크탑 OS 혹은 서버측 코드를 개발하는 환경과는 뚜렷한 차이가 있습니다.
Cappuccino 는 이러한 차이를 줄여나가기 위해 디자인되어 있습니다.
Cappuccino 시작하기 이번에는 잘알려진 좋은 도구들에 대해 이야기 해보겠습니다.

## 브라우저
브라우저선택은 개발사이클에서 가장 중요한 부분입니다. Cocoa 앱이 Mac OSX 를 플랫폼으로 구동되듯이, Cappuccino 앱은 브라우저를 플랫폼으로 구동됩니다.
어떤 브라우저를 선택하든지 상관은 없지만, 다음의 4가지 주요 브라우저들((Firefox, Safari, Chrome 그리고 Internet Explorer) 의 특징 및 전용 디버깅 도구들을 살펴보겠습니다.

### Firefox
오늘날 많은 웹개발자들이 최신버전의 파이어폭스를 선택하고 있습니다.
그 가장 큰 이유중 하나는 Firebug extension 을 사용할 수 있다는 것입니다.
Firebug 는 예외 스텍 추적(trace), 성능에 주요 병목점을 찾기위한 Javascript 프로파일러, 그리고 XMLHTTPRequest 를 포함한 모든 http 트래픽 모니터링등 어플리케이션을 디버깅하는데 필요한 좋은 기능들을 많이 제공합니다.
만약 Firefox를 사용할 생각이라면 Firebug는 정말 유용한 add-on 이 될것입니다.

![](./@img/Ff_error.png)

대부분의 브라우저가 그렇듯이 Firefox 역시 포괄적인 에러로그를 제공합니다.
Firefox 의 에러 해석은 특정한 이슈나 문제가 있을것으로 보이는 코드가 어디에 있는지에 관해서 힌트를 얻는데 유용합니다. 그밖의 다른 예외상황 리포트 역시 일반적으로 다른 브라우저보다 더 좋은 편입니다.
심지어 개발을 위해 다른 브라우저를 선택하더라도, Firefox 를 곁에 두는것은 문제를 찾고 수정하는데 좋은 참고가 될것입니다.

![](./@img/Ff_net.png)

### Safari
기존버전의 Safari 는 빈약한 개발자도구를 가지고 있었기 때문에 그리 인기있는 브라우저는 아니었습니다.
그러나 다행히도 이런 문제는 대부분 해결되었습니다. 현재의 Safari에는 디버거, 프로파일러 그리고 http 모니터 기능을 포함한 좋은 Safari Web Inspector 가 추가되었습니다.
개발자도구 를 사용하려면 환경설정의 Advanced 섹션에 있는 “개발자메뉴를 메뉴바에 보이기” 를 활성화 하여야 합니다.

![](./@img/Sf_develop1.png)

Safari 는 Firefox 보다 조금 더 좋은 부분도 있습니다.
첫째, Javascript 성능이 더 우수합니다. 현대의 대부분의 브라우저들이 Javascript의 성능이 우수한 편이지만 빠른 개발 사이클, 새로고침, 바뀐 코드의 확인 등에서 고려해 보았을때 더 빠른 속도를 가질 수록 개발에 유리합니다.
둘째, 아마도 더 중요한 부분일 듯 한데 Safari 는 XMLHTTPRequest 와 same origin 정책등에 있어서 Firefox 와는 약간 다른 보안 모델을 갖고 있습니다.
Safari 에서 로컬에서 파일을 실행할때는 (예, file:///MyFolder/MyApplication/index.html) 어떠한 사이트에도 접근요청이 허용됩니다. 이는 mac의 dashboard 위젯이 외부 사이트를 접근할 수 있게 하기 위한 디자인이지만, 어플리케이션 개발에 있어서도 유용합니다.
여러분은 same origin 정책에 대해 걱정하지 않은채 여러분의 워킹카피를 웹서버와 분리하거나 개발용 환경이나 로컬개발을 위한 백엔드 환경만을 구축하는것으로 개발을 진행할 수 있습니다.

이는 Firefox3 에서도 환경설정을 통해 유사한 환경을 구축할 수 있습니다 – FireFox 에서 로컬개발 활성화하기
Firefox 4 에서는 별다른 설정이 없어도 Safari 처럼 로컬 XMLHttpRequest 를 사용할 수 있습니다.

![](./@img/Sf_inspector.png)

### Chrome
구글의 Chrome 은 Safari 의 렌더링 엔진과 같은 Webkit 엔진을 사용한다는 점에서 Safari 와 많은 부분 비슷합니다.
그러나 가장 큰 차이가 있는데 바로 보안정책에 관한 것 입니다.
Safari 와는 다르게 Chrome 은 file:/// 로 부터 로컬파일을 실행할때 원격 호스트로의 접근을 허용하지 않습니다. (same origin 정책 위반).
이뿐만 아니라 나아가 로컬파일로 부터 XHR request 를 다른 file:/// 주소로 보내는것도 허용하지 않습니다. 왜냐하면 Cappuccino 는 여러분의 어플리케이션을 로드하는데 XHR 요청을 사용하며 Chrome 은 Cappuccino 를 파일시스템으로 부터 바로 실행해 주지 않습니다. 대신에 Apache 와 같은 웹서버를 통해 실행하여야 합니다. 보안정책에 관한것은 구글의 버그 트레커에서 좀 더 알아볼 수 있습니다.

만약 Cappuccino 툴이 설치되어 있다면 Narwhal 이라 불리는 Javascript 커멘드라인 환경 프로그램도 설치가 되어 있습니다. 여러분은 Narwhal 의 jackup 을 사용하여 현재 디렉토리를 서버로 만들 수 있습니다.

```
jackup -e "require('jack/file').File('.')" -E deployment
```

### Internet Explorer
Cappuccino 는 IE 에서도 매우 잘돌아갑니다.
그리고 여러분은 IE 에서도 자주 테스트 하게 될것입니다. IE를 사용해 개발환경을 갖추는것은 그리 추천하는 방법은 아닙니다.
IE의 Javascript 인터프리터는 현존하는 브라우저중 가장 느리고 최악의 디버깅 툴을 가지고 있습니다.

IE는 Built-in 된 기본 개발자도구를 가지고 있지만 많이 부족한 편입니다.
IE를 사용해서 반드시 디버깅을 해야한다면 개발 경험을 좀 더 낫게 만들 수 있는 툴을 알아두는게 좋습니다.
하나는 [Companion.js](http://www.my-debugbar.com/wiki/CompanionJS/HomePage) 입니다. 이 툴은 DOM Inspector 와 JS 콘솔을 지원합니다.
또 다른것으로는 Companion.js 의 일부분을 좀 더 낫게 만들기 위해 필요한 [MS Script Debugger](http://www.microsoft.com/downloads/en/details.aspx?familyid=2f465be0-94fd-4569-b3c4-dffdf19ccd99&displaylang=en) 도 있습니다.

![](./@img/Ie_companion.gif)

## 개발도구
Cappuccino 개발도구는 여러분이 익숙한 어떠한 개발툴을 사용하셔도 상관 없습니다.
최소한의 Javascript 네이티브 기능만을 지원하는 개발툴을 사용하고 계셔도 상관없습니다. Objective-J 는 Javascript 와 매우 친숙한 언어이기 때문입니다.
만약, Objective-J 에 대한 코드 하이라이팅을 사용하고 싶다면 아래의 에디터는 최고의 선택이 될것입니다.

### Objective-J Add-on 을 지원하는 개발도구
* [Atalas – 280North 에서 제공하는 Cappuccino 개발도구](http://280atlas.com/)
* [SubEthaEdit Objective-J 모듈](http://cappuccino.org/files/objj.mode.zip)
* [Coda – 맥용 웹개발툴(Objective-J 기본 지원)](http://panic.com/coda)
* [TextMate Objective-J 번들](http://cappuccino.org/files/objj.tmbundle.zip)
* [VIM Objective-j 코드 하일라이터](http://cappuccino.org/files/objj.vim)
* [Xcode Cappuccino 플러그인](http://cappuccino.org/files/Cappuccino_Developer_Tools.pkg)

## 로그 남기기
로깅은 디버거가 사용 불가능한 상황에서 특히 중요한 개발프로세스의 한 부분입니다.
수년간 사람들은 웹어플리케이션의 다양한 로깅 방법들에 적응되어 왔습니다. 이런 방법중 가장 마음에 드는 방식을 택하면 됩니다.

Cappuccino 는 전용 빌트인 로거를 가지고 있습니다. CPLog 는 Apache의 log4j 의 모델을 따르고 사용하기 쉽습니다. 여러분의 코드에 어느곳에라도 쉽게 CPLog(“a message”) 를 호출하면 로그에 쌓입니다. 또한 CPLog.error(“an error”), CPLog.debug(“a debug message”) 그리고 그밖의 다른것도 사용할 수 있습니다.
로그들은 팝업창에 보여지게 되어있지만, 기본적으로는 비활성화 되어 있습니다. 이를 활성화 하기 위해서는, index.html 대신에 index-debug.html 을 불러오면 됩니다. 만약 다음과 같은 환경에서 개발한다면

```
http://localhost/NewApplication/
```

이 url 대신에 아래와 같이 입력하면 디버그 모드로 동작하게 됩니다.

```
http://localhost/NewApplication/index-debug.html
```
기본적으로 index.html 을 통해서 어플리케이션에 접근하면 Cappuccino Framework 는 프레임워크상의 javascript 소스가 minify 된 버전을 제공합니다.

디버깅을 위해서 index-debug.html 을 통해 접근하면 프레임워크는 minify 되지 않은 버전을 제공함으로써 디버깅 및 소스추적에 도움을 주게 됩니다.

Cappuccino 콘솔은 CPLogRegister(CPLogPopup) 을 코드내에서 호출하면 활성화 되며, CPLog 를 사용해서 로그를 남길 수 있게 됩니다.

## 컴파일러
Java, C++ 혹은 Objective-C 를 사용했던 개발자들은 컴퍼일러가 문법이나 타입오류를 쉽게 찾아내어 보여주기때문에 처음 Objective-J 와 Javascript 로 넘어오면 혼한스러워 합니다.
웹 클라이언트 환경에서는 이런 문제들을 찾는데 있어서 컴파일러에 의존할 수 없습니다.
대부분 이런 문제점은 어플리케이션 최초로딩시 혹은 특정 이벤트 발생시 해당 브라우저의 Javascript 콘솔에 찍혀있는 오류 메세지를 통해 발견하게 됩니다.
Safari 및 Chrome 의 개발자도구나 Firefox Firebug 등의 도구등를 사용하면 좀 더 향상된 오류정보를 접할수 있습니다.

이렇게 오류메세지를 분석하는 과정은 여러분의 코드가 여전히 제대로 작동하는지 확실히 파악하는데 매우 중요합니다. 여러분은 에러가 발생하는 코드가 어디에 있는지를 알아내기 위해 어떠한 단서도 없이 300코드 라인을 일일이 확인하길 원하지는 않을겁니다. 이는 Objective-J 가 실제 오류가 발생하는 라인넘버를 보고하는데 어려움이 있다는 사실때문에 더 악화될 수도 있습니다.
그래서 여러분은 여러분의 코드를 자주 실행해보고 에러 콘솔을 확인하고 문제를 해결하는데 사용할 수 있는 다른 툴들을 동원해야합니다.

지금까지 Cappuccino 를 시작하기 위한 기본준비를 마쳤습니다.
다음 포스트에서는 Objective-J 문법의 기본적인것에 대해 이야기 하도록 하겠습니다. ^^