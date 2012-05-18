{
    "title": "JavaScript/CSS 압축 최적화 80초에서 6초로 단축 기법 공개 – Mozilla",
    "author": "Rhio Kim",
    "date": "2012-05-18T02:21:53.171Z",
    "categories": [
        "tech"
    ],
    "tags": [],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-05-18T02:21:53.171Z",
    "status": "public",
    "important": false,
    "advanced": {}
}

<img src="./@img/automatically-compressing-javascript-files-on-apache.jpeg" hspace="5" class="pull-left">
Mozilla WebDev 블로그에 “From 80 Seconds to 60 : Optimizing Our Asset Compression” 으로 JavaScript, CSS, 이미지 최소화 처리 시간을 80초~160초가 걸리던 것을 6초로 단축시키는 기술이 공개되었습니다.

실용적인 자료로 참고할 필요가 있습니다.

현재 “Add-ons for Filefox”는 CDN 을 활용하여 리소스를 제공하고 있지만 이렇게 되기 전에 캐시 효과를 극대화하기 위해 JavaScript, CSS, 이미지 요소를 미리 최소화 처리 후 사용했다고 합니다.  이 리소스의 생성하는데 80초에서 160초 걸리던 이 작업을 고려하여 최종적으로 6초로 단축하게 되었다고 설명하고 있습니다.
여기에서 소개한 속도 축약 기술을 정리하면 다음과 같습니다.

1. 지금까지는 YUICompressor 을 채용하고 있었으나 YUICompressor 는 처리 속도가 느립니다.
이 부분을 자브스크립트에 대해서는 UgilfyJS 로 CSS 에 대해서는 Clean-css 를 사용하도록 변경했는데 이 변경으로 43초를 단축.

2. 파일 해시값을 사전에 보관하도록 변경으로 2초 정도 단축

3. 이미지의 경우 해시값에 대해서는 Git 커밋 대신 파일 해시를 사용해서 12초를 단축

4. 모든 파일을 최소화하는 것이 아니라 변경된 파일만 최소화하도록 해서 17초를 단축
5. 
CDN을 사용할 수 있는 경우에는 이런 기술의 필요성이 다소 낮아지지만 그렇지 않는 경우 제작 시 빌드 속도 기법을 추천합니다.  특히 YUICompressor 아 니라 UglifyJS 및 Clean-css 를 사용하여 빌드 시간을 단축하는 방법은 효과적인 방법으로 검토할 가치가 있습니다.