# npm 설정(configuration) - ver 5.6.0 (18.02.01)
## npmrc 변수
npm이 로컬 (또는 npmrc 파일)에 설정된 매개 변수보다 사용하는 몇 가지 enironment 변수입니다. 
몇 가지 예는 NODE_ENV 및 HTTPS_PROXY입니다. npm_config_를 접두어로하여 npm 매개 변수를 설정할 수도 있습니다. 이렇게하면 export npm_config_registry = localhost : 1234와 같은 일을 할 수 있습니다.

## command line 플래그
모든 매개 변수가 파일 또는 환경 변수에 영구적으로 설정되어야하는 것은 아닙니다. 그들 중 많은 것들은 npm 명령 내에서 - 접두어로 플래그로 사용될 수 있습니다.

예를 들어, 레지스트리에서 새 패키지를 설치하고 package.json 파일에 저장하려면 --save 플래그를 사용하는 것이 좋지만 항상 그런 것은 아닙니다. 경우에 따라 --save-dev 또는 --save-optional을 사용하기를 원할 수 있으므로 npmrc를 사용하는 것이 적절하지 않을 수 있습니다.

## package.json
```json
{
  "name": "module-name",
  "version": "10.3.1",
  "config": {
    "foo": "bar",
    "test-param": 12
  },
  "dependencies": {
    "express": "4.2.x",
  }
}
```
코드 내에서 process global 변수 (process.env.npm_package_config_foo)를 사용하여 이러한 매개 변수에 액세스 할 수 있습니다. 접두어 npm_package_config_에 노드가 변수를 가져올 위치를 알려줍니다.

이는 npm 스크립트 (노드 index.js 만 사용하는 것이 아니라)를 통해 프로젝트를 실행할 때만 작동합니다.

## nom config set
마지막으로 npm config set을 통해 매개 변수를 설정할 수있는 기능이 항상 있습니다. 이는 package.json 구성보다 우선합니다. 예를 들어, 명령 행에서 npm config set module-name : foo baz를 실행 한 경우 (위의 package.json 파일이있는 경우), foo 매개 변수는 bar 대신 baz가됩니다. 모듈 이름 범위 지정은이 변수가 다른 프로젝트에 설정되지 않도록합니다.

위의 방법과 마찬가지로이 작업을 수행하려면 npm과 같이 npm 스크립트를 통해 프로그램을 실행해야합니다.

## 가능한 변수
가능한 한 최선을 다해 각 매개 변수를 분류하려했지만 많은 매개 변수가 다른 범주에서도 잘 작동했습니다. 그래서 몇 가지 고민을 한 후에 각 매개 변수를 컨텍스트에 가장 적합한 범주에 넣었습니다.

다행스럽게도 나는 이것을 잘 조직하여 잘 사용하는 참조 자료로 사용할 수 있습니다. 실수 나 누락이 있으면 알려주세요!

### Access Control/Authorization
1. *access*
	* 이 옵션은 패키지의 범위 액세스 수준을 설정하며 기본값은 restricted입니다. 이 매개 변수를 public으로 설정하면 공개적으로 볼 수 있고 설치 가능합니다. 프로젝트의 범위가 지정되지 않은 경우 프로젝트는 공개됩니다.
	* `Default`: restricted
	* `Type`: Access(string)
2. *always-auth*
	* GET 요청의 경우에도 레지스트리에 액세스 할 때마다 인증을 요구하려면 true로 설정하십시오.
	* `Default`: false
	* `Type`: boolean
3. *ca*
	* 이것은 패키지 레지스트리와의 SSL 연결을 신뢰하는 데 사용되는 인증 기관 서명 인증서입니다. 인증서를 지정하려면 PEM 형식을 사용하고 모든 줄 바꿈을 \ n 문자로 바꿉니다. 예를 들어, CA 설정은 다음과 같습니다.
	* ca="-----BEGIN CERTIFICATE-----\nXXXX\nXXXX\n-----END CERTIFICATE-----"
	* 인증서 배열 (각 줄마다 하나씩)을 지정하여 여러 CA를 신뢰할 수도 있습니다.
