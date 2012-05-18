{
    "title": "Knockout.js Install",
    "author": "frends",
    "date": "2012-05-18T01:09:12.602Z",
    "categories": [
        "javascript"
    ],
    "tags": [],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-05-18T01:09:12.602Z",
    "status": "public",
    "important": false,
    "advanced": {}
}

## 설치

Knockout 핵심 라이브러리는 다른 어떤 자바스크립트 라이브러리에 의존하지 않는다.  기존 어플리케이션에서 KO를 사용하기 위해 다음과 같은 절차를 따른다:

1. [다운로드 페이지](http://github.com/SteveSanderson/knockout/downloads)에서 최신버전의 Knockdown 자바스크립트 파일을 내려받는다. 일반적으로 개발용과 서비스용 모두 압축된 버전을 사용하자 (`knockout-x.x.js`).디버깅을 하려면 압축되지 않은 버전을 사용하자.  압축된 버전보다 파일 용량이 크다. (`knockout-x.x.debug.js`). 압축된 버전과 동작은 같으나 변수명을 줄이지 않고 주석이 제거되지 않았으며 내부 API를 숨기지 않은 버전이기 때문에 읽고 디버깅 하기 쉽다.
2. `<script>` 태그를 사용하여 HTML 페이지내에 소스코드를 읽어들인다.

단지 이 두가지만 하면 설치가 완료된다.

 

### 필요시 템플릿 기능 사용

템플릿 기능을 사용하려면 두개의 자바스크립트 파일이 필요하다.  KO의 기본 템플릿 엔진은 jQuery 기반의 jquery.tmpl.js를 사용한다. 다음에 표시된 스크립트 파일을 내려받고 `<script>`태그를 사용하여 Knockdown.js를 읽어들이기 전에 템플릿 엔진을 먼저 읽어들여야 한다:

* [jQuery 사이트](http://docs.jquery.com/Downloading_jQuery),에서 버전 1.4.2 이상의 파일을 내려받는다.
* jquery-tmpl.js — [잘 동작 되는 버전](http://github.com/downloads/SteveSanderson/knockout/jquery.tmpl.js) 을 사용하거나 [jquery.tmpl](http://github.com/jquery/jquery-tmpl) 프로젝트 홈페이지 에서 새로운 버전을 확인해보자.
잘 모를경우, 순서에 따라 다음과 같이 세개의 스크립트를 읽어들인다:

```html
<script src="jquery-1.4.2.min.js" type="text/javascript"></script>
<script src="jquery-tmpl.js" type="text/javascript"></script>
<script src="knockout-1.2.1.js" type="text/javascript"></script>
```

(실제 파일이 있는 위치를 지정하는 것을 잊지 말자)