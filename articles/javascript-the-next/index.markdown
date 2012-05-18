{
    "title": "Google's The Next JavaScript",
    "author": "Rhio Kim",
    "date": "2012-05-18T00:25:41.632Z",
    "categories": [
        "javascript"
    ],
    "tags": [],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-05-18T00:25:41.632Z",
    "status": "public",
    "important": false,
    "advanced": {}
}

## 구글과 자바스크립트

차세대 자바스크립트라는 글을 쓰기에 앞서 구글에 대해서 간략히 정리가 필요하다.
구글은 ECMA-262 4th 스펙 표준화에 참여하였다. 뿐만아니라 많은 브라우저 벤더에서 참가하였다.

이때의 프로젝트 명은 Harmony!

브라우저 벤더들이 대부분이였고 Adobe 가 참여하였었다.  많은 이야기가 메일링으로 오갔으며 스펙의 분량은 점점 넓어지고 결국 실패로 돌아가게 되었습니다.  하지만 어느 정도의 뼈대가 갖춰졌었는지 Adobe 는 최종 권고가 나지 않는 스펙의 일부를 ActionScript 에 적용하였고. ActionScript 3.0 으로 2.x 이하 버젼보다 좀더 객체 지향적인 언어로 탈바뀜하게 되었습니다.

이때 구글은 무엇을 했을까? HTML5 의 주도적인 표준화에 참여하였고 JavaScript 의 표준 사양인 ECMA-262 표준에도 공헌하였다.

물론 이는 앞으로의 웹 시장에 굉장히 중요한 역할을 할것이라는 것을 충분히 알았고 또한 자사의 다양한 플랫폼 개발을 위한 Go, Python 과 같이 기본 언어가 될 것이기에 나름 철학을 담아 노력을 해왔던 것으로 판단된다.

물론 실패로 돌아갔던 Harmony 프로젝트이지만 구글은 미련이 남았고, 그때의 불 필요한 기업간의 의견 마찰을 피하고 좀더 나은 방식을 택한 것이라 보여진다.

자바스크립트는 오픈 웹 기술의 일부이고 이젠 커뮤니티의 힘으로도 충분히 표준안을 정립해 갈 수 있다고 생각한 것이다.

사실 개인적인 잡담이고 더글러스 크록포드도 이와같이 이야기 했지만 ECMA 표준 재단은 정신을 차리고 열의를 보였으면 한다는 말을 하고 싶다.

그래야 나같은 자바스크립트를 주로 하는 프론트앤드 기술자들이 플레이 그라운드를 좀 누빌것 아닌가?

## 다음 세대의 자바스크립트

몇일전 구글은 트레이서 컴파일러 프로젝트를 발표했다. 물론 google code 를 통해 오픈 소스 기반으로 진행된다.

내용을 살펴보니 차세대 자바스크립트라는 명목하에 Harmony 프로젝트가 실패로 돌아가 그때 협의된 결과를 가지고 커뮤니티의 힘을 빌어 새롭게 진행하고자 하는 것이 살짝 엿보인다.

일단 목표는 자바스크립트의 향상된 언어로의 새로운 기능들이 추가되고 좀더 향상키고 브라우저에서 직접적으로 컴파일 될 수 있고 또한 이로써 속도를 향상 시키는데 있다.

## Getting Started

간단히 hello world 를 특정 엘리먼트에 표시하는 데모는 다음과 같다.
 
```js
...
<script type="text/traceur">// < ![CDATA[
// < ![CDATA[
      class Greeter {
        new(message) {
          this.message = message;
        }

        greet() {
          let element = document.querySelector('#message');
          element.innerHTML = this.message;
        }
      };

      let greeter = new Greeter('Hello, world!');
      greeter.greet();
// ]]></script>
...
```

위의 코드에서 평범하지 않는 ECMAScript 가 두군데 있다. 그것은 class 정의와 let 변수 설정 부분이다. 또 하나 특별한게 있다면 script 태그의 type 이 text/traceur 이라는 것이다.

 
## Compiling

```
...
<script src="http://traceur-compiler.googlecode.com/svn/branches/v0.10/src/traceur.js" type="text/javascript"></script> ...
```

컴파일을 해보고자 한다면 위와 같이 traceur.js 를 text/javascript 형태로 추가를 해주어야 하며 traceur.js 는 트레이서 자바스크립트 컴파일러 의 진입점이고 이 라이브러리 자체만으로는 아무것도 할 수 없고 bootstrap.js 를 함께 추가해야 traceur 코드를 컴파일할 수 있게 된다.  

```
 ...    
 <script src="http://traceur-compiler.googlecode.com/svn/branches/v0.10/src/traceur.js" type="text/javascript"></script>
<script src="http://traceur-compiler.googlecode.com/svn/branches/v0.10/src/bootstrap.js" type="text/javascript"></script>
....
 ```

### Language Features

트레이서에서 제공하는 모든 분법은 ECMA-262 4th 스펙에서 모두 논의되었던 내용을 기반으로 하고 있다.
부연 설명은 없고 제공하는 스펙 문서를 통해 습득하기를 권장한다.


Traceur Language Feature : [http://code.google.com/p/traceur-compiler/wiki/LanguageFeatures](http://code.google.com/p/traceur-compiler/wiki/LanguageFeatures)

ECMA-262 4th proposal : [http://wiki.ecmascript.org/doku.php?id=harmony:proposals](http://wiki.ecmascript.org/doku.php?id=harmony:proposals)
 

### 앞으로

국외 커뮤니티에서는 자바스크립트의 새로운 이름이 필요하지 않겠냐는 이야기(JavaScript needs a new name. What should it be?)도 오가기도 하고 있고 구글의 이런 실험은 다음 자바스크립트에 어떤 변화를 가져다 줄지 기대되고 개인적으로는 단순히 실험에서 끝나지 않고 많은 공헌자에 의해서 발전되고 개발자들에게 친근한 언어로 많이 갖춘 언어로 발전하였으면 한다.

가장 중요한건 표준 사양이 되어야 하고 스펙 문서도 좀 발전되었으면 한다는 것. ^-^/


## Reference

- [http://code.google.com/p/traceur-compiler/](http://code.google.com/p/traceur-compiler/)
- JSConf 2011 : [http://traceur-compiler.googlecode.com/svn/branches/v0.10/presentation/index.html#1](http://traceur-compiler.googlecode.com/svn/branches/v0.10/presentation/index.html#1)
- Traceur Demo : [http://traceur-compiler.googlecode.com/svn/branches/v0.10/demo/repl.html](http://traceur-compiler.googlecode.com/svn/branches/v0.10/demo/repl.html)

### See More
글을 올려는 시점에서 국외에서도 포스팅이 있어 함께 읽어보면 좋을 것 같다.
[http://www.i-programmer.info/news/98-languages/2395-javascript-creator-talks-about-the-future.html](http://www.i-programmer.info/news/98-languages/2395-javascript-creator-talks-about-the-future.html)

이 친구 이번 JSConf 2011에서 차세대 자바스크립트에 대한 발표를 했고 역시 차세대 표준화에 실패를 번복해서는 안된다고 글의 마지막에 제안하고 있다. 동감하는 바이다.