```
ca[]="..."  
ca[]="..." 
```
	* `Default`: The nom CA certificate
	* `Type`: string, array or null
4. *cafile*
	* ca 매개 변수와 마찬가지로 cafile을 사용하면 레지스트리에 연결하기위한 신뢰할 수있는 인증서를 설정할 수 있습니다. 차이점은 하나 이상의 인증서를 포함 할 수있는 인증서에 대한 파일 경로를 지정할 수 있다는 것입니다.
	* `Default`: null
	* `Type`: path
5. *cert*
	* cert 매개 변수는 레지스트리를 인증하기위한 클라이언트 인증서를 지정합니다. 이것은 레지스트리 인증 대신 클라이언트 인증을위한 것이라는 점에서 이전의 ca 및 cafile 인증서와 반대입니다. 자신의 레지스트리를 호스팅하는 경우 이는 사용자 이름과 비밀번호로 인증하지 않고도 비공개로 만들 수있는 좋은 방법 일 수 있습니다.
	* `Default`: null
	* `Type`: string
### Caching
1. *cache*
	* 이것은 npm의 캐시 디렉토리의 위치입니다.
	* `Default` : windows `%AppData%\npm-cache`, Posix: `~/.npm`
	* `Type`: path
2. *cache-lock-stale*
	* 캐시 폴더 잠금 파일이 고쳐지지 않게 될 때까지의 시간 (밀리 초).
	* `Default`: 60000 (1minute)
	* `Type`: number
3. *cache-lock-retries*
	* 캐시 폴더 잠금 파일에 대한 잠금을 확보하기 위해 재 시도 할 횟수.
	* `Default`: 10
	* `Type`: number
4. *cache-lock-wait*
	* 캐시 잠금 파일이 만료 될 때까지 대기하는 시간 (밀리 초)
	* `Default`: 10000(10sec)
	* `Type`: number
5. *cache-max*
	* 레지스트리로 업데이트하기 전에 항목을 캐시하는 최대 시간 (초)입니다. 따라서 패키지가 상당히 자주 변경 될 것으로 예상되면이 값을 더 낮은 숫자로 설정하는 것이 좋습니다.
	* 캐시 된 패키지를 제거하는 유일한 방법은 npm cache clean 명령을 사용하는 경우입니다 (또는 수동으로 선택하여 제거 할 패키지를 선택할 수도 있습니다).
	* `Default`: infinity
	* `Type`: number
6. *cache-min*
	* cache-max 매개 변수의 반대 인 cache-min 매개 변수는 레지스트리를 다시 검사하기 전에 캐시에 항목을 보관할 최소 시간 (초)을 설정합니다.
	* `Default`: 10
	* `Type`: number
### General
1.  *color*
	* 색상 매개 변수는 색칠이 npm 출력에 사용되는지 결정합니다. true로 설정하면 npm은 tty 파일 설명자의 색만 인쇄합니다. 또는 항상 색상을 사용하도록 설정할 수 있습니다.
	* `Default`: true on Posix, false on Windows
	* `Type`: boolean or ‘always’
2. *description*
	* npm 검색을 사용할 때 패키지 설명이 표시되는지 결정합니다.
	* `Default`: true
	* `Type`: boolean
3. *force*
	* force를 사용하면 다양한 명령이 더욱 강력 해집니다. 당신은 거의 sudo를 사용한다고 생각할 수있다. sudo는 특정 제한을 우회 할 수있다. 따라서 몇 가지 예를 들자면 라이프 사이클 스크립트 오류로 인해 진행이 차단되지 않고 게시가 이전에 게시 된 버전을 덮어 쓰고 npm이 레지스트리에서 요청할 때 캐시를 건너 뛰거나 npm 이외의 파일을 덮어 쓰지 않도록 검사하지 않도록 할 수 있습니다.
	* `Default`: false
	* `Type`: boolean
