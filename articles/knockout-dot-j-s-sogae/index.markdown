{
    "title": "Knockout.js 소개",
    "author": "frends",
    "date": "2012-05-18T01:00:00.037Z",
    "categories": [
        "javascript"
    ],
    "tags": [],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-05-18T01:00:00.037Z",
    "status": "public",
    "important": false,
    "advanced": {}
}

## 소개

원문 : [http://knockoutjs.com/documentation/introduction.html](http://knockoutjs.com/documentation/introduction.html)

Knockout은  깔끔한 데이터 모델을 기반으로 잘 반응하는 사용자 인터페이스를 만들 수 있게 해주는 자바스크립트 라이브러리다 . 사용자의 반응이나 외부의 데이터 소스의 변화에 따라 동적으로 변하는 UI를 만들어야 한다면,  KO를 이용해서 간단하게 유지 가능하도록 구현할 수 있다.

 

### 주요 특징:

* 멋진 의존성 추적 – 번잡한 코딩없이 데이터가 변경될때마다 자동으로 적절한 UI를 갱신한다.
* 선언적 바인딩 – 데이터와 UI를 연결하는 간단하고 확실한 방법이다.
* 유연하고 완전한 템플릿 – 복잡하고 동적인 UI를 다중 중첩된 템플릿을 사용해 만들 수 있다.
* 손쉬운 확장 – 바인딩을 추가로 선언하면 원하는 커스톰 기능을 단 몇 줄의 코드만으로 쉽게 재사용할 수 있도록 구현할 수 있다.
 

### 다른 장점:

* 순수 자바스크립트 라이브러리 – 기존 웹서버 혹은 클라이언트측 기술과 문제없이 동작한다.
* 큰 변경없이 기존 웹 어플리케이션과 혼용가능
* 컴팩트 – 압축이전의 크기는 25kb 내외
* 모든 주요 브라우저에서 동작 (IE 6+, 파이어폭스 2+, 크롬, 사파리, 기타)
* 포용적인 규격 – (BDD 방식으로 개발됨) 기능이 문제없이 동작하는지 새로운 브라우저나 플랫폼에서 쉽게 검증할 수 있다.

실버라이트나 WPF를 사용해온 개발자는 KO가  MVVM 패턴 예제인 것을 눈치챌 것이다; Ruby나 Ruby on Rails 혹은 다른 MVC 기술에 더 익숙한 개발자는 KO를  선언형 MVC의 실시간 형태로 볼수도 있다. 다른 의미에서, KO는 UI가 JSON 데이터를 조작하도록 만드는 일반적인 방법 중 하나다.

 

## 좋다, 어떻게 사용하는가?

요약:  자료모델을 자바스크립트 객체로 정의하고 HTML5  ‘data-bind’ 속성으로 객체를 바인딩한다. 템플릿은 필요한 경우 사용하며 강제사항이 아니다.

 

## 예문을 보자

비행기로 여행할때, 승객용 스크린에서 식단을 선택하는 경우를 생각해보자. 승객이 메뉴를 선택하면 자세한 설명과 가격을 표시할 것이다. 우선, 선택가능한 식단을 정의한다:

```js
var availableMeals = [
    { mealName: 'Standard', description: 'Dry crusts of bread', extraCost: 0 },
    { mealName: 'Premium', description: 'Fresh bread with cheese', extraCost: 9.95 },
    { mealName: 'Deluxe', description: 'Caviar and vintage Dr Pepper', extraCost: 18.50 }
];
```
위의 항목들을 UI에 표시하기 위해 드롭-다운 리스트 (HTML `<select>` 요소)에 바인딩한다. 예를 들면 다음과 같다:

```html
<h3>Meal upgrades</h3>
<p>ake your flight more bearable by selecting a meal to match your social and economic status.</p>
Chosen meal: <select data-bind="options: availableMeals, 
                               optionsText: 'mealName'"></select>
``` 

Knockout을 기동하여 바인딩이 실제로 동작하려면 `availableMeals` 선언다음에 다음과 같은 라인을 추가한다:

```html
var viewModel = {
    /* 생략 */
};
ko.applyBindings(viewModel); // Knockout을 동작시킨다
```

뷰 모델과 MVVM에 대한 더 자세한 내용은 여기를 참조하자. 이제 페이지는 다음과 같이 표시될 것이다:

![](./@img/working-dropdown-1.png)


## 선택이 변경되었을때 반응하기

이제, 어떤 항목이 선택되었는가를 기술하기 위해 간단한 데이터 모델을 선언하자. 다음과 같이 뷰 모델에 속성(property)을 추가한다:

```js
var viewModel = {
    chosenMeal: ko.observable(availableMeals[0])
};
```

음..  `ko.observable`은 무엇인가?  KO의 기본 개념이다. UI는 이것의 값을 ‘관찰’하고 변경에 반응한다.  위의 예제에서는 선택한 메뉴를 나타내는 값을 관찰하며,  `availableMeal` 첫번째 항목을 초기값으로 설정한다. (‘Standard’ 메뉴)

UI에 있는 드롭-다운 리스트와 `chosenMeal` 변수를 연결시키자. `<select>` 요소의 `data-bind` 속성(attribute)을 수정하여  이 요소의 value 는  `chosenMeal` 모델 속성(property)을 읽고 써야한다고 알려준다 :

```html
Chosen meal: <select data-bind="options: availableMeals, 
                               optionsText: 'mealName',
                               value: chosenMeal"></select>
```

이렇게 함으로써 `chosenMeal` 속성(property)을 읽고 쓰지만 UI를 추가해야 동작하는 것을 실제로 볼수 있다. 선택된 메뉴의 가격과 설명을 표시하자:

```html
<p>
    You've chosen: 
    <b data-bind="text: chosenMeal().description"></b>
    (price: <span data-bind='text: chosenMeal().extraCost'></span>)
</p>
 ```

사용자가 드롭다운 메뉴를 선택함에 따라 UI의 일부가 자동으로 갱신된다:

![](./@img/info-about-chosen-meal.png)

 

## 관찰과 의존성 추적에 대해 조금 더 자세히

마지막으로, 가격을 통화 기호처럼 표시하면 멋질 것이다. 그저 자바스크립트 함수로 정의한다.

```js
function formatPrice(price) {
    return price == 0 ? "Free" : "$" + price.toFixed(2);
}
```

… 그런후, 바인딩을 수정하고 사용한다 …

```js
(price: <span data-bind='text: formatPrice(chosenMeal().extraCost)'></span>)
```

… 조금 나아진 UI를 볼수 있다:


가격을 표시하는 형태를 보면 임의의 자바스크립트 코드를 바인딩에 사용할 수 있음을 알수있다. 그렇게해도 KO는 여전히 바인딩과 연결된 관측대상을 감지할수 있다.  이것이 KO가 모델이 변경되었을때 갱신할 UI 부분이 무엇인지 아는 방식이다. 물론, 항상 모든 UI를 다시 표시하지는 않고 의존성이 변경된 부분만 변경한다.

관측대상을 다른 관측대상으로 중첩해서 연결할 수 있다 (예; 수량별 가격 합계).  연결 고리에서 어떤 것이 변경되면 그 위치에서 더 깊은 의존성은 재 평가되고 연결된 모든 UI가 갱신된다.  명시적으로 관측대상 간의 관계를 선언할 필요는 없다;  실시간으로 코드를 수행하면서 내부적으로 의존성을 알게된다.

관측대상 과 관측대상 배열에 대해서 더 알아볼 수있다. 위의 예제는 간단해서 KO의 효율성을 모두 보여주지는 못했다.  다른 내장 바인딩과  템플릿팅에 대해서도 알아 볼 수 있다.

 ![](./@img/formatting-price.png)

 

## KO는 jQuery (혹은 PrototypeJS 나 기타)와 경쟁관계인가? 아니면 같이 사용할수 있나?

모두가 jQuery를 좋아한다!  지난 날 꼴보기 싫고 일관성없는 DOM API 사용의 멋진 대안이기 때문이다. jQuery는 웹페이지 요소 조작과 이벤트 처리를 저수준방식으로 훌륭하게 처리한다. 저수준 방식으로 DOM 조작을 하기 위해서는 당연히 jQuery를 사용하지만, KO는 다른 종류의 문제를 해결한다.

UI가 복잡해지고 기능이 중첩됨에 따라  jQuery만 사용할 경우 코드를 간결하게 유지하는 것이 쉽지 않다.  예를 들어보자. 목록에 있는 항목을 표시하려고 할 때, 목록에 있는 항목 갯수를 표시하고  5개 이하의 항목이 있을 때만  ‘Add’ 버튼을 활성화 하고 싶다고 하자.  jQuery는 내부적으로 데이터 모델이라는 컨셉이 없다.  따라서,  태이블에서 TR 태그의 갯수를 계산하거나 특정 CSS 클래스를 가진 DIV의 갯수를 통해 내부적으로 참조할 항목 갯수를 얻는다.  어떤 SPAN 요소에 항목갯수가 표시되었다면  사용자가 항목을 추가 했을 때 SPAN 내용을 갱신하는 것을 잊지 말아야 한다. 또한, TR의 갯수가 5개 일때  ‘Add’ 버튼을 사용하지 못하게 해야한다.  나중에  ‘삭제’ 버튼도 구현해달라고 요청을 하면  클릭할 때마다 어느 DOM 요소를 변경해야하는지 알아 내야한다.

 

## KO는 어떻게 다른가? How is Knockout different?

KO를 사용하는 것은 다른 프레임워크 혹은 라이브러리를 사용하는 것보다 훨씬 쉽다.  일관성 문제를 염려할 필요도 없이 정교하게 확장할 수 있다.  그냥 항목을 자바스크립트 배열로 나타내고 그 배열을 TABLE이나 DIV로 변경하도록  템플릿을 쓰자.  배열이 변경될 때 마다  UI는 적절히 바뀐다 ( 새 TR을 어떻게/어디에 추가하는지 알 필요 없다) .  UI 나머지는 동기화상태를 유지한다. 예를 들어,  항목갯수를 표시하기 위해 SPAN에 바인딩을 다음과 같이 선언할 수 있다 (템플릿 뿐만 아니라 페이지내 어디든 넣을 수 있다):

```html
There are <span data-bind="text: myItems().count"></span> items
```

이게 전부다. 갱신하기 위한 코드를 작성할 필요도 없다. `myItems` 배열이 변경될 때마다 자동으로 갱신된다. 비슷하게, 항목 갯수에 따라 ‘Add’ 버튼을 켜고 끄려고 한다면, 그냥 다음과 같이 추가하자:

```html
<button data-bind="enable: myItems().count < 5">Add</button>
```

나중에,  ‘삭제’ 기능을 구현해야 할 때  UI 어느 부분과 상호작용 해야 할지 알 필요가 없고, 내부 데이터 모델을 수정하기만 하면 된다.

> 요약: KO는 jQuery 또는 유사한 DOM API와 경쟁하지 않는다.  KO는 데이터 모델을 UI와 연결하는 고수준의 다른 방법을 제공한다. KO는 jQuery에 의존하지 않아서 동시에 jQuery를 사용할 수 있다. 이런 점 때문에  에니메이션 효과를 줄 경우 KO를 사용하면 효과적이다.