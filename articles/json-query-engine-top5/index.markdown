{
    "title": "JSON Query Engine Top5",
    "author": "frends",
    "date": "2012-05-14T07:12:15.708Z",
    "categories": [
        "JavaScript"
    ],
    "tags": [],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-05-14T07:12:15.708Z",
    "status": "public",
    "important": false,
    "advanced": {}
}

![](./@img/json160.gif)

JavaScript에서 JSON형식으로 데이터를 유지하는 것은 현대적인 웹 애플리케이션 개발에서 자주 사용됩니다.  간단하고 사용하기 쉽고 eval() 함수로 평가만 되면 개체로 사용할 수 있습니다.  입출력을 쉽게하며 JavaScript가 아닌 언어에서도 JSON 형식의 데이터를 처리하는 경우가 늘어나고 있습니다.

JSON 데이터를 조작하기 위한 엔진의 하나로 JSON 쿼리 엔진 또는 JSON 쿼리 언어, JSON 쿼리 라이브러리라는 것이 있습니다.   JSON 데이터에서 특정 데이터를 가져와 다른  JSON 객체를 생성하는 등 브라우저단에서 데이터 구조를 형성하는데 매우 유용합니다.  예를 들어 서버에서 데이터를 JSON 형식으로 클라이언트 캐시에 저장해 놓고, 실제로 표시되는 데이터는 조건에 따라 JSON 쿼리 엔진을 사용하여 캐시에 저장된 데이터를 통해 표시하도록 하는 것들입니다.

JSON 쿼리 엔진을 적용하고 있는 쿼리 언어는 다양한 종류가 있습니다.  SQL과 유사한 문법을 채용한 것이나 LINQ를 채용한 것 그리고 자체 형식 SQL과 LINQ 또는 JSON의 조합 등으로 사용하는 등의 여러가지가 있습니다.  제공하는 기능도 다양하고 각각의 특징을 가지고 있습니다.   JSON Query Languages : 5 special purpose editors 에서는 다섯가지 JSON 쿼리 엔진의 장단점과 특징을 소개하고 있습니다.    소개하고 있는 JSON 쿼리 엔진은 다음과 같습니다.



## SQLike