4.  *global*
	* 전역은 주어진 명령이 '전역'모드로 작동하게합니다. 이 폴더에 설치된 패키지는 시스템의 모든 사용자 및 프로젝트에서 액세스 할 수 있습니다. 즉, 패키지는 일반적으로 노드가 설치된 'prefix'폴더에 설치됩니다. 특히 전역 패키지는 {prefix} / lib / node_modules에 있으며 bin 파일은 {prefix} / bin에 링크되고 매뉴얼 페이지는 {prefix} / share / man에 링크됩니다.
	* `Default`: false
	* `Type`: boolean
5. *globalconfig*
	* 전역 구성 옵션을 위해 읽을 구성 파일의 위치입니다.
	* `Default`: {prefix}/etc/npmrc
	* `Type`: path
6. *group*
	* 이 매개 변수는 전역 모드의 패키지 스크립트를 루트 사용자로 실행할 때 사용할 시스템 그룹을 npm에 알려줍니다.
	* `Default`: groupID of the current process
	* `Type`: string or number
7. *long*
	* npm ls 및 npm search를 실행할 때 자세한 정보 표시 여부.
	* `Default`: false
	* `Type`: boolean
8. *prefix*
	* 이것은 전역 항목이 설치된 위치이며 기본적으로 npm 자체의 설치 위치입니다. 명령 줄에 prefix가 설정되어 있으면 비전 역이 아닌 명령이 해당 폴더에서 실행됩니다.
	* `Type`: path
9. *spin*
	* spin 매개 변수는 npm이 대기 중이거나 무언가를 처리하는 동안 ASCII 스피너가 표시되는지 여부를 결정합니다 (process.stderr가 TTY라고 가정). 이 값을 false로 설정하면 회 전자를 완전히 억제하거나 '항상'으로 설정하여 비 TTY 출력에 대해서도 회 전자를 출력 할 수 있습니다.
	* `Default`: true
	* `Type`: boolean or always
10. *tmp*
	* 임시 파일 및 디렉토리가 저장되는 디렉토리입니다. npm 프로세스가 성공적으로 완료되면 모든 파일과 디렉토리가 삭제됩니다. 그러나 프로세스가 실패하면 파일과 디렉토리가 검사되지 않고 문제를 디버그 할 수 있도록 파일과 디렉토리가 삭제되지 않습니다.
	* `Default`: tmpdir environment variable, or ‘/tmp’
	* `Type`: path
11. *unicode*
	* unicode 매개 변수는 트리 출력에서 ​​유니 코드 문자를 사용할 것인지 여부를 npm에 알려줍니다. false 인 경우 ASCII 문자 만 사용하여 트리를 그립니다.
	* `Default`: true
	* `Type`: boolean
12. *unsafe-perm*
	* unsafe-perm이 true로 설정되면 패키지 스크립트가 실행될 때 사용자 / 그룹 ID 전환이 억제됩니다. false 인 경우 루트가 아닌 사용자는 패키지를 설치할 수 없습니다.
	* `Default`: false if running as root, true otherwise
	* `Type`: boolean
13. *usage*
	* 사용 플래그를 사용하면 명령에 대한 도움말을 얻을 때 출력량이 줄어 듭니다. -H 플래그와 같이 명령에 가능한 모든 플래그 / 입력을 표시하는 대신 도움말 문서의 요점을 제공합니다. 예를 들어 npm --usage 검색을 실행하면 npm 검색 [일부 검색 용어 ...]가 출력됩니다.
	* `Default`: false
	* `Type`: boolean
14. *user*
	* 이것은 패키지 스크립트가 루트로 실행될 때 사용할 UID입니다. 따라서 스크립트에 루트 권한이 없도록하려면 올바른 권한 수준과 응용 프로그램에 대한 액세스 권한이있는 사용자의 UID로 설정하십시오. 루트로서 패키지 스크립트를 실행하는 것은 위험 할 수 있습니다!
	* `Default`: nobody
	* `Type`: string or number
