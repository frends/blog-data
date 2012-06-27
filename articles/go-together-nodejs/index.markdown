{
    "title": "Go together nodejs",
    "author": "nanhapark",
    "date": "2012-06-27T14:20:51.756Z",
    "categories": [
        "node.js"
    ],
    "tags": ["node.js"],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-06-27T14:20:51.756Z",
    "status": "public",
    "important": false,
    "advanced": {}
}

# Go together node.js

[FRENDS](http://blog.frends.kr) 6차 모임때 발표한 슬라이드입니다.


<div style="width:425px" id="__ss_13427102"> <strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/nanhapark/frends-go-together-with-nodejs" title="[Frends] go together with nodejs" target="_blank">[Frends] go together with nodejs</a></strong> <iframe src="http://www.slideshare.net/slideshow/embed_code/13427102" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0" allowfullscreen></iframe> <div style="padding:5px 0 12px"> View more <a href="http://www.slideshare.net/" target="_blank">presentations</a> from <a href="http://www.slideshare.net/nanhapark" target="_blank">Nanha Park</a> </div> </div>

# JS2C Live coding

## 전체순서

    // console API 를 js2c로 컴파일
    python module.compile.py
     
    // header 확인
    cat output.h |less
      
    // ascii 모음의 진위 판단하기 위한 decompile 확인
    vi module_decompile.c 
      
    // gcc / 실행
    gcc -o module_exec module_decompile.c 
    ./module_exec |more
      
    // 비교
    vi lib/console.js 

## output.h

`js2c`는 아래와 같은 `Header`파일을 만들어 냅니다. 예제에서는 `console` 만 대상으로 했지만, 모든 `lib`디렉토리의 모듈이 포함되어집니다.


    #ifndef node_natives_h
    #define node_natives_h
     
    namespace node {
     
      const char console_native[] = { 47, 47, 32, 67, 111, 112, 121, 114, 105, 103, 104, 116, 32, 74, 111, 121, 101, 110, 116, 44, 32, 73, 110, 99, 46, 32, 97, 110, 100, 32, 111, 116, 104, 101, 114, 32, 78, 111, 100, 101, 32, 99, 111, 110, 116, 114, 105, 98, 117, 116, 111, 114, 115, 46, 10, 47, 47, 10, 47, 47, 32, 80, 101, 114, 109, 105, 115, 115, 105, 111, 110, 32, 105, 115, 32, 104, 101, 114, 101, 98, 121, 32, 103, 114, 97, 110, 116, 101, 100, 44, 32, 102, 114, 101, 101, 32, 111, 102, 32, 99, 104, 97, 114, 103, 101, 44, 32, 116, 111, 32, 97, 110, 121, 32, 112, 101, 114, 115, 111, 110, 32, 111, 98, 116, 97, 105, 110, 105, 110, 103, 32, 97, 10, 47, 47, 32, 99, 111, 112, 121, 32, 111, 102, 32, 116, 104, 105, 115, 32, 115, 111, 102, 116, 119, 97, 114, 101, 32, 97, 110, 100, 32, 97, 115, 115, 111, 99, 105, 97, 116, 101, 100, 32, 100, 111, 99, 117, 109, 101, 110, 116, 97, 116, 105, 111, 110, 32, 102, 105, 108, 101, 115, 32, 40, 116, 104, 101, 10, 47, 47, 32, 34, 83, 111, 102, 116, 119, 97, 114, 101, 34, 41, 44, 32, 116, 111, 32, 100, 101, 97, 108, 32, 105, 110, 32, 116, 104, 101, 32, 83, 111, 102, 116, 119, 97, 114, 101, 32, 119, 105, 116, 104, 111, 117, 116, 32, 114, 101, 115, 116, 114, 105, 99, 116, 105, 111, 110, 44, 32, 105, 110, 99, 108, 117, 100, 105, 110, 103, 10, 47, 47, 32, 119, 105, 116, 104, 111, 117, 116, 32, 108, 105, 109, 105, 116, 97, 116, 105, 111, 110, 32, 116, 104, 101, 32, 114, 105, 103, 104, 116, 115, 32, 116, 111, 32, 117, 115, 101, 44, 32, 99, 111, 112, 121, 44, 32, 109, 111, 100, 105, 102, 121, 44, 32, 109, 101, 114, 103, 101, 44, 32, 112, 117, 98, 108, 105, 115, 104, 44, 10, 47, 47, 32, 100, 105, 115, 116, 114, 105, 98, 117, 116, 101, 44, 32, 115, 117, 98, 108, 105, 99, 101, 110, 115, 101, 44, 32, 97, 110, 100, 47, 111, 114, 32, 115, 101, 108, 108, 32, 99, 111, 112, 105, 101, 115, 32, 111, 102, 32, 116, 104, 101, 32, 83, 111, 102, 116, 119, 97, 114, 101, 44, 32, 97, 110, 100, 32, 116, 111, 32, 112, 101, 114, 109, 105, 116, 10, 47, 47, 32, 112, 101, 114, 115, 111, 110, 115, 32, 116, 111, 32, 119, 104, 111, 109, 32, 116, 104, 101, 32, 83, 111, 102, 116, 119, 97, 114, 101, 32, 105, 115, 32, 102, 117, 114, 110, 105, 115, 104, 101, 100, 32, 116, 111, 32, 100, 111, 32, 115, 111, 44, 32, 115, 117, 98, 106, 101, 99, 116, 32, 116, 111, 32, 116, 104, 101, 10, 47, 47, 32, 102, 111, 108, 108, 111, 119, 105, 110, 103, 32, 99, 111, 110, 100, 105, 116, 105, 111, 110, 115, 58, 10, 47, 47, 10, 47, 47, 32, 84, 104, 101, 32, 97, 98, 111, 118, 101, 32, 99, 111, 112, 121, 114, 105, 103, 104, 116, 32, 110, 111, 116, 105, 99, 101, 32, 97, 110, 100, 32, 116, 104, 105, 115, 32, 112, 101, 114, 109, 105, 115, 115, 105, 111, 110, 32, 110, 111, 116, 105, 99, 101, 32, 115, 104, 97, 108, 108, 32, 98, 101, 32, 105, 110, 99, 108, 117, 100, 101, 100, 10, 47, 47, 32, 105, 110, 32, 97, 108, 108, 32, 99, 111, 112, 105, 101, 115, 32, 111, 114, 32, 115, 117, 98, 115, 116, 97, 110, 116, 105, 97, 108, 32, 112, 111, 114, 116, 105, 111, 110, .... 중략 ....};
     
      struct _native {
        const char* name;
        const char* source;
        size_t source_len;
      };
     
     
      static const struct _native natives[] = {
     
        { "console", console_native, sizeof(console_native)-1 },
     
        { NULL, NULL } /* sentinel */
     
      };
     
    }#endif

## module_compile.py

    #!/bin/env python
    # -*- coding: utf-8 -*-
    # =============================================
    # js2c 사용한 console.js 컴파일 과정 예제
    #
    # @author nanhapark
    # =============================================
    def main():
      # 최상위 디렉토리에서 tools를 라이브러리 참조디렉토리로 포함
      import sys
      sys.path.append('./tools')
      
      # js2c 불러옴
      import js2c
     
      # 소스트리 최상위에서 native module이 존재하는 lib 디렉토리안의
      # console.js 지정
      source = ['./lib/console.js']
      targets = ['output.h']
      js2c.JS2C(source, targets)
     
      print 'Complete ..'
      print 'Confirm output.h'
     
    if __name__ == '__main__':
      main()


## module_decompile.c

아래 코드를 컴파일 하시면 `lib/console.js` 내용을 확인할 수 있습니다.

    /****************************************************
     * js2c 에 의해 컴파일된 C Style의 Static 값을
     * decompile 하여 원래 소스내용을 확인하는 예제소스
     *
     * @author nanhapark
     ****************************************************/

    #include <stdio.h>
    int main(void) {
      // native module console 객체가 js2c로 인해 변화된 값
      const char console_native[] = { 47, 47, 32, 67, 111, 112, 121, 114, 105, 103, 104, 116, 32, 74, 111, 121, 101, 110, 116, 44, 32, 73, 110, 99, 46, 32, 97, 110, 100, 32, 111, 116, 104, 101, 114, 32, 78, 111, 100, 101, 32, 99, 111, 110, 116, 114, 105, 98, 117, 116, 111, 114, 115, 46, 10, 47, 47, 10, 47, 47, 32, 80, 101, 114, 109, 105, 115, 115, 105, 111, 110, 32, 105, 115, 32, 104, 101, 114, 101, 98, 121, 32, 103, 114, 97, 110, 116, 101, 100, 44, 32, 102, 114, 101, 101, 32, 111, 102, 32, 99, 104, 97, 114, 103, 101, 44, 32, 116, 111, 32, 97, 110, 121, 32, 112, 101, 114, 115, 111, 110, 32, 111, 98, 116, 97, 105, 110, 105, 110, 103, 32, 97, 10, 47, 47, 32, 99, 111, 112, 121, 32, 111, 102, 32, 116, 104, 105, 115, 32, 115, 111, 102, 116, 119, 97, 114, 101, 32, 97, 110, 100, 32, 97, 115, 115, 111, 99, 105, 97, 116, 101, 100, 32, 100, 111, 99, 117, 109, 101, 110, 116, 97, 116, 105, 111, 110, 32, 102, 105, 108, 101, 115, 32, 40, 116, 104, 101, 10, 47, 47, 32, 34, 83, 111, 102, 116, 119, 97, 114, 101, 34, 41, 44, 32, 116, 111, 32, 100, 101, 97, 108, 32, 105, 110, 32, 116, 104, 101, 32, 83, 111, 102, 116, 119, 97, 114, 101, 32, 119, 105, 116, 104, 111, 117, 116, 32, 114, 101, 115, 116, 114, 105, 99, 116, 105, 111, 110, 44, 32, 105, 110, 99, 108, 117, 100, 105, 110, 103, 10, 47, 47, 32, 119, 105, 116, 104, 111, 117, 116, 32, 108, 105, 109, 105, 116, 97, 116, 105, 111, 110, 32, 116, 104, 101, 32, 114, 105, 103, 104, 116, 115, 32, 116, 111, 32, 117, 115, 101, 44, 32, 99, 111, 112, 121, 44, 32, 109, 111, 100, 105, 102, 121, 44, 32, 109, 101, 114, 103, 101, 44, 32, 112, 117, 98, 108, 105, 115, 104, 44, 10, 47, 47, 32, 100, 105, 115, 116, 114, 105, 98, 117, 116, 101, 44, 32, 115, 117, 98, 108, 105, 99, 101, 110, 115, 101, 44, 32, 97, 110, 100, 47, 111, 114, 32, 115, 101, 108, 108, 32, 99, 111, 112, 105, 101, 115, 32, 111, 102, 32, 116, 104, 101, 32, 83, 111, 102, 116, 119, 97, 114, 101, 44, 32, 97, 110, 100, 32, 116, 111, 32, 112, 101, 114, 109, 105, 116, 10, 47, 47, 32, 112, 101, 114, 115, 111, 110, 115, 32, 116, 111, 32, 119, 104, 111, 109, 32, 116, 104, 101, 32, 83, 111, 102, 116, 119, 97, 114, 101, 32, 105, 115, 32, 102, 117, 114, 110, 105, 115, 104, 101, 100, 32, 116, 111, 32, 100, 111, 32, 115, 111, 44, 32, 115, 117, 98, 106, 101, 99, 116, 32, 116, 111, 32, 116, 104, 101, 10, 47, 47, 32, 102, 111, 108, 108, 111, 119, 105, 110, 103, 32, 99, 111, 110, 100, 105, 116, 105, 111, 110, 115, 58, 10, 47, 47, 10, 47, 47, 32, 84, 104, 101, 32, 97, 98, 111, 118, 101, 32, 99, 111, 112, 121, 114, 105, 103, 104, 116, 32, 110, 111, 116, 105, 99, 101, 32, 97, 110, 100, 32, 116, 104, 105, 115, 32, 112, 101, 114, 109, 105, 115, 115, 105, 111, 110, 32, 110, 111, 116, 105, 99, 101, 32, 115, 104, 97, 108, 108, 32, 98, 101, 32, 105, 110, 99, 108, 117, 100, 101, 100, 10, 47, 47, 32, 105, 110, 32, 97, 108, 108, 32, 99, 111, 112, 105, 101, 115, 32, 111, 114, 32, 115, 117, 98, 115, 116, 97, 110, 116, 105, 97, 108, 32, 112, 111, 114, 116, 105, 111, 110, 115, 32, 111, 102, 32, 116, 104, ..... 중략..... };
     
      const char *pp;
      pp = console_native;
      while (*pp) {
        printf("%c", *pp);
        pp++;
      }
      
      return 0;
    }


# nodeman Live coding

## 목차

    # 설치
    npm install nodeman -g

    # 포함된 메뉴얼 키워드 확인
    nodeman -b

    # cluster 모듈 확인
    nodeman cluster|more

## vi wscript +1013

`node.js black edition`의 `Helper`로서 `wscript`에 빌드시 자동으로 설치될 수 있도록 삽입함.

    1011   # Node.js Black Edition Above
    1012   # @author nanhapark <http://about.me/nanha>
    1013   NodeBlackEdition(bld)
    1014 
    1015 def install_npm(bld):
    1016   start_dir = bld.path.find_dir('deps/npm')
    1017   # The chmod=-1 is a Node hack. We changed WAF so that when chmod was set to
    1018   # -1 that the same permission in this tree are used. Necessary to get
    1019   # npm-cli.js to be executable without having to list every file in npm.
    1020   bld.install_files('${LIBDIR}/node_modules/npm',
    1021                     start_dir.ant_glob('**/*'),
    1022                     cwd=start_dir,
    1023                     relative_trick=True,
    1024                     chmod=-1)
    1025   bld.symlink_as('${PREFIX}/bin/npm',
    1026                  '../lib/node_modules/npm/bin/npm-cli.js')
    1027 
    1028 # Node.js Black Edition
    1029 # @author nanhapark <http://about.me/nanha>
    1030 def NodeBlackEdition(bld):
    1031   start_dir = bld.path.find_dir('deps/nodeman')
    1032   bld.install_files('${LIBDIR}/node_modules/nodeman',
    1033                     start_dir.ant_glob('**/*'),
    1034                     cwd=start_dir,
    1035                     relative_trick=True,
    1036                     chmod=0755)
    1037   bld.symlink_as('${PREFIX}/bin/nodeman',
    1038                  '../lib/node_modules/nodeman/bin/nodeman')


# Make과정 이해
node.js의 make과정은 `lib, src` 디렉토리의 모듈이 변화하지 않으면 `make`할시에 `rebuild`하지 않습니다.

    [root@nodejs:nodejs]# make
    Waf: Entering directory `/home/nanhadev/node-black-edition/nodejs/out'
    DEST_OS: linux
    DEST_CPU: x64
    Parallel Jobs: 1
    Product type: program
    Waf: Leaving directory `/home/nanhadev/node-black-edition/nodejs/out'
    'build' finished successfully (0.260s)
    -rwxr-xr-x 1 root root 12M  6월 23 15:46 out/Release/node

# javascript module 추가방법


## lib 디렉토리에 nanhapark.js 만들기

    function nanhapark() {
      this.name = 'nanhapark';
      this.hp = '010-9864-1512';
    }

    module.exports = nanhapark;

## make build 과정에 추가됨

- lib 디렉토리에 새로운 라이브러리가 추가되어 `src/node_natives.h` 만드는 과정이 `rebuild` 되어짐
- 이로인해 `src/node_natives.h` 불러들이는 `src/node_javascript.cc` 파일이 `rebuild` 되어짐
- 마지막 `node.js` 실행파일인 `out/Release/node` 생성됨. 이것은 소스트리 최상단에 `symbolic link` 되어 있어서 `make install`하지 않고, `./node` 으로 현 빌드상황을 확인할 수 있음.

## build log

    [ 3/37] src/node_natives.h: src/node.js lib/optimist.js lib/wordwrap_doc.js lib/querystring.js lib/net.js lib/restler_multipartform.js lib/vm.js lib/module.js lib/_debugger.js lib/winston_exception.js lib/moment.js lib/mkdirp.js lib/restler_doc.js lib/os.js lib/step_doc.js lib/minimatch.js lib/crypto.js lib/node_static_util.js lib/freelist.js lib/readline.js lib/def_doc.js lib/tty.js lib/cluster.js lib/nanhapark.js lib/graceful_fs.js lib/black_edition_info.js lib/winston_file.js lib/glob.js lib/buffer.js lib/base64.js lib/restler.js lib/node_static_doc.js lib/http.js lib/winston_transports_couchdb.js lib/mustache_doc.js lib/assert.js lib/underscore.js lib/optimist_doc.js lib/winston_container.js lib/winston_transports_loggly.js lib/url.js lib/winston_config_cli_config.js lib/winston_common.js lib/async.js lib/dns.js lib/clog.js lib/winston_config_syslog_config.js lib/_linklist.js lib/util_class.js lib/colors.js lib/buffer_ieee754.js lib/child_process.js lib/repl.js lib/clog_doc.js lib/zlib.js lib/winston_logger.js lib/dgram.js lib/winston_config.js lib/hashish.js lib/fileutils_doc.js lib/traverse.js lib/glob_doc.js lib/moment_doc.js lib/mysql.js lib/events.js lib/inherits.js lib/colors_doc.js lib/winston.js lib/winston_doc.js lib/util_prototype.js lib/step.js lib/hashish_doc.js lib/winston_transports_webhook.js lib/Class_doc.js lib/node_static_mime.js lib/mustache.js lib/commander_doc.js lib/winston_transports_transport.js lib/winston_transports_file.js lib/commander.js lib/path.js lib/sys.js lib/constants.js lib/minimatch_doc.js lib/console.js lib/fileutils.js lib/punycode.js lib/mkdirp_doc.js lib/extend.js lib/winston_transports.js lib/util.js lib/fs.js lib/node_static.js lib/inherits_doc.js lib/underscore_doc.js lib/lru_cache.js lib/winston_config_npm_config.js lib/tls.js lib/stack_trace.js lib/https.js lib/stream.js lib/graceful_fs_doc.js lib/winston_transports_console.js lib/string_decoder.js lib/wordwrap.js lib/timers.js -> out/Release/src/node_natives.h
    [ 9/37] cxx: src/node_javascript.cc -> out/Release/src/node_javascript_5.o
    /usr/bin/g++ -pthread -g -O3 -DHAVE_OPENSSL=1 -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64 -DHAVE_FDATASYNC=1 -DARCH="x64" -DPLATFORM="linux" -D__POSIX__=1 -Wno-unused-parameter -D_FORTIFY_SOURCE=2 -IRelease/src -I../src -IRelease/deps/http_parser -I../deps/http_parser -IRelease/deps/uv/include -I../deps/uv/include -IRelease/deps/uv/src/ares -I../deps/uv/src/ares -IRelease/deps/v8/include -I../deps/v8/include -I/usr/kerberos/include -Ideps/v8/include ../src/node_javascript.cc -c -o Release/src/node_javascript_5.o
    [37/37] cxx_link: out/Release/src/node_main_5.o out/Release/src/node_5.o out/Release/src/node_buffer_5.o out/Release/src/node_javascript_5.o out/Release/src/node_extensions_5.o out/Release/src/node_http_parser_5.o out/Release/src/node_constants_5.o out/Release/src/node_file_5.o out/Release/src/node_script_5.o out/Release/src/node_os_5.o out/Release/src/node_dtrace_5.o out/Release/src/node_string_5.o out/Release/src/node_zlib_5.o out/Release/src/timer_wrap_5.o out/Release/src/handle_wrap_5.o out/Release/src/stream_wrap_5.o out/Release/src/tcp_wrap_5.o out/Release/src/udp_wrap_5.o out/Release/src/pipe_wrap_5.o out/Release/src/cares_wrap_5.o out/Release/src/tty_wrap_5.o out/Release/src/fs_event_wrap_5.o out/Release/src/process_wrap_5.o out/Release/src/v8_typed_array_5.o out/Release/src/node_extend_5.o out/Release/src/node_base64_5.o out/Release/src/node_signal_watcher_5.o out/Release/src/node_stat_watcher_5.o out/Release/src/node_io_watcher_5.o out/Release/src/platform_linux_5.o out/Release/src/node_crypto_5.o out/Release/deps/http_parser/http_parser_3.o -> out/Release/node

## require 확인하기

    # make install 하지 않아도 최상위 경로에 링크되어 있습니다.
    [root@nodejs:nodejs]# ./node
    FRENDS #6~ > var a = require('nanhapark');
    undefined
    FRENDS #6~ > var o = new a();
    undefined
    FRENDS #6~ > o
    { name: 'nanhapark', hp: '010-9864-1512' }
    FRENDS #6~ > 




# cpp module 추가방법과 make과정의 이해


## 관련파일 목록
- src/node_모듈명.cc
- src/node_모듈명.h (optional)
- src/ndoe_extensions.h
- ./wscript


----
## 추가파일
- src/node_모듈명.cc
- src/node_모듈명.h (optional)
- lib/모듈명.js

위 2개의 파일을 추가한다.

----
## lib/모듈명.js 예제

    var binding = process.binding('extend');
    exports.hello = binding.hello;

cpp 모듈을 컴파일 하게 되면 보통 `모듈명.node`으로 튀어나오고, 이를 require해서 사용했는데, native module으로 들어가 있기 때문에, 이것을 가져나올려면 `process.binding`을 사용해야 한다. 그래서 lib 안에 브릿지 역할을 하는 js를 포함한다.


----
## src/node_모듈명.h (optional)

    #ifndef node_extend_h
    #define node_extend_h

    #include <node.h>
    #include <v8.h>

    namespace node {

    class EXTEND {
    public:
      static void Initialize (v8::Handle<v8::Object> target);
    };


    }  // namespace node

    #endif  // node_extend_h



----
## src/node_모듈명.cc

    /*********************************************************
     * Node.js Black Edition
     * cpp native module: extend
     *
     * @source
     * @author nanhapark <http://about.me/nanha>
     *********************************************************/

    #include <node.h>
    #include <node_extend.h>

    #include <v8.h>

    #include <errno.h>
    #include <string.h>

    namespace node {

    using namespace v8;

    static Handle<Value> hello(const Arguments& args) {
      HandleScope scope;
      return scope.Close(String::New("wow cpp binding"));
    }


    void EXTEND::Initialize(v8::Handle<v8::Object> target) {
      HandleScope scope;

      NODE_SET_METHOD(target, "hello", hello);
    }


    }  // namespace node

    NODE_MODULE(node_extend, node::EXTEND::Initialize)



## src/node_extensions.h

    47 /*********************************************************
    48  * Node.js Black Edition
    49  *
    50  * @author nanhapark <http://about.me/nanha>
    51  *********************************************************/
    52 NODE_EXT_LIST_ITEM(node_extend)
    53 NODE_EXT_LIST_ITEM(node_base64)


----
## wscript 수정

     917     src/v8_typed_array.cc
     919     src/node_extend.cc

917 라인 이후로 추가함


----
## make build 과정에 추가됨

- lib 디렉토리에 새로운 라이브러리가 추가되어 `src/node_natives.h` 만드는 과정이 `rebuild` 되어짐
- 이로인해 `src/node_natives.h` 불러들이는 `src/node_javascript.cc` 파일이 `rebuild` 되어짐
- `src/node_extensions` 변화되어 `rebuild`
- 마지막 `node.js` 실행파일인 `out/Release/node` 생성됨. 이것은 소스트리 최상단에 `symbolic link` 되어 있어서 `make install`하지 않고, `./node` 으로 현 빌드상황을 확인할 수 있음.


## build log

    [ 3/37] src/node_natives.h: src/node.js lib/optimist.js lib/wordwrap_doc.js lib/querystring.js lib/net.js lib/restler_multipartform.js lib/vm.js lib/module.js lib/_debugger.js lib/winston_exception.js lib/moment.js lib/mkdirp.js lib/restler_doc.js lib/os.js lib/step_doc.js lib/minimatch.js lib/crypto.js lib/node_static_util.js lib/freelist.js lib/readline.js lib/def_doc.js lib/tty.js lib/cluster.js lib/graceful_fs.js lib/black_edition_info.js lib/winston_file.js lib/glob.js lib/buffer.js lib/base64.js lib/restler.js lib/node_static_doc.js lib/http.js lib/winston_transports_couchdb.js lib/mustache_doc.js lib/assert.js lib/underscore.js lib/optimist_doc.js lib/winston_container.js lib/winston_transports_loggly.js lib/url.js lib/winston_config_cli_config.js lib/winston_common.js lib/async.js lib/dns.js lib/clog.js lib/winston_config_syslog_config.js lib/_linklist.js lib/util_class.js lib/colors.js lib/buffer_ieee754.js lib/child_process.js lib/repl.js lib/clog_doc.js lib/zlib.js lib/winston_logger.js lib/dgram.js lib/winston_config.js lib/hashish.js lib/fileutils_doc.js lib/traverse.js lib/glob_doc.js lib/moment_doc.js lib/mysql.js lib/events.js lib/inherits.js lib/colors_doc.js lib/winston.js lib/winston_doc.js lib/util_prototype.js lib/step.js lib/hashish_doc.js lib/winston_transports_webhook.js lib/Class_doc.js lib/node_static_mime.js lib/mustache.js lib/commander_doc.js lib/winston_transports_transport.js lib/winston_transports_file.js lib/commander.js lib/path.js lib/sys.js lib/constants.js lib/minimatch_doc.js lib/console.js lib/fileutils.js lib/punycode.js lib/mkdirp_doc.js lib/extend.js lib/winston_transports.js lib/util.js lib/fs.js lib/node_static.js lib/inherits_doc.js lib/underscore_doc.js lib/lru_cache.js lib/winston_config_npm_config.js lib/tls.js lib/stack_trace.js lib/https.js lib/stream.js lib/graceful_fs_doc.js lib/winston_transports_console.js lib/string_decoder.js lib/wordwrap.js lib/timers.js -> out/Release/src/node_natives.h
    [ 9/37] cxx: src/node_javascript.cc -> out/Release/src/node_javascript_5.o
    /usr/bin/g++ -pthread -g -O3 -DHAVE_OPENSSL=1 -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64 -DHAVE_FDATASYNC=1 -DARCH="x64" -DPLATFORM="linux" -D__POSIX__=1 -Wno-unused-parameter -D_FORTIFY_SOURCE=2 -IRelease/src -I../src -IRelease/deps/http_parser -I../deps/http_parser -IRelease/deps/uv/include -I../deps/uv/include -IRelease/deps/uv/src/ares -I../deps/uv/src/ares -IRelease/deps/v8/include -I../deps/v8/include -I/usr/kerberos/include -Ideps/v8/include ../src/node_javascript.cc -c -o Release/src/node_javascript_5.o
    [10/37] cxx: src/node_extensions.cc -> out/Release/src/node_extensions_5.o
    /usr/bin/g++ -pthread -g -O3 -DHAVE_OPENSSL=1 -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64 -DHAVE_FDATASYNC=1 -DARCH="x64" -DPLATFORM="linux" -D__POSIX__=1 -Wno-unused-parameter -D_FORTIFY_SOURCE=2 -IRelease/src -I../src -IRelease/deps/http_parser -I../deps/http_parser -IRelease/deps/uv/include -I../deps/uv/include -IRelease/deps/uv/src/ares -I../deps/uv/src/ares -IRelease/deps/v8/include -I../deps/v8/include -I/usr/kerberos/include -Ideps/v8/include ../src/node_extensions.cc -c -o Release/src/node_extensions_5.o
    [37/37] cxx_link: out/Release/src/node_main_5.o out/Release/src/node_5.o out/Release/src/node_buffer_5.o out/Release/src/node_javascript_5.o out/Release/src/node_extensions_5.o out/Release/src/node_http_parser_5.o out/Release/src/node_constants_5.o out/Release/src/node_file_5.o out/Release/src/node_script_5.o out/Release/src/node_os_5.o out/Release/src/node_dtrace_5.o out/Release/src/node_string_5.o out/Release/src/node_zlib_5.o out/Release/src/timer_wrap_5.o out/Release/src/handle_wrap_5.o out/Release/src/stream_wrap_5.o out/Release/src/tcp_wrap_5.o out/Release/src/udp_wrap_5.o out/Release/src/pipe_wrap_5.o out/Release/src/cares_wrap_5.o out/Release/src/tty_wrap_5.o out/Release/src/fs_event_wrap_5.o out/Release/src/process_wrap_5.o out/Release/src/v8_typed_array_5.o out/Release/src/node_extend_5.o out/Release/src/node_base64_5.o out/Release/src/node_signal_watcher_5.o out/Release/src/node_stat_watcher_5.o out/Release/src/node_io_watcher_5.o out/Release/src/platform_linux_5.o out/Release/src/node_crypto_5.o out/Release/deps/http_parser/http_parser_3.o -> out/Release/node


## require 확인하기

    [root@nodejs:nodejs]# ./node
    FRENDS #6~ > var a = require('extend');
    undefined
    FRENDS #6~ > a
    { hello: [Function] }
    FRENDS #6~ > a.hello();
    'wow cpp binding'
    FRENDS #6~ > 


# 결론

아 힘드네요. ;; 좋은 하루 보내세요.
