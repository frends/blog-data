{
    "title": "Server Side javaScript based on Rhino",
    "author": "frends",
    "date": "2012-05-14T07:42:44.701Z",
    "categories": ["rhino"],
    "tags": [],
    "acceptComment": true,
    "acceptTrackback": true,
    "published": "2012-05-14T07:42:44.701Z",
    "status": "public",
    "important": false,
    "advanced": {}
}

먼저 ContextFactory Class에서 제공하는 observeInstructionCount(Context, int) Method를 Overriding 하는 것으로 시작한다.

이 Method는 본래 실행할 Javascript 코드의 Instruction 개수를 제한을 두어, 실행 제한을 두기 위해 사용하는데, 이를 재정의하면 Timeout 기능 구현이 가능하겠다.

observeInstructionCount의 기본 구현은 실행 코드의 Instruction의 갯수에 제한을 둘 경우에만 작동하는데, 이는 Context Class의 setInstructionObserverThreshold(int count)로 실행 코드 갯수에 제한을 둘 수 있다.

친절하게도, ContextFactory의 API 문서를 보면 아래와 같은 코드가 첫장에 소개되어있다. 아래의 코드는 실행 코드가 10초가 넘을 경우 Error를 발생하게 하는 예제이다. (http://www.mozilla.org/rhino/apidocs/org/mozilla/javascript/ContextFactory.html)

```java
import org.mozilla.javascript.*;

 class MyFactory extends ContextFactory
 {

     // Custom Context to store execution time.
     private static class MyContext extends Context
     {
         long startTime;
     }

     static {
         // Initialize GlobalFactory with custom factory
         ContextFactory.initGlobal(new MyFactory());
     }

     // Override makeContext()
     protected Context makeContext()
     {
         MyContext cx = new MyContext();
         // Make Rhino runtime to call observeInstructionCount
         // each 10000 bytecode instructions
         cx.setInstructionObserverThreshold(10000);
         return cx;
     }

     // Override hasFeature(Context, int)
     public boolean hasFeature(Context cx, int featureIndex)
     {
         // Turn on maximum compatibility with MSIE scripts
         switch (featureIndex) {
             case Context.FEATURE_NON_ECMA_GET_YEAR:
                 return true;

             case Context.FEATURE_MEMBER_EXPR_AS_FUNCTION_NAME:
                 return true;

             case Context.FEATURE_RESERVED_KEYWORD_AS_IDENTIFIER:
                 return true;

             case Context.FEATURE_PARENT_PROTO_PROPERTIES:
                 return false;
         }
         return super.hasFeature(cx, featureIndex);
     }

     // Override observeInstructionCount(Context, int)
     protected void observeInstructionCount(Context cx, int instructionCount)
     {
         MyContext mcx = (MyContext)cx;
         long currentTime = System.currentTimeMillis();
         if (currentTime - mcx.startTime > 10*1000) {
             // Context가 생성되고 스크립트가 실행될때, 10초가 넘을 경우 Script를 중단한다.
             // 이 경우에는 실행 코드 자체가 중단된다.
             throw new Error();
         }
     }

     // Override doTopCall(Callable,
                               Context, Scriptable,
                               Scriptable, Object[])
     // doTopCall의 경우, 실제 스크립트를 evaluateString 등으로 실행 시킬 때 실행 시점 전에 호출되는 Method이다.
     // 그렇기 때문에 실행 전에 시작 시간을 저장해놓았다.
     protected Object doTopCall(Callable callable,
                                Context cx, Scriptable scope,
                                Scriptable thisObj, Object[] args)
     {
         MyContext mcx = (MyContext)cx;
         mcx.startTime = System.currentTimeMillis();

         return super.doTopCall(callable, cx, scope, thisObj, args);
     }

 }
```

그런데 만약, 실행해야할 Script가 하나가 아니고 여러개일 경우라면 어떨까?

Script를 실행 시켜주는 Shell이 있고, 여러 Script를 받아서 제한 시간내에 실행시키고 결과를 출력해야 하는 경우라면, 위의 코드를 그대로 쓰기에는 좀 부족하다.

단순히 Error를 Throw 하기 보다 일종의 Runtime Exception을 던지는게 훨신 깔끔해 보일듯 하다.

그래서 위 코드의 observeInstructionCount를 Context.throwAsScriptRuntimeEx를 사용하여 아래처럼 바꿔보았다.

```java
...
protected void observeInstructionCount(Context cx, int instructionCount)
    {
      MyContext mcx = (MyContext)cx;
        mcx.endTime = System.currentTimeMillis() - mcx.startTime;
        if ( mcx.endTime > limitedTime*1000 ) {
             // Context에 throwAsScriptRuntimeEx라는 Exception이 있다.
             // 이를 활용하면 프로그램 자체가 Error로 중단되는 일은 없겠다.
             throw Context.throwAsScriptRuntimeEx(new Exception("Timeout"));
        }
    }
...
```

여기까지 일단 10초 내로 Timeout을 거는건 해결되었다. 그런데, 스크립트 별로 제한 시간을 달리 둘 수도 있을 것이다.

이런 경우는 프로그램을 작성하기 나름이라 어느것이 효율적이라고 잘라 말하기는 어렵다.

여기서는 Rhino의 Context Class의 Method를 활용하는 쪽으로 예를 들어본다.

아래의 코드는 다른 Class에서 위의 코드를 사용해 Javascript를 실행하는 예제이다.


```java
import org.mozilla.javascript.*;

public class TestScript {
    // ContextFactory를 앞에서 정의한 MyFactory Class의 Instance로 대체한다.
    // 아래의 Static Block은 앞에서 작성한 MyFactory 에서도 정의되어 있는데,
    // 이는 실제 Context에 진입해 코드를 실행할때 필요하기 때문에 여기에 두었다.(즉, 위에서는 삭제해도 되겠다.)
    static {
        ContextFactory.initGlobal(new MyFactory());
    }

    public static void main(String[] args)
    {
        Context cx = Context.enter();
        Long runningTime = 0;
        try {
            Scriptable scope = cx.initStandardObjects();

            // 실행하기 전에 해당 Thread 영역에 제한시간을 설정한다.
            // Context Class의 putThreadLocal과 getThreadLocal을 사용하여 값을 전달한다.
            cx.putThreadLocal("limitedTime", 5);

            cx.evaluateString(scope, "while(1){};", "MySource", 1, null);

        } finally {
            runningTime = (Long)cx.getThreadLocal("executionTime");
            Context.exit();
        }
        System.out.println("Running time : "+runningTime+"s");
    }
}
```

위의 코드에서 Context에 "limitedTime" 이라는 Key로 5란 값을 Value로 설정했고, "executionTime" 이라는 Key로 최종 실행 시간을 가져오도록 설정했다.  이에 맞춰서 기존의 MyContextFactory에도 일부 코드가 수정되어야 한다. 수정 코드는 아래와 같다.

```java
...
   // Override observeInstructionCount(Context, int)
    protected void observeInstructionCount(Context cx, int instructionCount)
    {
      MyContext mcx = (MyContext)cx;

        // 현재 시점에서의 실행 시간을 의미한다.
        long currentRunningTime = System.currentTimeMillis() - mcx.startTime;

        // 이 부분에서 앞에서 설정한 값을 Thread 영역에서 가져온다.
        long limitedTime = (Long)(cx.getThreadLocal("limitedTime"));

        // 이 부분에서 실행시간이 'limitedTime' 초를 넘어설 경우를 검사한다.
        if ( currentRunningTime > limitedTime*1000 ) {

             // 중단된 지점까지의 실행 시간을 executionTime으로 값을 저장한다.
             cx.putThreadLocal("executionTime", currentRunningTime);

             throw Context.throwAsScriptRuntimeEx(new Exception("Timeout"));
        }
    }

    // 정상적으로 종료되었을 경우를 고려해 이 부분에 추가로 종료시간을 Context에 저장한다.
    protected Object doTopCall(Callable callable,
                               Context cx, Scriptable scope,
                               Scriptable thisObj, Object[] args)
    {
      MyContext mcx = (MyContext)cx;
      mcx.startTime = System.currentTimeMillis();

      Object retObj = super.doTopCall(callable, cx, scope, thisObj, args);

      // 최종 실행 시간을 executionTime으로 값을 저장한다.
      long finalRunningTime = System.currentTimeMillis() - mcx.startTime;

        cx.putThreadLocal("executionTime", finalRunningTime);
        return retObj;
    }
...
```

위의 코드는 [http://jsonlinejudge.appspot.com](http://jsonlinejudge.appspot.com) 에서 코드의 실행시간을 측정하기 위한 방법으로 사용되었다.