15. *userconfig*
	* 이것은 사용자 레벨 구성 파일의 위치입니다. 시스템의 각 사용자는 npm 설치에 대해 서로 다른 설정을 가질 수 있으며 파일은 userconfig에 지정된 경로에 있어야합니다.
	* `Default`: ~/.npmrc
	* `Type`: path
16. *unmask*
	* 이것은 파일과 디렉토리 모두에 대해 파일 작성 모드를 설정할 때 사용할 마스크 값입니다. 생성되는 파일 / 디렉토리 유형은 사용 된 마스크 값에 따라 다릅니다. 디렉토리 또는 실행 파일 인 경우 umask 값은 0777에 대해 마스크됩니다. 다른 모든 파일의 경우 umask 값은 0666에 대해 마스크됩니다. 기본값은 각 파일 유형에 대해 상당히 보수적 인 마스크 인 0755 및 0644입니다.
	* `Default`: 022
	* `Type`: 0000..0777 범위의 8 진수 문자열 (0..511)
17. *version*
	* 이 플래그를 사용하면 설치된 npm의 버전이 출력됩니다. 이것은 명령 행에서 npm --version과 같은 플래그로 사용될 때만 작동합니다.
	* `Default`: false
	* `Type`: boolean
18. *versions*
	* 이 플래그를 사용하는 것은 버전과 비슷하지만 현재 디렉토리 (있는 경우), V8, npm 및 process.versions의 세부 정보를 포함하여 몇 가지 패키지에 버전 정보 (JSON)를 출력합니다. 이는 npm -versions와 같은 플래그로 명령 행에서 사용될 때만 작동합니다.

예제 출력은 다음과 같습니다.
```json
{ 'my-project': '0.0.1',
  npm: '2.14.2',
  http_parser: '2.3',
  modules: '14',
  node: '0.12.2',
  openssl: '1.0.1m',
  uv: '1.4.2-node1',
  v8: '3.28.73',
  zlib: '1.2.8' }
```
	* `Default`: false
	* `Type`: boolean
19. *viewer*
	* 도움말 콘텐츠를 볼 때 사용할 프로그램입니다. '브라우저'로 설정하면 기본 웹 브라우저가 열리고 HTML로 도움말 콘텐츠가 표시됩니다.
	* `Default`: ‘man’ on Posix, ‘browser’ on Windows
	* `Type`: path, ‘main’ or ‘browser’
### Development
1. *dev*
	* 패키지를 설치할 때이 플래그를 사용하면 dev-dependencies 패키지도 설치됩니다. 프로덕션 환경에서 프로젝트를 실행하지 않을 때는 거의 항상이 옵션을 사용해야합니다. 이것은 npat 플래그와 유사합니다.
	* `Default`: false
	* `Type`: boolean
2. *editor*
	* 이것은 편집기를 열 때 실행할 명령 (또는 실행 파일 경로)입니다.
	* `Default`: EDITOR 환경 변수를 설정하거나 Posix에서 "vi"또는 Windows에서 "메모장"으로 설정하십시오.
	* `Type`: path
3. *engine-strict*
	* 이 매개 변수는 npm에게 package.json 파일의 엔진 스펙을 엄격하게 따라야하는지 여부를 알려줍니다. true로 설정하면 현재 Node.js 버전이 지정된 버전과 일치하지 않으면 패키지 설치가 실패합니다.
	* 패키지에 특정 Node.js 버전 또는 io.js가 필요한 경우 유용합니다 (패키지가 ES6 기능을 사용하기 때문일 수 있습니다).
	* `Default`: false
	* `Type`: boolean
