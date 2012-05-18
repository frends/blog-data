{
    "title": "Cappuccino 프레임워크 시작하기1",
    "author": "frends",
    "date": "2012-05-17T09:35:42.843Z",
    "categories": [
        "javascript"
    ],
    "tags": [],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-05-17T09:35:42.843Z",
    "status": "public",
    "important": false,
    "advanced": {}
}


카푸치노는 어떠한 OS 나 Browser 상에서라도 개발할 수 있습니다.
여러분에게 가장 편리한 에디터를 사용해서 개발하실 수 있습니다.
그러나, Max OSX 혹은 Ubuntu 와 같은 리눅스계열 OS에 설치하면, 좀 더 다양한 툴을 제공해 줍니다.
Cappuccino 프레임워크를 시작하기 위한 준비로, 설치방법 및 디버깅툴, 개발툴 등을 알아보겠습니다.

![](./@img/download1.jpeg)
 

## Mac OSX 에 설치하기

Cappuccino 는 Mac OSX 와 최적의 궁합을 보여줍니다.
초보자를 위한 Starter Package 를 직접제공하고 있어 단 한줄의 커멘드 만으로도 설치가 가능합니다.

[Starter Package 다운로드하기](http://cappuccino.org/starter)

터미널에서 Starter Package를 압축해제한 디렉토리로 이동후 아래 커멘드를 입력하고 묻는 질문에 적절히 대답해주면 설치가 완료됩니다.

```
~ $ ./bootstrap.sh
```
 

## Linux 및 사용자 설치하기

Mac이 없다면 리눅스 운영체제를 선택할 수도 있습니다. (Windows 에서 가상 Unix 환경을 만드는 방법은 편의상 생략했습니다.)
여기서는 Ubuntu 10.4 를 예로들어 설치합니다.

* Github 소스저장소에서 최신버전의 Cappuccino 소스를 체크아웃 받습니다.

```
~ $ git clone git://github.com/280north/cappuccino.git
```

* 체크아웃받은 디렉토리에서 bootstrap.sh 를 실행합니다.
* 때때로 현재 OS에 설치된 JVM 과 narwhal(Command line Javascript Interpreter) 이 충돌이 난다는 메세지를 보여줄 경우가 있습니다.

```
~/cappuccino $ ./bootstrap.sh
Error: Narwhal is not compatible with your JVM. Please switch to the Sun (HotSpot) 
```

이럴경우 다음과 같이 조치합니다. 

```
// 저장소 추가
$ sudo add-apt-repository "deb http://kr.archive.ubuntu.com/ubuntu/ jaunty multiverse"
$ sudo add-apt-repository "deb http://kr.archive.ubuntu.com/ubuntu/ jaunty-updates multiverse"

// 저장소 목록 업데이트후 자바 설치
$ sudo apt-get update
$ sudo apt-get install sun-java5-jdk
```

HotSpot JVM 설치참조 - [http://adsgear.tistory.com/76](http://adsgear.tistory.com/76)
다시 bootstrap.sh 를 실행하면 narwhal 및 필요한 패키지들을 설치합니다. 대부분 기본값으로 설치하면 문제가 없습니다.

* 최종 설치 완료메세지

```
Added to "/home/frends/.profile". Restart your shell or run "source /home/frends/.profile".
=========================================================================
Before building Cappuccino we recommend you set the $CAPP_BUILD environment variable to a path where you wish to build Cappuccino.
NOTE: If you have previously set $CAPP_BUILD and built Cappuccino you may want to delete the directory before rebuilding.
=========================================================================
Bootstrapping of Narwhal and other required tools is complete.
NOTE: any changes made to the shell configuration files won't take place until you restart the shell.
```

* 설치가 완료되면 .profile 에 CAPP_BUILD 환경변수를 설정합니다. 

```
export CAPP_BUILD="/home/frends/enviroment/cappuccino"
source /home/frends/.profile
```

bootstrap.sh 를 실행한 폴더에서 jake를 실행하면 필요한 파일들이 CAPP_BUILD 에 지정한 디렉토리에 빌드되고 설치가 완료됩니다.
 

## 바로 개발시작하기

Cappuccino 개발에 필요한 파일들을 구할수만 있다면 이런 설치과정이 없어도 개발을 시작할 수 있습니다.
Starter 패키지의 NewApplication 폴더가 바로 Cappuccino 어플리케이션의 기본틀입니다. 다운로드
Objective-J 구동에 필요한 기본 프레임워크와 AppKit 등의 필수 라이브러리들을 포함하고 있습니다.

![](./@img/appkit.png)

웹 브라우저에서 접근하는 파일은 index.html 혹은 index-debug.html 이고 main.j 와 AppController.j 가 Objective-J 로 코딩된 소스코드 파일입니다.
기본적인 코드가 들어있는데 그 유명한 "Hello World" 프로그램이 작성되어 있습니다.

```
@import /CPObject.j>

@implementation AppController : CPObject
{
}
- (void)applicationDidFinishLaunching:(CPNotification)aNotification
{
    var theWindow = [[CPWindow alloc] initWithContentRect:CGRectMakeZero() styleMask:CPBorderlessBridgeWindowMask],
        contentView = [theWindow contentView];

    var label = [[CPTextField alloc] initWithFrame:CGRectMakeZero()];

    [label setStringValue:@"Hello World!"];
    [label setFont:[CPFont boldSystemFontOfSize:24.0]];

    [label sizeToFit];

    [label setAutoresizingMask:CPViewMinXMargin | CPViewMaxXMargin | CPViewMinYMargin | CPViewMaxYMargin];
    [label setCenter:[contentView center]];

    [contentView addSubview:label];

    [theWindow orderFront:self];
}
@end</foundation>
```
 

## 웹 브라우저로 확인하기

"Hello World" 처럼 간단한 어플리케이션을 웹브라우저로 확인해봅시다.
여러분의 프로젝트 디렉토리를 여러분이 가진 웹서버로 접근해보면 아래와 같은 심플한 화면을 보실 수 있습니다.

![](./@img/loading2.png)
![](./@img/hello2.png)

웹서버를 사용해서 띠워야 하는 이유는 Cappuccino 는 내부적으로 Class import 및 여러가지 작업에서 XMLHttpRequest 를 사용하기 때문입니다.

Chrome 과 같이 로컬파일 XHR 통신이 제한되어 있는 브라우저의 경우에는 반드시 웹서버를 통해야 가능합니다.
로컬파일 XHR 에 제한이 없는 브라우저라면(IE와 같은) 그냥 로컬파일을 실행해서 봐도 무방합니다.
만약 카푸치노툴이 설치되어 있다면 Narwhal Cli 환경을 이용한 jackup 서버를 사용하여 현재 디렉토리를 홈으로 서버를 만들 수 있습니다.

```
jackup -e "require('jack/file').File('.')" -E deployment
```

자. Cappuccino Framework 설치가 완료되었습니다.
다음 포스트에서는 Cappuccino 시작하기의 두번째로 개발시 알아두면 좋을 브라우저 및 디버깅도구 를 소개하도록 하겠습니다.
