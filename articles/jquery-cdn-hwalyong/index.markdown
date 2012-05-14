{
    "title": "jQuery CDN 활용",
    "author": "frends",
    "date": "2012-05-14T07:08:43.361Z",
    "categories": [
        "jQuery"
    ],
    "tags": [],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-05-14T07:08:43.361Z",
    "status": "public",
    "important": false,
    "advanced": {}
}

[Jon Galloway](http://weblogs.asp.net/jgalloway/default.aspx)씨는 [Using CDN Hosted jQuery with a Local Fall-back Copy](http://weblogs.asp.net/jgalloway/archive/2010/01/21/using-cdn-hosted-jquery-with-a-local-fall-back-copy.aspx)라는 글에서 대안적인 방법과 함께 jQuery CDN을 사용하는 방법을 소개하였습니다.

jQuery같은 공통 자바스크립트 라이브러리를 자신의 서버가 아닌 CDN을 사용해야 하는 여러가지 이유가 있는데 [Dave Ward가 3가지 주요한 이유를 설명](http://encosia.com/2008/12/10/3-reasons-why-you-should-let-google-host-jquery-for-you/)하였습니다.

* 지연 감소 - CDN은 정적파일을 여러물리적 서버로 분산하여 요청이 들어왔을때 가장 가까운 네트워크에서 다운로드 할 수 있도록 해줍니다.
* 병렬 증가 - 쓸데없는 서버부하를 피하려고 웹브라우져들은 동시에 연결할 수 있는 커넥션의 수를 제한하고 있습니다. 브라우져에 따라 다르지만 호스트명당 2개정도 입니다. CDN을 사용하면 사이트에 대한 요청이 하나 줄어 병렬로 다운로드 받을 수 있습니다.
* 향상된 캐싱 - 잠재적으로 가장 좋은 이익은 사용자가 다운로드를 받지 않을 수도 있다는 것입니다. 다른 사이트에서 받아왔으면 받지 않아도 됩니다.

하지만 CDN이 불능상태에 빠질때에 대한 한가지 단점이 존재합니다. CDN은 안정적이지만 아래와 같은 2가지 경우가 CDN을 사용하지 못하는 대표적입니다.

* 오프라인 개발
* 데모시연과 샘플코드

아래 코드로 간단하게 해결할 수 있습니다.

```js
<script type="text/javascript" src="http://ajax.microsoft.com/ajax/jquery/jquery-1.3.2.min.js"></script>
<script type="text/javascript">// <![CDATA[
    if (typeof jQuery == 'undefined') {
        document.write(unescape("%3Cscript src='/Scripts/jquery-1.3.2.min.js' type='text/javascript'%3E%3C/script%3E"));
    }
// ]]></script>
```