4. *git*
	* 이것은 git 명령을 실행하는 데 사용해야하는 명령이어야합니다. 이것은 git이 설치되었을 때 유용 할 수 있지만, PATH에없는 경우에는 git install의 경로를 지정하십시오.
	* `Default`: ‘git’
	* `Type`: string
5.  *git-tag-version*
	* 이것은 npm version 명령을 실행할 때 커밋을 태그해야하는지 npm에게 알려줍니다 (패키지 버전을 범프하여 package.json에 저장). 이것은 실수를 줄이는 데 도움이 될 수 있습니다 (git 커밋에 태그를다는 것을 잊어 버린 것, 잘못된 버전으로 태그를 지정하는 것 등).하지만 컨트롤이 적기 때문에 트레이드 오프의 무게를 줄여야합니다.
	* `Default`: true
	* `Type`: boolean
6. *heading*
	* 디버그 정보를 출력 할 때 인쇄 할 문자열입니다.
	* `Default`: npm
	* `Type`: string
7. *if-present*
	* npm run-script 명령을 사용할 때 script가 package.json 파일에 정의되어 있지 않으면 npm은 오류 코드와 함께 종료됩니다. if-present가 true로 설정되면 오류 코드가 리턴되지 않습니다. 이 옵션은 스크립트를 선택적으로 실행하려고 할 때 유용하지만, 스크립트가없는 경우에는 신경 쓰지 않아도됩니다. 따라서 예를 들어 일부 프로젝트에는 스크립트 (스크립트 A)가 있지만 전부는 아니며 다른 일반 스크립트 (스크립트 B)를 사용하여 실행할 수 있습니다. 이 방법으로 스크립트 A가 없으면 스크립트 B는 오류를 내지 않으며 안전하게 계속 실행될 수 있습니다.
	* `Default`: false
	* `Type`: boolean
8. *ignore-scripts*
	* 프로젝트의 package.json 파일에 정의 된 스크립트를 실행하지 않으려면이 플래그를 설정하십시오.
	* `Default`: false
	* `Type`: boolean
9. ignore-prepublish
	* true이면 npm은 사전 게시 스크립트를 실행하지 않습니다.
	* `Default`: false
	* `Type`: boolean
10. *init-module*
	* 이것은 프로젝트 초기화에 도움이되는 JavaScript 파일의 경로입니다. 따라서 새 프로젝트에 Bluebird 또는 기본 엔진에 대한 종속성과 같은 모든 사용자 지정 구성을 적용하려는 사용자 지정 구성이있는 경우 초기화를 처리하도록 지정된 위치에 파일을 만들 수 있습니다.
	* `Default`: ~/.npm-init.js
	* `Type`: path
11. *init-author-name*
	* 새 프로젝트를 만들 때 npm init이 사용하는 기본 이름입니다.
	* `Default`: ‘’
	* `Type`: string
12. *init-author-email*
	* 새 프로젝트를 만들 때 npm init이 사용하는 기본 작성자 전자 메일입니다.
	* `Default`: ‘’
	* `Type`: string
13. *init-author-url*
	* 새 프로젝트를 만들 때 npm init이 사용하는 기본 작성자 URL입니다.
	* `Default`: ‘’
	* `Type`: string
14.  *init-license*
	* 새로운 프로젝트를 생성 할 때 npm init이 사용하는 기본 라이센스.
	* `Default`: ISC
	* `Type`: string
15. *init-version*
	* 새로운 프로젝트를 생성 할 때 npm init이 사용하는 기본 버전.
	* npm은이 기능이 실험적이며 hte JSON 객체의 구조가 변경 될 수 있다고 주장합니다.
	* default: false
	* type: boolean
16. *link*
	* link를 true로 설정하면 로컬 설치가 전역 패키지 설치에 링크됩니다 (일치하는 패키지가있는 경우). 이 기능의 중요한 부산물 중 하나는 글로벌 패키지에 링크함으로써 로컬 설치가 글로벌 공간에 다른 것들을 설치하게 할 수 있다는 것입니다.
	* 두 조건 중 적어도 하나가 충족되면 링크가 생성됩니다.
	* 패키지가 전 세계적으로 설치되어 있지 않습니다.
	* 전역 적으로 설치된 버전은 로컬에 설치되는 버전과 동일합니다.
	* `Default`: false
	* `Type`: boolean