introduce : [http://thomasfrank.se/sqlike.html](http://thomasfrank.se/sqlike.html)

JOIN 구문이 우수합니다.  LINQ 보다 다소 SQL 문법과 유사하게 사용하며 프로그래머에게 좀더 친숙하게 접근하고 있습니다.  반면 방법이 약간 복잡한 부분이 있고, 특정 키워드를 파이프로 묶을 필요가 있습니다.  그리고 함수의 설명이 조금 귀찮다는 단점이 있습니다.

쿼리 자체가 JSON을 사용하여 구조화 된 것이 특징입니다.  추가적으로 ActionScript 도 지원합니다.


```js
//선택 구문은 where 조건 비교함수의 this.born이 1941 보다 작은 조건에 만족하는 튜플을
 //‘instrument’ 필드를 기준으로 내림차순으로 정렬하여 반환한다.
 SQLike.q({
 Select: ['*'],
 From: dataJson,
 Where: function(){return this.born < 1941},
 OrderBy: ['instrument','|desc|']
 });
 ```


## JSLINQ

introduce : [http://jslinq.codeplex.com/](http://jslinq.codeplex.com/)

LINQ의 표기법에 가장 유사하게 구현되었습니다.  반면 JOIN 구문은 SQLike 만큼 완벽하지는 않습니다.  LINQ(Language Integrated Query : 통합 쿼리 언어)는 다양한 데이터 소스에 대한 범용적으로 사용할 수 있도록 설계된 쿼리 언어로 SQL 데이터베이스 쿼리 언어에 비해 LINQ는 데이터 원본의 종류를 가리지 않고 사용할 수 있도록 되어있다는 차이가 있습니다.

```js
//firstName 필드가 “Ringo”와 같은 튜플을 반환한다.
 var exampleArray = JSLINQ(dataJson)
 .Where(function(item){ return item.firstName == "Ringo"; });
```



## JSINQ

introduce : [http://jsinq.codeplex.com/](http://jsinq.codeplex.com/), project : [http://hugoware.net/projects/jlinq](http://hugoware.net/projects/jlinq)

쿼리를 JavaScript 코드가 아닌 순수하게 문자열로 구성된 쿼리로 작성할 수 있는 특징이 있습니다.  그래서 쿼리를 만들거나 실행하는 것을 다른 방식으로도 할 수 있습니다.  그러나 이것은 이점이 있는 반면 단점도 있습니다.


```js
//문자열 형태의 쿼리를 수행할 수 있다.
 var query = new jsinq.Query('\
 from customer in $0 \
 group customer by customer.lastname into g \
 select {lastname: g.key, count: g.count()} \
 into r \
 orderby r.count descending \
 select r \
 ');
 

query.setValue(0, customers);
 var result = query.execute();
```



## JLinq

introduce : [http://jlinq.codeplex.com/](http://jlinq.codeplex.com/)

철저히 조건 함수로 작성하도록 되어 있고 현실적인 함수를 제공하며 사용하는데 별 어려움이 없습니다.  JOIN 기능은 제공되지 않으며 대신 union()으로 결합하고 그 후에 필요한 것들을 얻기 위한 기술해야 합니다.


```js
var results = jLinq.from(records)
 .ignoreCase()
 .startsWith("name", "m")
 .or("j")
 .is("admin")
 .orderBy("age")
 .select();
 }}
 ```

## JimmyLinq

introduce : [http://netindonesia.net/blogs/jimmy/archive/2007/07/16/Javascript-LINQ_3F003F003F00_.aspx](http://netindonesia.net/blogs/jimmy/archive/2007/07/16/Javascript-LINQ_3F003F003F00_.aspx)

아주 간단한 JSON 쿼리 엔진으로 다른 JSON 쿼리 엔진과 비교할 때 제공하는 기능이 매우 적습니다.  직접 JSON 쿼리 엔진을 구현해야 하는 경우에 기초적으로 제공되어지기 위한 것에 가깝습니다.


```js
var p = from (peoples).
 where (function(n) { return n.LastName === "Doe" || n.LastName === "White"; }).
 orderby([ "Age", "LastName", "FirstName" ]).
 select(function(n) { return { FullName : n.FirstName + " " + n.LastName, Age : n.Age }; });
```



## Summary

위의 엔진을 사용해보면서 느낀점은 하나의 웹 서비스를 개발함에 있어서 많은 Back-end 개발자들과 Front-end 개발자들이 협업할 때 백앤드 개발자들과는 일관된 데이터 포맷을 정의하고 제공받기 위해 유용하고 프론트 앤드 개발자들간에는 일관된 데이터 모델을 제공함으로써 복잡한 코드에서 일관된 데이터 모델에 접근할 수 있도록 제공하여 향후 다양한 이점이 발생할 것이라는 느낌을 받았습니다.

뿐만 아니라 SQL을 기반으로 하고 있기 때문에 간단한 쿼리문만 이해하고 있다면 초기 학습시간도 그리 오래 걸리지 않을 것입니다.



## See More

위에서 소개한 것 외에 다음과 같은 JSON 쿼리 라이브러리들이 더 존재합니다.

TrimQuery

[http://code.google.com/p/trimpath/wiki/TrimQuery](http://code.google.com/p/trimpath/wiki/TrimQuery)

JSONQuery: Data Querying Beyond JSONPath

[http://www.sitepen.com/blog/2008/07/16/jsonquery-data-querying-beyond-jsonpath/](http://www.sitepen.com/blog/2008/07/16/jsonquery-data-querying-beyond-jsonpath/)

jaql - Query Language for JavaScript(r) Object Notation (JSON)

[http://code.google.com/p/jaql/wiki/JaqlOverview](http://code.google.com/p/jaql/wiki/JaqlOverview)
