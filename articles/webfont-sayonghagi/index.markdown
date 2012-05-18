{
    "title": "Webfont 사용하기",
    "author": "frends",
    "date": "2012-05-17T09:13:37.509Z",
    "categories": [
        "html5"
    ],
    "tags": [],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-05-17T09:13:37.509Z",
    "status": "public",
    "important": false,
    "advanced": {}
}

최근, 웹폰트를 사용해보고 싶어서 안달이 난 나머지 폰트를 분해해 재조립 해보기도 하고,
어느정도 사이즈가 줄어들때로 줄어드니깐 이제 Cross-browsing에 안달이나 @font-face로 별짓 다 해봤다.
(이 글도 자세히 쓰려다가 중간에 날라가서 다시 쓰는 삽질도 하고 있다. T_T)

그래서 얻은 결론은?

해볼만하다 였다.


## @font-face 지원 브라우저 및 지원 포멧

<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="155" valign="top">
<p><strong><span style="font-size: small;"><span style="color: #993300;">Browser</span></span></strong></p>
</td>
<td width="130" valign="top">
<p><strong><span style="font-size: small;"><span style="color: #993300;">Version</span></span></strong></p>
</td>
<td width="142" valign="top">
<p><strong><span style="font-size: small;"><span style="color: #993300;">Font Type</span></span></strong></p>
</td>
</tr>
<tr>
<td rowspan="2" width="155" valign="top">
<p><span style="font-size: small;">Internet Explorer</span></p>
</td>
<td width="130" valign="top">
<p><span style="font-size: small;">6,7,8</span></p>
</td>
<td width="142" valign="top">
<p><span style="font-size: small;">EOT</span></p>
</td>
</tr>
<tr>
<td width="130" valign="top">
<p><span style="font-size: small;">9</span></p>
</td>
<td width="142" valign="top">
<p><span style="font-size: small;">EOT, WOFF</span></p>
</td>
</tr>
<tr>
<td rowspan="2" width="155" valign="top">
<p><span style="font-size: small;">Firefox</span></p>
</td>
<td width="130" valign="top">
<p><span style="font-size: small;">3.5</span></p>
</td>
<td width="142" valign="top">
<p><span style="font-size: small;">TTF/OTF</span></p>
</td>
</tr>
<tr>
<td width="130" valign="top">
<p><span style="font-size: small;">3.6+</span></p>
</td>
<td width="142" valign="top">
<p><span style="font-size: small;">TTF/OTF, WOFF</span></p>
</td>
</tr>
<tr>
<td rowspan="2" width="155" valign="top">
<p><span style="font-size: small;">Chrome</span></p>
</td>
<td width="130" valign="top">
<p><span style="font-size: small;">4+</span></p>
</td>
<td width="142" valign="top">
<p><span style="font-size: small;">TTF/OTF, SVG</span></p>
</td>
</tr>
<tr>
<td width="130" valign="top">
<p><span style="font-size: small;">5+</span></p>
</td>
<td width="142" valign="top">
<p><span style="font-size: small;">TTF/OTF, SVG, WOFF</span></p>
</td>
</tr>
<tr>
<td rowspan="2" width="155" valign="top">
<p><span style="font-size: small;">Safari</span></p>
</td>
<td width="130" valign="top">
<p><span style="font-size: small;">3.1+<br>
</span></p>
</td>
<td width="142" valign="top">
<p><span style="font-size: small;">TTF/OTF, SVG<br>
</span></p>
</td>
</tr>
<tr>
<td width="130" valign="top">
<p><span style="font-size: small;">Mobile(iPhone/iPad)</span></p>
</td>
<td width="142" valign="top">
<p><span style="font-size: small;">SVG</span></p>
</td>
</tr>
</tbody>
</table>

## 파일 크기 및 대역폭

TTF를 subset 하지 않고 단순 사이즈를 비교해보자.
아래는 Daum_Regular.ttf(12261 Glyph)를http://www.fontsquirrel.com/fontface/generator 통해서 변환한 크기다.

<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="213" valign="top">
<p><strong><span style="font-size: small;"><span style="color: #993300;">Format</span></span></strong></p>
</td>
<td width="213" valign="top">
<p><strong><span style="font-size: small;"><span style="color: #993300;">Size</span></span></strong></p>
</td>
</tr>
<tr>
<td width="213" valign="top">
<p><span style="font-size: small;">TTF(Original)</span></p>
</td>
<td width="213" valign="top">
<p><span style="font-size: small;">1.2M</span></p>
</td>
</tr>
<tr>
<td width="213" valign="top">
<p><span style="font-size: small;">TTF/OTF</span></p>
</td>
<td width="213" valign="top">
<p><span style="font-size: small;">807kb</span></p>
</td>
</tr>
<tr>
<td width="213" valign="top">
<p><span style="font-size: small;">EOT</span></p>
</td>
<td width="213" valign="top">
<p><span style="font-size: small;"><strong>336kb</strong></span></p>
</td>
</tr>
<tr>
<td width="213" valign="top">
<p><span style="font-size: small;">WOFF</span></p>
</td>
<td width="213" valign="top">
<p><span style="font-size: small;"><strong>368kb</strong></span></p>
</td>
</tr>
<tr>
<td width="213" valign="top">
<p><span style="font-size: small;">SVG</span></p>
</td>
<td width="213" valign="top">
<p><span style="font-size: small;">1.5M</span></p>
</td>
</tr>
<tr>
<td width="213" valign="top">
<p><span style="font-size: small;">SVGZ(Gzipped version   of SVG)</span></p>
</td>
<td width="213" valign="top">
<p><span style="font-size: small;"><strong>414kb</strong></span></p>
</td>
</tr>
</tbody>
</table>