17. *local-address*
	* 이것은 npm 레지스트리에 연결할 때 사용할 시스템의 로컬 네트워킹 인터페이스의 IP 주소입니다.
	* 노드 v0.12 및 이전 버전의 IPv4 주소 여야합니다.
	* `Default`: undefined
	* `Type`: IP address
18. *loglevel*
	* 이것은 응용 프로그램을 실행할 때의 기본 로그 레벨입니다. 여기서 주어진 로그 이벤트보다 높거나 같으면 로그 이벤트가 사용자에게 출력됩니다. 응용 프로그램에 오류가 발생하면 모든 로그가 현재 작업 디렉토리의 npm-debug.log에 기록됩니다.
	* `Default`: ‘warn’
	* `Type`: string
19. *logstream*
	* 런타임에 npmlog 패키지에서 사용하는 스트림입니다.
	* 이것은 명령 행에서 설정할 수 없습니다. 파일이나 환경 변수와 같은 다른 방법을 사용하여 구성해야합니다.
	* `Default`: process.stderr
	* `Type`: stream
20. *message*
	* 이것은 npm version 명령에서 사용할 커밋 메시지입니다. '% s'포맷팅 문자는 버전 번호로 대체됩니다.
	* `Default`: ‘%s’
	* `Type`: string
21. *node-version*
	* package.json 파일에서 패키지의 엔진 선언을 확인할 때 사용되는 노드 버전입니다.
	* `Default`: process.version
	* `Type`: server or false
22. *npat*
	* 설치시 패키지 테스트 실행 여부.
	* `Default`: false
	* `Type`: boolean
23. *onload-script*
	* 이것은 npm이로드되면 requre () 패키지의 위치입니다. 이는 npm을 프로그래밍 방식으로 사용하는 경우에 권장됩니다.
	* `Default`: true
	* `Type`: boolean
24. *parseable*
	* parseable 매개 변수는 표준 출력에 쓸 때 구문 분석 가능한 형식으로 출력 형식을 지정하도록 npm에 지시합니다.
	* `Default`: false
	* `Type`: boolean
25. *production*
	* true로 설정하면 npm은 프로덕션 모드에서 실행되며 대부분 devDependencies가 설치되지 않음을 의미합니다. 라이프 사이클 스크립트를 사용할 때는 NODE_ENV = "production"환경 변수를 사용해야합니다.
	* `Default`: false
	* `Type`: boolean
26. *rollback*
	* 이 플래그를 npm과 함께 사용하면 설치에 실패한 패키지가 제거됩니다 (예 : 컴파일 / 종속성 오류로 인해).
	* `Default`: true
	* `Type`: boolean
27. *save*
	* 이 플래그를 npm과 함께 사용하면 주어진 패키지를 로컬 package.json 파일에 의존성하에 저장합니다. 또는이 플래그를 npm rm 명령과 함께 사용하면 package.json 파일의 종속성 섹션에서 종속성이 제거됩니다.
	* 이 옵션은 package.json 파일이 현재 디렉토리에있는 경우에만 작동합니다.
	* `Default`: false
	* `Type`: boolean
28. *save-bundle*
	* 패키지가 --save, --save-dev 또는 --save-optional 플래그를 사용하여 설치시 저장되는 경우 bundleDependencies 목록에도 넣습니다. npm rm 명령과 함께 사용하면 bundledDependencies 목록에서 제거됩니다.
	* `Default`: false
	* `Type`: boolean
