## [1] javascript compiler에 대해서
### 1. 초기 javascript의 실행 원리
현재 우리가 HTML5 브라우저로 사용되고 있는 Safari, Chrome, Firefox에서 사용하는 javascript 엔진들은 모두 JITC(Just-In-Time compiler) 방식을 사용합니다. 기본적으로 javascript는 인터프리터(interpreter) 언어로 알고 있는 부분이 많은데, 초기에는 수행되는 모든 javascript code를 바로(Just-In-Time) Native code로 변환하여 통째로 읽는 방식을 사용했습니다.

기본적인 실행 방식은 이렇습니다.<br>
- 1. javascript는 기본적으로 text형태로 배포됩니다.
- 2. 처음에는 text 형태로 배포된 javascript code를 파싱하여 중간 언어로써 byte code 형태로 전환합니다.
- 3. 이후 native code로 변경하기 위해서 interpreter 와 JITC의 각각의 동작 방식이 달라집니다.

그럼 interpreter와 JITC의 차이점은 무엇일까요?<br>
- 1. interpreter의 경우 변환된 바이트 코드를 한줄씩 읽어나가며 동작을 수행합니다.
- 2. JITC의 경우 바이트 코드를 기반으로 전체를 native code로 컴파일하여 동작을 수행합니다.

물론, 바이트코드를 한줄씩 읽으면서 수행하는 인터프리터 방식보다, 통째로 Native code로 컴파일하여 수행하는 것이 실행 과정에서는 빠를지 모릅니다. 그러나, 많이들 알고 있는 것처럼 javascript에서는 변수에 대한 다양한 예외처리가 발생하기 마련입니다.<br>
즉, JAVA나 기타 언어에서 자료형이 명시되는 것과 달리, var 형태로 동적인 자료형이 할당되기 때문에, 이에 대한 자료형이 어떠한 것인지 통째로 Native code로 컴파일 하는 형태에서는 알기가 어렵습니다. 따라서, Native code로 변환하기 위해서는 변수에 따른 다양한 예외처리가 전체적으로 수행되어야 하기 때문에, 실행 속도면에서 잃는 부분이 더 많을 수 있습니다.

### 2. Adaptive JITC 방식
Adaptive JIT Compilation 방식이란 모든 코드를 일괄적으로 같은 수준의 최적화를 진행하는 것이 아닌, 반복 수행되는 정도에 따라 유동적으로 다른 최적화 수준을 적용하는 방식입니다. 기본적으로는 인터프리터를 통해 수행을 하다가, 자주 반복되는 코드 수행 부분이 있는 경우 관련된 부분에만 JITC를 적용하여 Native 코드로 최적화하여 변경 수행합니다.

Adaptive JITC방식은 baseline / optimizing을 두어 최소한의 최적화면 적용하는 baseline-JITC를 수행하다 더 많은 코드 최적화가 필요한 경우 Optimizing JITC로 최적화 합니다.

## [2] javascript V8 engine changing History
### 1. 2010 - Crankshaft
Crankshaft는 위에서 언급한 Adaptive JITC와 같이, Baseline과 Optimizing이 분리된 형태로써, 차이점은 인터프리터가 존재하지 않습니다. Baseline에서는 최소한의 비용으로 코드를 수행하고, 계산비용이 높은 코드의 경우 Crankshaft로 위임하여 최적화를 진행합니다. 이때의 최적화 방법은 hidden class를 이용하는데, hidden class에 대한 내용은 이후에 더 자세히 설명해야 할 것 같습니다. :)
<!-- <img src="/assets/posts/google-v8-history2.png" /> -->
### 2. 2015 - Turbofan
ES2015, asm.js, Web Assembly등 javascript 사양이 다양화되고 변화되면서, Crankshaft에서 변화에 대응하지 못하는 문제가 발생합니다. 따라서, 새로운 javascript 객체에 대한 최적화를 위해 V8에서는 Turbofan을 추가하게 됩니다. 따라서, 기존의 최적화에서 새로운 javascript 객체에 대한 최적화는 Turbonfan이 하게 됩니다.
<!-- <img src="/assets/posts/google-v8-history3.png" /> -->
### 3. 2016 - Ignition 추가 (v5.9, 6.0)
모바일 환경에 대한 최적화에 대응하기 위해 Ignition 엔진이 추가됩니다. Ignition은 Baseline 단에서 바로 Native code로 전환되지 않고, 바이트코드로 변환하는 컴파일러와 이를 실행하는 인터프리터를 내장하고 있습니다. 따라서, 실행환경에서 수행 시간이 효율적으로 단축되며 인터프리터를 통해서 이후 수행시간에 대한 최적화를 Turbofan과 함께 최적화되며, 모바일 환경에 대응되도록 설계가 가능해 졌습니다. 5.9 버전에서는 Crankshaft가 포함된 형태로 진행하나, 6.0 버전에서는 Baseline단의 Full-codegen과 Crankshaft모두 제외된 구성으로 릴리즈 됩니다.
<!-- <img src="/assets/posts/google-v8-history4.png" /> -->
<!-- <img src="/assets/posts/google-v8-history5.png" /> -->

## [3] Node.js 8.9.1 LTS
최근, Node.js에서는 6.11버전 이후 8.9.1 버전이 LTS 버전으로 릴리즈 됩니다. 릴리즈 기간이 보류되었던 이유는 6.11에서 사용된 Crankshaft가 제외된 Ignition + Turbofan 조합을 V8 엔진버전으로 채택하기 위해서 입니다. 그러면, 퍼포먼스 상에서 어떠한 이점을 가져갈 수 있게 될까요?

일단, 앞서 이야기 한 것 처럼 표면적으로 예상할 수 있는 부분은 모바일 최적화를 위한 Ignition이라고 볼 수 있을 것 같다고 생각합니다. 모바일 환경에서는 데스크탑보다 지원되는 리소스가 한정적이기 때문에, Chrome V8에서는 한정된 리소스의 모바일 디바이스에서 메모리 최적화를 위한 엔진으로 Ignition을 추가한 것입니다.

이는 즉, Node.js Application에서도 Memory 관리가 효율적으로 관리될 수 있다고 예상해 볼 수 있을 것 같습니다. 실제로 Outsider님의 섹션에서 발표한 내용에 따르면, 10%정도의 속도 상승과 더불어 응답속도는 확연하게 차이가 났으며, Throughput, 즉 서버 내에서의 처리량은 6.11보다는 다소 늦는것 같아 보이지만 대체적으로 안정성 면에서 효과적이었다는 발표내용이 있었습니다. 따라서, 앞으로 서버 운영에 있어서 8.9.1 LTS 버전의 채택도 충분히 고려해볼만 하지 않을까 개인적으로 생각합니다.

## References
- [크롬 v8 엔진 위키 (https://ko.wikipedia.org/wiki/%ED%81%AC%EB%A1%AC_V8)](https://ko.wikipedia.org/wiki/%ED%81%AC%EB%A1%AC_V8)
- [jscript.me](https://jscript.me/2017/10/16/google-javascript%EC%97%94%EC%A7%84-v8-%EB%B2%84%EC%A0%84-6%EC%84%B8%EB%8C%80-%EC%A0%84%ED%99%98%EC%99%84%EB%A3%8C/#comment-3)
- [v8 project Blogspot](https://v8project.blogspot.kr/2017/05/launching-ignition-and-turbofan.html)