여기서 원본 TTF를 Subset하면 훨신 더 작은 사이즈의 폰트를 얻을 수 있겠다.
(구글링하다보니 정리 잘 된 Subset을 http://bystyx.com/blog/?p=165 에서 찾을 수 있었다.)



## 렌더링 차이

Plain Font에는 렌더링 차이가 많지 않은데, Anti-Aliased Font의 경우, Windows에서는 Clear Type 을 설정하지 않으면(솔직히 하나 안하나) 정말 보기 힘들정도다.
이 문제는 Platform Dependent한 문제로, Mac용 브라우저에서는 모두 동일하게 보이는 폰트가 윈도우에서는 브라우저 별로 제각각이다.

CSS 패치(text-shadow)를 한 Chrome이 싱크로율 100%로 깔끔하게 보여줬고, 그 외 브라우저(IE678,  FF, Safari, Opera)에서는 끔찍했다.
이를 해결하기 위해 다양한 핵이 존재했지만, 이를 적용해도 100% 똑같이 일치하지는 않는다.

아래는 Windows 7에서 Daum_Regular.ttf를 사용해 브라우저별로 비교를 해보았다.
(시안에는 letter-spacing:-1px이 적용되어있지만, 브라우저 비교삿에는 이를 제외했다. 그리고 핵을 사용하지 않은 스크린샷이다.)



인터넷에 떠도는 각종 핵(IE에서 zoom을 이용한 핵/PNG 이용 핵 등등)을 사용해서 싱크로율을 맞추려고 했지만,
정말 맞추기 힘들고, 맞추는 시간에 비해 얻는 퀄리티가 적어서 실망스러웠었다. 그나마 영문자는 좀 나은데, 한글은 정말...T_T

결론은, Windows에서 Browser로 아름다운 윤곽선 웹폰트 보는건 이미지나 플래시 폰트를 사용하지 않고서는 불가능이다!

![](./@img/browser.gif)

## @font-face 대안은?

사실...정말 @font-face 대안은 찾기 싫었다.
script를 쓰지 않고 CSS만 가지고 아름다운 웹폰트를 쓰고 싶었지만 아직은 역부족인것 같아서 별 수 없이 대안을 찾기로 결심했다.

이러한 플랫폼 의존적인 렌더링 문제 때문에 여러 사이트에서 유/무료 @font-face 대안 솔루션을 제공하고 있다.
Comparison of web fonts solutions(http://t.co/4sZBVFG)을 보면, 현존하는 웹폰트 솔루션의 장단점이 잘 나와있다.(꼭 한번 보기 바란다.)

**하지만…**

구매한 폰트가 있다(또는 공개된 폰트 중 맘에 드는게 있다)
Flash가 느려서 싫다
지금까지 필자가 써본 것 중, 위의 두가지를 만족시키고 가장 만족스러웠던 품질을 얻은건 Cufón 이었다.
Cufón의 단점이라고 한다면... 파일 사이즈인데, 텍스트파일이 원본 폰트 사이즈만큼 크다는것이다.

**하지만!**

이를 Subsetting하고, (mod_gzip 사용해서) gzip 압축하면 220kb대 까지 떨어진다.
그리고 브라우저별로 거의 일치하는 싱크로율을 보여주었다. 물론 속도도 만족스러웠다.

최근에는 text-selection까지 지원하고 있다고 하니 더더욱 만족스러워 보였다. 현재 IE9까지 지원하고 있다고 한다.(1.09i)



## 결론

정말 간단하게 요약하면...

Windows에서는 Cufón을 쓰고, Mac/Linux에서는 @font-face를 쓰자.
플랫폼을 나누기 귀찮으면 Cufón으로 통일하자.
1, 2번이 영 맘에 안든다면 Flash 폰트를 사용하라. (참고로 Flash를 이용한 Font는 Dynamic Text 지원으로 text-selection도 된다.)
마지막으로...


### @font-face 사용과 관련해서 반드시 읽어보아야 할 URL
* [http://paulirish.com/2009/fighting-the-font-face-fout/](http://paulirish.com/2009/fighting-the-font-face-fout/)
* [http://paulirish.com/2009/bulletproof-font-face-implementation-syntax/](http://paulirish.com/2009/bulletproof-font-face-implementation-syntax/)
* [http://paulirish.com/2010/font-face-gotchas/](http://paulirish.com/2010/font-face-gotchas/)