29. *save-dev*
	* 이 플래그를 사용하면 package.json 파일의 devDependencies 목록에 패키지가 저장됩니다. npm rm과 함께 사용하면 패키지가 devDependencies에서 제거된다는 것을 의미합니다. save 플래그와 마찬가지로 package.json 파일이있는 경우에만 작동합니다.
	* `Default`: false
	* `Type`: boolean
30. *save-exact*
	* 종속성이 --save, --save-dev 또는 --save-optional 플래그 중 하나를 사용하여 package.json 파일에 저장되면 npm의 기본 semver 범위 연산자 대신 정확한 버전 번호를 사용하여 구성됩니다.
	* `Default`: false
	* `Type`: boolean
31. *save-optional*
	* 이 플래그를 사용하면 package.json 파일의 optionalDependencies 목록에 패키지를 저장합니다. npm rm과 함께 사용하면 패키지가 optionalDependencies에서 제거된다는 것을 의미합니다. save 플래그와 마찬가지로 package.json 파일이있는 경우에만 작동합니다.
	* `Default`: false
	* `Type`: boolean
32. *save-prefix*
	* 이 매개 변수는 --save 또는 --save-dev 플래그와 함께 사용되는 경우 package가 package.json에 저장되는 방법을 결정합니다. 예를 들어 기본값을 사용하여 버전 1.2.3으로 패키지를 저장하면 package.json에 ^ 1.2.3으로 실제로 저장됩니다.
	* `Default`: ‘^’
	* `Type`: string
33. *option*
	* "dev"또는 "development"및 실행중인 로컬 npm을 인수없이 설치하면 devDependencies (및 해당 종속성) 만 설치됩니다.
	* "dev"또는 "development"및 실행중인 로컬 npm ls, npm 구식 또는 npm update가 --dev의 별칭입니다.
	* "prod"또는 "production"을 실행하고 로컬 npm을 실행할 때 인수없이 설치하면 non-devDependencies (및 해당 종속성) 만 설치됩니다.
	* “prod"또는 "production"을 실행하고 로컬 npm ls, npm 구식 또는 npm update를 실행하는 경우 --production의 별칭입니다.
	* `Default`: null
	* `Type`: string
34. *optional*
	* optionalDependencies 객체에 패키지를 설치하려고 시도합니다. 이 패키지를 설치하지 않으면 전체 설치 프로세스가 중단되지 않습니다.
	* `Default`: true
	* `Type`: boolean
35. *scope*
	* 스코프를 사용하면 범위 레지스트리에 어떤 스코프를 사용할 지 알려줍니다. 이것은 개인 레지스트리를 처음 사용할 때 유용 할 수 있습니다.
npm login --scope = @ organization --registry = registry.example.com
	* 이로 인해 @organization은 @ organization / package 패턴에 따라 지정된 패키지의 향후 설치를 위해이 레지스트리로 맵핑됩니다.
	* `Default`: ‘’
	* `Type`: string
36. *shrinkwrap*
	* false 인 경우 npm-shrinkwrap.json 파일은 설치 중에 무시됩니다.
	* `Default`: true
	* `Type`: boolean
37. *package-lock*
	* false로 설정하면 설치시 package-lock.json 파일을 무시하십시오. save가 true이면 package-lock.json을 쓰지 못하게됩니다.
	* 이 옵션은 --shrinkwrap의 별명입니다.
	* `Default`: true
	* `Type`: boolean
38. *package-lock-only*
	* true로 설정하면 node_modules를 확인하고 종속성을 다운로드하는 대신 package-json 만 업데이트합니다.
	* `Default`: false
	* `Type`: boolean
39. *prefer-offline*
	* true 인 경우 캐시 된 데이터의 스 태 니스 검사가 무시되지만 서버에서 누락 된 데이터가 요청됩니다. 전체 오프라인 모드를 강제 실행하려면 --offline을 사용하십시오.
	* 이 옵션은 사실상 --cache-min = 9999999와 동일합니다.
	* `Default`: false
	* `Type`: boolean
