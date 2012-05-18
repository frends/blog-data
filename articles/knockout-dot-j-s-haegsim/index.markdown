{
    "title": "Knockout.js 핵심",
    "author": "frends",
    "date": "2012-05-18T01:13:13.037Z",
    "categories": [
        "javascript"
    ],
    "tags": [],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-05-18T01:13:13.037Z",
    "status": "public",
    "important": false,
    "advanced": {}
}

## 관측대상 (Observables)

원문 : [http://knockoutjs.com/documentation/observables.html](http://knockoutjs.com/documentation/observables.html)

Knockout을 제작할 때, 세개의 핵심 사항이 목표였다:

1. 관측대상과 의존성 추적 (Observables and dependency tracking)
2. 바인딩 선언 (Declarative bindings)
3. 템플릿 사용 (Templating)

이 페이지에서는 이 세가지중 처음 것을 배울 것이다. 그전에 MVVM 패턴과 뷰모델에 대한 개념을 먼저 설명한다.

## MVVM과 뷰모델

모델-뷰-뷰모델(MVVM)은 사용자 인터페이스 작성에 쓰이는 디자인 패턴이다. MVVM은 복잡해질 가능성이 큰 UI를 세개의 다른 부분으로 나누어 간결함을 유지토록 한다.

* 모델: 어플리케이션이 저장하는 데이터. 이 데이터는 비즈니스 영역에서 객체와 행위를 나타내며 (예: 은행 계좌의 금융 거래), 이 데이터는 UI에 의존성이 전혀 없다.  일반적으로 KO 어플리케이션은 저장된 모델 데이타를 읽고 쓰기 위해서 서버측 코드 실행을 하는 AJAX 호출을 한다.

* 뷰모델: UI에 나타나는 데이터와 행위의 순수 코드 표기. 예를 들어, 항목 편집기를 구현할때 뷰모델은 항목 리스트를 담고 있는 객체와 항목을 추가/삭제하는 메소드를 말한다.
이것은 UI 자체가 아니다: 버튼이나 표현 스타일 개념이 아니다. 저장해야하는 데이터 모델도 아니다 – 단지 사용자가 작업하고 저장하지 않은 데이터를 담고 있다.  KO를 사용할 때, 뷰 모델은 HTML 코드를 제외한 자바스크립트 객체다. 프로그래머는 추상화된 뷰 모델을 단순하게 유지함으로써 큰 어려움없이 정교한 동작을 제어할 수 있다.

* 뷰:  사용자가 눈으로 볼수 있고 상호작용하는 유저 인터페이스로써  뷰모델 상태를 표시한다. 뷰모델의 정보를 표시하고 뷰모델의 메소드를 호출하며 (예: 사용자가 버튼을 클릭할때), 뷰모델 상태가 변경될때마다 유저 인터페이스를 갱신한다.
KO를 사용할 때, 단순히 뷰는 뷰모델과 바인딩된 HTML 문서를 말한다. 뷰모델 데이터를 템플릿과 연동하여 HTML을 생성할 수도있다.

KO를 사용할 때 뷰모델을 만들려면 자바스크립트 객체를 선언하자. 예를들어,

```js
var myViewModel = {
  personName: 'Bob',
  personAge: 123
};
```

이제, 바인딩을 선언하여 뷰모델에 대한 간단한 뷰를 만들 수 있다. 예를 들어 다음과 같은 마크업은 personName 값을 표시한다.

```html
The name is <span data-bind="text: personName"></span>
```

## Knockout 활성화

data-bind 속성(attribute)은  HTML 규약이 아니지만 문제 없다 (HTML5 규약 검사에서는 경고를 주고 HTML4  규약검사에서는 알수없는 속성이라 지적해도 문제는 없다). 그래도 브라우저는 그 속성이 무엇을 말하는지 알지 못하므로 KO를 동작시키려면 활성화 시켜야 한다.

KO를 활성화 하려면 `<script>` 블록에 다음과 같은 라인을 추가하자.

```js
ko.applyBindings(myViewModel);
```

HTML 문서 밑부분에 스크립트 블록을 넣거나,  jQuery $ 함수 와 같은 DOM의 ready 핸들러를 사용해서  본문을 감싼 후 문서의 앞부분에 둘수도 있다.

이제, 다음과 같은 HTML 코드를 작성한 것 처럼 뷰가 변환(transform)된다.

```js
The name is <span>Bob</span>
```

ko.applyBindings에 전달되는 인자를 알아보자:

* 첫번째 인자는 바인딩 선언으로 활성화시키는 뷰모델 객체를 말한다.
* data-bind 속성(attribute)에 의해 문서에서 검색할 범위를 정의하는 인자를 두번째로 지정할 수 있다.  예를 들어 ko.applyBindings(myViewModel, document.getElementById(‘someElementId’)) 코드 예제처럼,  두번째 인자를 전달하여 someElementId ID를 가지는 요소와 그 자손으로 적용 범위를 제한함으로써 여러개의 뷰모델을 각각 다른 페이지 영역과 연결시키고자 할때 유용하다.

더 이상 더 쉬울 수 없다.

## 관측대상 (Observables)

좋다, 기본 뷰모델을 만드는 방법과 바인딩을 사용하여 속성 가운데 하나를 표시하는 방법을 알았다.  KO의 가장 핵심적인 잇점은 뷰모델이 변경될 때 마다 연결된 UI를 자동으로 갱신한다는 것이다. 그렇다면, 뷰모델이 변경되었을때 어떻게 KO가 알아 내는 것인가? 답은 모델 속성(properties)을 관측대상으로 선언하기 때문이다.  모델 속성은 변경사항이 생기면 관측자에게 알려주는 특별한 자바스크립트 객체이기 때문에  의존성을 자동으로 감지한다.

앞선 뷰모델 객체를 다음과 같이 재작성해보자:

```js
var myViewModel = {
  personName: ko.observable('Bob'),
  personAge: ko.observable(123)
};
```

뷰는 아무것도 수정할 필요없다.  data-bind 부분도 이전과 같이 동작할 것이다. 차이점은, 변경 사항을 감지할 수 있으며 뷰를 자동으로 갱신한다는 것이다.


## 관측대상을 읽고 쓰기

모든 브라우저가 자바스크립트 getter/setter를 지원하지 않는다 (망할 IE ..)  이 문제 때문에 ko.observable 은 함수다.

* 관측대상의 현재 값을 읽을려면 인자없이 함수가된 관측대상을 호출하자.  예를 들어, myViewModel.personName() 함수 호출은 ‘Bob’을 리턴하고  myViewModel.personAge()  함수 호출은 123을 리턴한다.

* 관측대상을 새로운 값으로 갱신하려면 인자와 함께 호출하자. 예를 들어,  myViewModel.personName(‘Mary’) 함수 호출은 이름을 ‘Mary’로 변경한다.

* 여러개의 관측대상을 한번에 업데이트 하려면 함수호출 체인 방식을 쓴다. 예를 들어, myViewModel.personName(‘Mary’).personAge(50) 함수 호출은 이름을 ‘Mary’ 로 변경하고 나이를 50으로 변경할 것이다.

관측대상의 요점은 값이 바뀌는지 관찰 가능하다는 것이다.  즉, 값이 바뀌었을 때 알림을 받을 수 있도록 코드를 작성할 수 있다는 것이고 이점이 바로 KO의 많은 내장 binding이 내부적으로 실행된다는 것을 말한다.  data-bind=”text: personName” 라고 작성하면  personName이 변경될 때  text 바인딩이 통보받도록 스스로를 등록한다.

myViewModel.personName(‘Mary’)을 호출해서 이름을  ‘Mary’로 변경하면,  text 바인딩이 자동으로 관련된 DOM 요소의 텍스트 내용을 갱신한다.
 

## 명시적으로 관측 대상을 구독하기

직접 구독설정을 할 필요는 없기 때문에 초보자는 이 섹션을 건너뛰어도 좋다.

숙련자의 경우, 관측대상의 변경 통보를 등록하려면 관측대상의 속성인 subscribe 함수를 호출하자. 예를들어,

```js
myViewModel.personName.subscribe(function(newValue) {
  alert("The person's new name is " + newValue);
});
```

예제에서 보듯 subscribe 함수가 KO의 많은 부분이 내부적으로 동작하는 방식이다.  구독 취소를 하려면 값으로 일단 잡아서 dispose 함수를 호출하자. 예:

```html
var subscription = myViewModel.personName.subscribe(function(newValue) { /* do stuff */ });
// ...then later...
subscription.dispose(); // I no longer want notifications
```

내장된 바인딩과 템플릿 시스템이 구독과정을 관리하기 때문에 대부분의 경우 직접 이렇게 할 필요는 없다.