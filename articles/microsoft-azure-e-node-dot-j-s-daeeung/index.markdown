{
    "title": "Microsoft, Azure 에 Node.js 대응",
    "author": "Rhio Kim",
    "date": "2012-05-18T01:25:39.853Z",
    "categories": [
        "javascript"
    ],
    "tags": [],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-05-18T01:25:39.853Z",
    "status": "public",
    "important": false,
    "advanced": {}
}

![](./@img/cl-node-300x195.png)

최근 [Node.js][1] 를 윈도우에 포팅하기 위한 작업을 진행중입니다. Ryan Dahl 은 Microsoft는 [Node.js][1]를 Window Azure 및 Window Server 에 이식하기 위한 작업을 시작했다고 발표했다.

[Node.js][1] 를 Window Azure 와 Window Server 에 네이티브 응용 프로그램화를 목표로 비동기 I/O API인 [IOCP(Input Output Completion Port)](http://en.wikipedia.org/wiki/Input/output_completion_port) 대응이 진행중이다. 이에 대한 결과물은 Window용 node.exe 이다.

Ryan Dahl 에 의하면 이 대응을 위해서 Node.js 코어 아키텍쳐에 큰 변화를 수반하게 될 것으로 이에 필요한 기술 지원은 마이크로소프트에서 [기술 지원 및 개발 리소스를 제공](http://blog.nodejs.org/2011/06/23/porting-node-to-windows-with-microsoft%E2%80%99s-help/)한다고 한다.

마이크로소프트는 사실 Node.js 뿐만 아니라 유명한 오픈 소스 지원에 굉장히 적극적으로 대응하고 있다.
jQuery 지원은 이미 지원하고 있고 Windows Azure 는 PHP, Ruby on Rails, Mysql 등도 대응하였다.

무료 개발도구들의 대응을 바라보더라도 앞으로의 클라우드 시스템에서 Node.js 도 클라우드 애플리케이션 실행 환경으로 유력시 되고 있다는 의미이기도 하다.

물론 이 뒤에는 마이크로소프트의 Azure와 Server 제품들이 클라우드 시장에서 자리잡기 위한 의도가 있겠지만 자바스크립트 개발자들에게는 반가운 소식일 수 밖에 없다.

 

## References

* [http://www.publickey1.jp/blog/11/nodejswindows_azure.html](http://www.publickey1.jp/blog/11/nodejswindows_azure.html)
* [http://www.h-online.com/open/news/item/Node-js-to-go-native-on-Windows-with-Microsoft-s-help-1267168.html](http://www.h-online.com/open/news/item/Node-js-to-go-native-on-Windows-with-Microsoft-s-help-1267168.html)

[1]: http://nodejs.org