40. *prefer-online*
	* true 인 경우 캐시 된 데이터에 대한 staleness 검사가 강제 실행되므로 CLI는 새로운 패키지 데이터에 대해서도 즉시 업데이트를 찾습니다.
	* `Default`: false
	* `Type`: boolean
41. *sign-git-tag*
	* npm version 명령을 실행하고이 플래그를 사용하면 태그 지정 중에 -s 플래그를 사용하여 서명을 추가합니다. 이 기능을 사용하려면 이미 git configs에 GPG 키를 설정해야합니다.
	* `Default`: false
	* `Type`: boolean
42. *tag*
	* npm에서 패키지를 설치하고 버전을 지정하지 않으면이 태그가 대신 사용됩니다.
	* `Default`: latest
	* `Type`: string
43. *tag-version-prefix*
	* npmversion을 사용할 때 패키지 버전 앞에 추가 된 문자. 다른 프로그램에 버전에 대한 스타일 규칙이있는 경우 유용합니다.
	* `Default`: ‘v’
	* `Type`: string
### Networking
1. *https-proxy*
	* 나가는 HTTPS 연결에 사용되는 프록시입니다. 다음 환경 변수 중 하나가 설정되면 HTTPS_PROXY, https_proxy, HTTP_PROXY, http_proxy 대신 사용됩니다.
	* `Default`: null
	* `Type`: url
2. *proxy*
	* 나가는 HTTP 연결에 사용되는 프록시입니다. 다음 환경 변수 중 하나가 설정되면 대신 사용됩니다 (HTTP_PROXY, http_proxy).
	* `Default`: null
	* `Type`: url
3. *strict-ssl*
	* 이것은 HTTPS를 통해 레지스트리에 연결하기 위해 SSL을 사용할 것인지 여부를 npm에 알려줍니다.
	* `Default`: true
	* `Type`: boolean
4. *user-agent*
	* HTTP (S) 요청에 대한 User-Agent 요청 헤더를 설정합니다.
	* `Default`: node/{process.version} {process.platform} {process.arch}
	* `Type`: string
### Registry
1. *fetch-retries*
	* npm이 패키지를 가져 오기 위해 레지스트리에 접속하려고 시도하는 횟수.
	* `Default`: 2
	* `Type`: number
2. *fetch-retry-factor*
	* 패키지를 가져올 때 사용할 재시도 모듈의 "factor"구성입니다.
	* `Default`: 10
	* `Type`: number
3. *fetch-retry-mintimeout*
	* 레지스트리에서 패키지를 가져올 때 시간 초과되기까지 대기하는 최소 시간.
	* `Default`: 10000(10sec)
	* `Type`: number(milliseconds)
4. *fetch-retry-maxtimeout*
	* 레지스트리에서 패키지를 가져올 때 시간 초과되기까지 대기하는 최대 시간.
	* `Default`: 10000(10sec)
	* `Type`: number(milliseconds)
5. *key*
	* 이것은 레지스트리로 인증 할 때 사용할 클라이언트 키입니다.
	* `Default`: null
	* `Type`: string
6. *registry*
	* 패키지 가져 오기 및 게시에 사용할 레지스트리의 URL입니다.
	* `Default`: https://registry.npmjs.org
	* `Type`: url
7. *searchopts*
	* 레지스트리를 검색하는 데 항상 사용되는 공백으로 구분 된 옵션 목록입니다.
	* `Default`: ‘’
	* `Type`: string
8. *searchexclude*
	* 항상 레지스트리를 검색하는 데 사용되는 공백으로 구분 된 제한 목록입니다.
	* `Default`: “”
	* `Type`: string
9. *searchsort*
	* 결과에서 어떤 필드를 정렬해야하는지 나타냅니다. 정렬 순서를 바꾸려면 접두사 앞에 -를 붙이십시오.
	* `Default`: "name"
	* `Type`: String
	* `Values`: "name", "-name", "date", "-date", "description", "-description", "keywords", "-keywords"