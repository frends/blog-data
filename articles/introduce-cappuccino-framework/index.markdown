{
    "title": "Introduce Cappuccino Framework",
    "author": "frends",
    "date": "2012-05-17T09:23:43.593Z",
    "categories": [
        "javascript"
    ],
    "tags": [],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-05-17T09:23:43.593Z",
    "status": "public",
    "important": false,
    "advanced": {}
}


![](./@img/cappuccino-icon.jpeg)

> Cappuccino is an open source framework that makes it easy to build desktop-caliber applications that run in a web browser.

## 카푸치노는 무엇인가?

Cappuccino 는 280north 사에서 개발한 웹 어플리케이션 프레임워크로써 테스크탑 스타일의 Look & Feel 을 가진 소프트웨어를 개발하기 위한  오픈소스 프로젝트 입니다.
Cappuccino 를 사용하여 개발하게 되면, 개발자는 HTML, CSS, JS 의 스파게티 코드에 더이상 고민하지 않아도 됩니다.
DOM의 구성이나, 브라우저의 버그에 대한 예외처리등에 신경쓰지 않아도 되고, Drag & Drop, Copy & Paste, Undo/Redo, 리치한 그래픽 처리등을 구현하는것을 걱정하는 대신에 어플리케이션의 고유기능에 좀 더 집중해서 개발할 수 있게 도와줍니다.

 ![](./@img/280NorthLogoSmall.png)

다른 프레임워크와 비교하면 무엇이 다른가?

Cappuccino 는 웹사이트를 만들거나 기존의 사이트를 좀 더 "동적" 으로 만들기 위해 디자인 된것이 아닙니다.
이러한 작업  (동적인 웹사이트) 에는 이미 존재하는 prototype.js 나 jQuery 와 같은 훌륭한 프레임워크들로도 충분합니다.
그러나 이들은 DOM 처리와 같은 렌더링분야에 주로 포커스가 맞추어져 있어 웹어플리케이션을 구조적으로 개발하는데 적합한 프레임워크는 아닙니다.

![](./@img/webapps1-300x102.png)

Cappuccino 는 기존 프레임워크들과는 달리 웹 어플리케이션을 구조적으로 개발하는데 초점이 맞추어져 있습니다. 따라서, 일반적인 웹페이지를 만드는 용도로 사용하려면 적합하지 않습니다.
HTML, CSS, 그리고 prototype.js 나 jQuery 를 사용한 전통적인 방식으로 Desktop 어플리케이션과 비슷한 경험을 웹에서 제공할 수 있도록 하기 위해서는 매우 방대한 양의 코드를 작성하여야 합니다.

그러나, Cappuccino 를 사용하면 개발자는 HTML을 알 필요가 없고, CSS 를 작성할 일도 없으며, DOM 을 직접 작업할 일은 영영 없을 것입니다.

![](./@img/CappucinoApp.png)

Cappuccino 는 오직 한가지의 기술만을 요구합니다. 바로 Objective-J 와 Cappuccino API 입니다.
그리고 이런 기술들은 이미 존재하는 웹기술들의 깊은 이해를 바탕으로 구현되었습니다.
차세대 웹 어플리케이션을 개발하기위한 기술들은 흔히들 Javascript 2.0 이나 HTML5 와 같은 새로운 표준일 것이라고 생각하지만, 문제는 이러한 표준들은 너무 느리게 수립되고 있습니다.

Cappuccino 는 이런 이론적인 미래가 아니라 지금 당장 사용할 수 있는 기술입니다.

 
## Objective-J 란 어떤 언어인가?

![](./@img/skitched-20100401-0030591.png)

Objective-J 는 Objective-C 에 기반을 둔 새로운 객체지향 언어입니다.
Objective-J 는 자바스크립트의 상위집합(superset) 입니다.

이 말은 어떤 유효한  Javascript 코드는 Objective-J 의 문법내에서도 유효하다는 의미 입니다.
Objective-J 는 Javascript로 작성된 Objective-J.js 가 구문해석기 역할을 함으로써 동작합니다.
즉, Objective-J 는 Javascript 를 사용하여 컴파일되는 언어입니다. 그리고, Javascript 와 달리 Objective-J 로 개발하면 크로스브라우징을 신경쓸 필요가 없습니다.

 

## Cappuccino 를 사용해서 무엇을 할 수 있는가?

Objective-J 를 사용하여 클래스기반 상속 프로그래밍을 할 수 있습니다.
Interface Builder 와 같이 Cappuccino 나 다른 써드파티에서 제공하는 IDE 들을 사용하면 View 를 완벽히 분리할 수 있습니다. 또한 View 의 세부코드도 Objective-J 로 조작 가능하므로 개발시 언어를 일원화 할 수 있습니다.
SVG 나 Canvas 등을 활용한 리치한 그래픽 라이브러리를 손쉽게 구현할 수 있습니다.
 

## Cappuccino 쇼케이스

Cappuccino 로 작성된 어플리케이션은 매우 수려한 UI 를 가지고 있습니다. 또한, 정말 웹이라고는 생각하기 힘들정도로 Desktop 어플리케이션에 버금가는 사용성을 가지고 있습니다. Cappuccino 로 작성하고 서비스되고 있는 몇가지 쇼케이스를 보신다면 당장 공부를 시작하고 싶으실 지도 모릅니다..

[280 Slides](http://280slides.com/)

첫번째 Cappuccino 어플리케이션으로써 280 north 사에서 개발된 Keynote 스타일의 웹 슬라이드 어플리케이션

![](./@img/280slides.png)
 

[Mockingbird](http://gomockingbird.com/)

웹사이트나 어플리케이션을 프로토타이핑할때 Mock-up 작업시 쉽게 스케치하고, 공동 작업자와 공유할 수 있는 온라인 Mock-Up 도구

[PicsEngine](http://picsengine.com/home/)

언제 어디서나 개인의 사진을 저장하고 관리할 수 있는 온라인 사진 관리도구

![](./@img/screen4.png)

 
[Enstore](http://www.enstore.com/)

Checkout 과 AccountEdge 사가 공동으로 제공하는 e-commerce 플랫폼

![](./@img/enstore.png)
 

[GitHub Issues](http://githubissues.heroku.com/)

GitHub 이슈트래커를 Cappuccino 를 사용하여 front-end 를 구성한 버전

![](./@img/Screen-shot-2010-05-13-at-10.58.15-AM-copy.png)


[TimeTable](http://timetableapp.com/)

프리랜서 혹은 기업용 온라인 시간관리 툴.. 업무시간, 비율, 프로젝트에 들어간 비용등을 관리해주는 툴

![](./@img/header_left.png)

 
[Almost.at](http://almost.at/)

실제 특정 이벤트에 대한 데이터를 실시간으로 수집하고 정리하여 보여주는 사이트

![](./@img/almostatmed.jpeg)
