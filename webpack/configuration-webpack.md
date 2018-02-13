# Webpack configuration
## context
입력 옵션을 해결하기위한 기본 디렉토리 (절대 경로!) 입니다. output.pathinfo가 설정되면 포함 된 pathinfo가 설정해놓은 congtext 디렉토리로 단축됩니다.
`Default`: process.cwd()
## entry
번들을 설정하기 위한 진입점 입니다.
* `String` : 문자열이 들어오는 경우 시작시 로드되는 모듈로 해석
* `Array`: 시작시 모든 모듈이 로드됩니다. 마지막 하나가 내보내집니다.
* `Object`: 다중 항목 번들이 작성됩니다. *key는 청크되는 파일의 이름*입니다. 값은 문자열 또는 배열이 가능합니다.
```js
{
	entry: {
		page1: "./page1",
		page2: ["./entry1", "./entry2"]
	},
	output: {
		// Make sure to use [name] or [id] in output.filename
		//  when using multiple entry points
		filename: "[name].bundle.js",
		chunkFilename: "[id].bundle.js"
	}
}
```
## output
* 파일 번들링에 영향을 주는 옵션입니다. 번들링 옵션은 webpack에 컴파일된 파일을 디스크에 작성하는 방법을 알려줍니다. 다중 진입점이 있을 수 있으나, 디스크 작성에 관련된 경로는 하나의 구성만 지정됩니다.
* 해싱([hash] 또는 [chunkhash])를 사용하는 경우 모듈에 일관된 순서가 있어야 합니다. 순서를 사용하기 위해 `OccurrenceOrderPlugin` 또는 `recordsPath`를 사용해야 합니다.
### 1) filename
* 디스크의 작성될 파일 이름을 지정합니다.
* 해당 옵션에 절대 경로를 지정하면 안됩니다.
* `output.path` 옵션은 팡리이 기록되는 디스크상의 위치를 결정합니다. `filename`은 개별 파일의 이름을 짓기 위해서만 사용됩니다.

*Single entry 경우 예*
```js
{
  entry: './src/app.js',
  output: {
    filename: 'bundle.js',
    path: __dirname + '/build'
  }
}

// writes to disk: ./build/bundle.js
```

*Multiple 설정일 경우의 예*
번들링 구성에 있어 여러개의 진입점이 있거나, CommonsChunkPlugin과 같은 플러그인을 사용할 때 하나 이상의 청크파일이 생성되면 각 파일에 고유한 이름이 생성되는지 확인해봐야 합니다.

* `[name]`은 청크의 이름으로 바뀝니다.
* `[hash]`는 컴파일 해시로 바뀝니다.
* `[chunkhash]`는 청크의 해시로 대체됩니다.

```js
{
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/build'
  }
}

// writes to disk: ./build/app.js, ./build/search.js
```

### 2) path
출력 경로는 절대경로(필수) 입니다.
`[hash]`는 컴파일된 해시로 전환됩니다.

### 3) publicPath
* `publicPath`는 브라우저에서 참조될때 출력 파일의 공용 URL 주소를 지정합니다. `<script>` 태그, `<link>`태그 또는 이미지와 같은 참조 에셋을 포함하는 로더의 경우, publicPath는 디스크에서 path에 지정된 위치와 다른 경우 파일에 대한 href 또는 url() 로 사용됩니다.
* 이 기능은 다른 도메인이나 CDN에서 일부 또는 모든 출력 파일을 호스팅하려는 경우 유용할 수 있습니다.
* Webpack Dev Server는 또한 해당 option을 사용하여 출력파일이 제공될 것으로 예상되는 경로를 결정합니다. 경로와 마찬가지로 더 나은 캐싱 프로파일을 위해서 `[hash]` 로 대체할 수 있습니다.
* *Example*
	* config.js
```js
output: {
	path: "/home/proj/public/assets",
	publicPath: "/assets/"
}
```
	* index.html
```html
<head>
  <link href="/assets/spinner.gif"/>
</head>
```
asset에 CDN과 해시를 사용하는 예입니다.
	* config.js
```js
output: {
	path: "/home/proj/cdn/assets/[hash]",
	publicPath: "http://cdn.example.com/assets/[hash]/"
}
```
출력되는 파일의 최종 publicPath가 컴파일에 알려지지 않는 경우, 공백으로 남겨두고 런타임시 엔트리 포인트 파일에서 동적으로 설정할 수 있습니다. 컴파일하는 동안 publicPath를 모르는 경우에는 이를 생략하고 진입점에서 __webpack_public_path__를 설정할 수 있습니다.
```js
__webpack_public_path__ = myRuntimePublicPath

// rest of your application entry
```

### 4) chunkFilename
`output.path` 디렉토리 내의 상대 경로로서 엔트리가 아닌 청크의 파일 이름입니다.

* `[id]` 는 청크의 id로 대체합니다.
* `[name]`은 청크의 이름으로 대체됩니다. 청크에 이름이 없는 경우 id로 변경됩니다.
* `[hash]`는 컴파일 해시로 대체됩니다.
* `[chunkhash]`는 청크의 해시로 대체됩니다.

### 5) sourceMapFilename
javascript 파일의 source map 파일 이름입니다. sourcemap은 output.path 디렉토리 내에 존재합니다.
* `[file]`은 javascript 파일의 파일 이름으로 대체됩니다.
* `[id]`는 청크의 id로 대체됩니다.
* `[hash]`는 컴파일 해시로 대체됩니다.

### 6) devtoolModuleFilenameTemplate
생성된 SourceMap의 소스 배열에 대한 템플릿 문자열 입니다.
* `[resource]`는 webpack이 파일을 분석하는데 사용하는 경로로 대체되며, 쿼리 매개 변수를 포함하여 가장 오른쪽의 로더로 연결됩니다. (로더가 있는 경우)
* `[resource-path]`는 resource와 같지만 로더 쿼리에 대한 매개 변수가 없습니다.
* `[loaders]`는 가장 오른쪽 로더의 이름까지에 대한 로더 및 매개변수의 목록입니다.
* `[all-loaders]`는 가장 오른쪽의 로더 (자동 로더 포함) 이름까지의 로더 및 매개변수의 목록입니다.
* `[id]`는 모듈의 id로 대체됩니다.
* `[hash]`는 모듈 식별자의 해시로 대체됩니다. 
* `[absolute-resource-path]`는 파일의 절대경로로 바뀝니다.
| Default (devtool=[inline-]source-map) : "webpack:///[resource-path]"
| Default (devtool=eval):  "web pack:///[resource-path]?[loaders]"
| Default (devtool=eval-source-map): "webpack:///[resource-path]?[hash]"

문자열 템플릿 대신 함수로 정의할수도 있습니다. 함수는 다음과 같은 속성을 표시하는 객체 매개변수를 허용합니다.
	* identifier
	* shortIdentifier
	* resource
	* resourcePath
	* absoluteResourcePath
	* allLoaders
	* query
	* moduleId
	* hash
### 7) devtoolFallbackModuleFilenameTemplate
`output.devtoolModuleFilenameTemplate`과 유사하지만, 중복된 모듈 식별자의 경우 사용됩니다.
| Default: "webpack:///[resourcePath]?[hash]"
### 8) devtoolLineToLine
* 지정된 모듈 또는 모든 모듈에 대해 line to line 매핑 모드를 사용합니다.
line to line 모드는 생성된 소스의 각 라인이 원래 소스의 동일한 라인에 매핑되는 간단한 sourcemap을 사용합니다. 성능 최적화 방법이며 입력행이 생성된 행과 일치해야 하는 경우에만 사용을 권장합니다.
* `true`일 경우 모든 모듈에 포함됩니다. (권장사항 아님)
* module.loaders와 유사한 객체 `{test, include, exclude}`는 특정 파일에 대해 해당 옵션을 가능하게 만듭니다.
| Default: disabled
### 9) hotUpdateChunkFilename
Hout update chunk의 파일 이름입니다. 해당 파일은 output.path 디렉토리 내에 존재합니다.
* `[id]`는 chunk의 id로 대체됩니다.
* `[hash]`는 컴파일 해시로 전환됩니다. (레코드에 저장된 마지막 해시값)
| Default: "[id].[hash].hot-update.js"
### 10) hotUpdateMainFilename
Hot update 의 메인 파일 이름입니다. output.path 디렉토리 내에 존재합니다.
`[hash]`는 컴파일 해시로 저장됩니다. (레코드에 저장된 마지막 해시값)
| Default: "[hash].hot-update.json"
### 11) jsonpFunction
* chunk의 비동기 로딩을 위해서 webpack에서 사용되는 jsonp 함수입니다.
* 기능이 짧을수록 파일 크기가 약간 줄어들 수 있습니다. 한 페이지에 여러 webpack 인스턴스가 존재하는 경우 다른 식별자를 사용하시기 바랍니다.
| Default: "webpackJsonp"
### 12) hotUpdateFunction
* 최신으로 업데이트 된 chunk를 비동기적으로 로드하기 위해 webpack에서 사용하는 JSONP 함수입니다.
| Default: "webpackHotUpdate"
### 13) pathinfo
모듈에 대한 정보가 포함된 주석을 표시합니다. 배포모드에서는 사용하지 마십시오
`require(/* ./test */23)`
| Default: false
### 14) library
* 설정되면 번들을 라이브러리로 내 보냅니다. output.library는 이름입니다.
* 라이브러리를 작성하고 이를 단일 파일로 공개하려는 경우 이 옵션을 사용 하십시오.
### 15) libraryTarget
라이브러리를 내보낼 형식에 관련된 것입니다.
* `"var"` - 변수를 설정하여 내보내기 : var Library = xxx (기본)
* `"this"` - this를 설정하여 내보내기 : this["Library"] = xxx
* `"commonjs"` - commonjs 속성을 사용하여 내보내기 : exports["Library"] = xxx
* `"commonjs2"`  - module.exports를 설정하여 내보내기 : module.exports = xxx
* `"amd"` - AMD로 내보내기 (옵션으로 설정 - 라이브러리 옵션을 통해 이름 설정)
* `"umd"` - AMD, CommonJS2로 내보내기 또는 루트의 등록 정보로 내보내기
| Default : "var"
`output.library`가 설정되어 있지 않지만 `output.libraryTarget`이 var 이외의 값으로 설정된 경우 내보낸 객체의 모든 속성이 복사됩니다. (예외 and, commonjs2 및 umd)
### 16) umdNameDefine
`output.libraryTarget`이 `umd`로 설정되고, `output.library`가 설정된 경우 이 값을 true로 설정하면 AMD 모듈의 이름이 지정됩니다.
### 17) sourcePrefix
번들내의 모든 소스의 행에 이 문자열을 접두사로 붙입니다.
| Default: "\t"
### 18) crossOriginLoading
해당 옵션을 사용하면 chunk의 crossOrigin을 사용할 수 있습니다.
가능한 값은 다음과 같습니다.
* `false` - cross-origin 로드를 비활성화 합니다.
* `anonymous` - cross-origin모드를 사용할 수 있습니다. anonymous로 사용시 credential이 전송되지 않습니다. (인증)
* `use-credentials` - cross-origin이 활성화 되며, 인증이 request객체에 함께 전송됩니다.

## module
일반 모듈에 영향을 주는 옵션입니다.
### 1) loaders
자동 적용되는 로더의 배열입니다.

각 항목은 다음 속성을 가질 수 있습니다.
* `test` : loader를 적용할 조건
* `exclude`: loader 적용을 제외할 조건
* `include`: 가져온 파일이 로더에 의해 변환될 경로 또는 파일의 배열
* `loader`: !문자열의 분리형 로더
* `loaders` : 문자열 로더의 배열

조건은 절대경로에 대해 테스트된 정규식, 절대경로가 포함된 문자열, 함수 (absPath) : bool 또는 `and`와 결합된 이들 중 하나의 배열일 수 있습니다.

`중요`: 해당 모듈에 표기된 로더는 적용되는 리소스와 관련되어 처리됩니다. 이는 구성파일과 관련하여 처리되지 않았음을 의미합니다.
npm에서 로더가 설치되어 있고, node_modules 폴더가 모든 소스파일의 상위폴더에 있지 않는다면, webpack에서 로더를 찾을 수 없습니다. node_modules 폴더를 resolveLoader.root 옵션의 절대 경로로 추가해야 합니다.
```js
resolveLoader: {
	root: path.join(__dirname, 'node_modules')
}
```

Example:
```js
module: {
  rules: [
    {
      // test는 일반적으로 파일 확장명을 일치시키는데 사용됩니다.
      test: /\.jsx$/,

      // include는 일반적으로 디렉토리를 일치시키는데 사용됩니다.
      include: [
        path.resolve(__dirname, "app/src"),
        path.resolve(__dirname, "app/test")
      ],

      // 예외를 사용하기 위해서는 exclude를 사용해야 합니다.
      // 가능한 경우 include를 사용하는것을 권장합니다.

      // 모듈을 사용할 loader를 정의합니다.
      loader: "babel-loader" // 또는 babel을 사용하면 -loader를 자동으로 추가합니다.
    }
  ]
}
```

### 2) preLoaders, postLoaders
* `module.loaders`와 같은 구문입니다.
* pre loader, post loader의 배열값입니다.
### 3) noParse
* 정규식(RegExp 또는 RegExps) 배열과 일치하는 파일을 파싱하지 않습니다.
* 전체 처리된 요청과 일치합니다.
* 이렇게 설정하면 큰 라이브러리를 무시할때 성능이 향상될 수 있습니다.
* 파일 내에는 require, define 또는 이와 유사한 호출이 없어야 합니다. 해당 모듈들은 export와 module.exports를 사용할 수 있습니다.
### 4) 자동 생성 컨텍스트인 defaults - module.xxxContentXxxx
자동으로 생성되는 컨텍스트의 기본값을 구성하는데에는 여러가지 옵션이 있습니다. 아래와 같이 자동으로 생성된 세가지 유형의 컨텍스트를 차별화 합니다.
* `exprContext`: 의존 관계로서의 표현 (i.e., require(expr))
* `wrappedContext`: 표현식에 접두사와 / 또는 접미사가 붙은 문자열 (i.e. require("./templates/" + expr))
* `unknownContext`: require의 해석이 불가능한 다른 사용법 (i.e. require)

자동으로 생성되는 컨텍스트에는 아래와 같이 네가지 옵션이 가능합니다.
* requires: 컨텍스트에 대한 요청
* recursive: 하위 디렉토리를 순회
* regExp: 정규표현식
* critical: dependency에서 중요하게 고려되는 유형 (경로를 내보냅니다.)

모든 옵션과 default:
`unknownContextRequest = "."`, `unknownContextRecursive = true`, `unknownContextRegExp = /^\.\/.*$/`, `unknownContextCritical = true`

`wrappedContextRegExp = /.*/`, `wrappedContextRecursive = true`, `wrappedContextCritical = false`

참고: `module.wrappedContextRegExp`는 전체 정규식 가운데 부분만 참조합니다. 나머지는 prefix와 suffix에서 생성됩니다.

Example:
```js
{
  module: {
	// 알수없는 요청에대한 처리를 하지 않습니다.
	unknownContextRegExp: /$^/,
	unknownContextCritical: false,

	// 단일 표현식을 사용하여 요청에 대한 처리를 하지 않습니다.
	exprContextRegExp: /$^/,
	exprContextCritical: false,

	// require되는 모든 표현에 대해 경고를 출력합니다.
	wrappedContextCritical: true
  }
}
```


## resolve
모듈을 해석하는데 있어 영향을 미치는 옵션입니다.
### 1) alias
모듈을 다른 모듈이나 경로로 교체합니다.

key가 모듈이름인 객체를 만듭니다. value는 새 경로입니다. 대체와 비슷하나 더 영리하게 처리합니다. 키가 $로 끝나면 $가 없는 정확한 일치값만 교체합니다.

값이 상대 경로이면 require를 포함하는 파일에 상대적으로 적용됩니다.
Examples: 다른 별칭을 설정하여 /abc/entry.js에서 require를 호출함.

| alias: | require("xyz") | require("xyzzy/file.js") |
| --- | --- | --- |
| {} |	/abc/node_modules/xyz/index.js | /abc/node_modules/xyz/file.js |
| { xyz: "/absolute/path/to/file.js" } |	/absolute/path/to/file.js | error |
| { xyz$: "/absolute/path/to/file.js" } | /absolute/path/to/file.js |	/abc/node_modules/xyz/file.js |
| { xyz: "./dir/file.js" } | /abc/dir/file.js | error |
| { xyz$: "./dir/file.js" } |  /abc/dir/file.js | /abc/node_modules/xyz/file.js |
| { xyz: "/some/dir" } | /some/dir/index.js |  /some/dir/file.js |
| { xyz$: "/some/dir" } |  /some/dir/index.js | /abc/node_modules/xyz/file.js |
| { xyz: "./dir" } | /abc/dir/index.js | /abc/dir/file.js |
| { xyz: "modu" } | /abc/node_modules/modu/index.js	 | /abc/node_modules/modu/file.js |
| { xyz$: "modu" } | /abc/node_modules/modu/index.js | /abc/node_modules/xyz/file.js |
| { xyz: "modu/some/file.js" } | /abc/node_modules/modu/some/file.js |	 error |
|{ xyz: "modu/dir" } | /abc/node_modules/modu/dir/index.js | /abc/node_modules/dir/file.js |
| { xyz: "xyz/dir" } | /abc/node_modules/xyz/dir/index.js | /abc/node_modules/xyz/dir/file.js |
| { xyz$: "xyz/dir" }	 | /abc/node_modules/xyz/dir/index.js | /abc/node_modules/xyz/file.js |

`index.js`는 `package.json`에 정의된 경우 다른 파일로 해석될 수 있습니다.

`/abc/node_modules`도 `/node_modules`에서 확인할 수 있습니다.

### 2) root
모듈을 포함하는 절대경로의 디렉토리 입니다. 해당 값은 디렉토리의 배열일 수 있습니다. 이 설정은 검색 경로에 디렉토리를 추가하는데 사용되어야 합니다.
참고) **절대 경로만 사용하셔야 합니다!** 상대경로로 사용하지 마십시오

Example:
```js
var path = require('path');

// ...
resolve: {
  root: [
    path.resolve('./app/modules'),
    path.resolve('./vendor/modules')
  ]
}
```

### 3) moduleDirectories
현재 디렉토리 뿐만 아니라 그 상위의 디렉토리로 해석되고 모듈을 검색할 디렉토리 이름의 배열입니다. 이것은 node에서 `node_modules` 디렉토리를 찾는것과 유사하게 기능합니다. 예를들어 값이 ["mydir"] 이면, webpack은 `./mydir`, `../mydir`, `../../mydir` 등을 찾습니다.
| Default: ["web_modules", "node_modules"]
참고: '../someDir'., 'app', '.' 여기서 절대 경로는 필요하지 않습니다. 경로가 아닌 디렉토리 이름만 사용하십시오. 이러한 폴더 내에 계층 구조가 있을 것으로 예상되는 경우에만 사용하십시오. 그렇지 않으면 resolve.root 옵션을 대신할 수 있을 것입니다.
### 4) fallback
webpack이 `resolve.root` 또는 `resolve.modulesDirectories`에는 없는 모듀을 찾아야만 하는 디렉토리 (또는 디렉토리의 절대 경로가 포함된 배열)
### 5) extensions
모듈을 처리하는데 사용해야 하는 확장에 관련된 배열입니다. 예를들어 CoffeeScript 파일을 찾으려면, 배열에 ".coffee" 문자열이 포함되어 있어야 합니다.
| Default: ["", ".webpack.js", ".web.js", ".js", ".json"]

`중요` : 이 옵션을 설정하면 기본값보다 우선적으로 해당 옵션이 적용됩니다. 즉, webpack은 기본 확장명을 사용하여 모듈을 더이상 처리하지 않습니다.
확장명이 필요한 모듈 (예를들면 require('./somefile.ext'))을 올바르게 처리하려면 배열에 빈 문자열을 포함해야 합니다.

마찬가지로 확장자가 필요없는 모듈 (예를들면 require('underscore'))을 확장자가 .js인 파일로 처리하려면 배열에 `.js`를 포함해야 합니다.
### 6) packageMains
`package.json`의 적절한 파일을 해당 필드에서 찾습니다.
| Default: ["web pack", "browser", "web", "browserify", ["jam". "main"], "main"]
참고: 해당 옵션은 webpack2의 resolve.mainFields로 변경되었습니다.
### 7) packageAlias
`package.json`에서 객체를 확인합니다. 키-값의 쌍은 사양에 따라 별칭으로 처리됩니다.
Example: `"browser"`는 browser필드에서 체크합니다.
### 8) unsafeCache
파일의 일부를 처리하기 위해 공격적이지만 안전하지 않은 캐싱방법을 사용합니다. 캐시된 경로를 변경하면 오류가 발생할 수 있습니다.  (이러한 경우는 드물게 발생합니다.) RegExps의 배열은 RegExp 또는 true (모든 파일)만 예상됩니다. 해결된 경로가 일치하면 캐시됩니다.
| Default: []
## resolveLoader
`resolve`와 유사한 loader입니다.
```js
// Default:
{
	modulesDirectories: ["web_loaders", "web_modules", "node_loaders", "node_modules"],
	extensions: ["", ".webpack-loader.js", ".web-loader.js", ".loader.js", ".js"],
	packageMains: ["webpackLoader", "webLoader", "loader", "main"]
}
```
resolveLoader에 `alias`를 사용하고 다른 기능을 사용할 수 있습니다. 예를 들어, `{txt: 'raw-loader'}`는 raw-loader를 사용하기 위해 `txt!templates/demo.tx`를 shim 합니다.
### 1) moduleTemplates
이 옵션은 resolveLoader의 고유 속성입니다. 처리하기 위한 모듈 이름의 대안을 설명합니다.
| Default: ["x-webpack-loader", "x-web-loader", "x-loader", "*"]

## externals
webpack에의해 번들되어서는 안되지만, 대신 번들 결과에 의해 요청된 종속성을 나타냅니다. 요청된 기술은 `output.libraryTarget`에 의해서 지정된대로 또는 key-value 쌍으로 된 객체 정의를 사용하여 종속성별로 수정됩니다.

결과적으로 externals의 값은 문자열, 객체, 함수, RegExp 또는 배열일 수 있습니다.
* string: 정확히 일치하는 의존성 요청을 output.libraryTarget이 규정한 기술로 변환합니다.
* object: 개별 종속성의 처리를 설명하는 사전 object를 제공합니다. key는 연관된 종속성 요청과 정확하게 일치해야 하며, externalization 요청을 무시하고 어떻게 되던 종속성을 연결하려면 해당값이 false가 되어야 합니다. output.libraryTarget에 지정된 기술을 사용하여 종속성을 연결하는 경우 값은 true입니다. 또는 outputlibraryTarget 당 요청 기법을 참조하는 두개의 공백으로 구분된 이름을 포함하는 문자열 및 연관된 키와 다를 수 있는 종속성을 참조합니다.
* function: function (context, request, callback (err, result))으로 이루어진 이 함수는 각 의존성에 대해 호출되게 됩니다. 결과가 콜백함수 (callback(에 전달되면 이 값은 객체의 속성값처럼 처리가 됩니다.
* RegExp: 일치되는 모든 종속성 모듈은 외부처리 됩니다. 일치하는 텍스트들은 외부 종속성에 대한 요청으로 사용됩니다. 요청은 외부 코드의 Hook을 생성하는데 사용된 정확한 코드이므로, commonjs 패키지 (예: '.../some/package.json')와 일치하는 경우 함수 대신 외부화 전략을 사용하는 것이 좋습니다.  그런 다음 require('" + 요청 + "') 선언 후, module.exports = require('.../some/package.js') 및 콜백(null, require)를 사용하여 웹팩 컨텍스트내의 패키지를 가져올 수 있습니다.
* array: 복수의 schema에 대한 value 입니다 (재귀적임)

Example:
```js
{
	output: { libraryTarget: "commonjs" },
	externals: [
		{
			a: false, // a는 external이 아님
			b: true, // b는 외부처리됨
			"./c": "c", // "./c"는 외부처리됨 `module.exports = c`
			"./d": "var d", // "./d"는 외부처리됨 `module.exports = ./d`  <-- syntax에러 발생
                        "./f": "commonjs2 ./a/b", // "./f" 외부처리됨 `module.exports = require("./a/b")`
                        "./f": "commonjs ./a/b", // ...commonjs2와 유사함
                        "./f": "this ./a/b" // "./f" 외부처리됨 `(function() { module.exports = this["./a/b"]; }())`
		},
		// 모든 관련없는 모듈은 외부 모듈 처리됩니다.
		// abc -> require("abc")
		/^[a-z\-0-9]+$/,
		function(context, request, callback) {
			// 접두어가 global-인 모든 모듈은 외부처리됨
			// "global-abc" -> abc
			if(/^global-/.test(request))
				return callback(null, "var " + request.substr(7));
			callback();
		},
		"./e" // "./e" 는 외부처리됨 (require("./e"))
	]
}
```

| type | value | import code 결과 |
| --- | --- | --- |
| "var" | "abc" | module.exports = abc; |
| "var" | "abc.def" | module.exports = abc.def; |
| "this" | "abc" | (function () { module.exports = this["abc"]}) |
| "this" | ["abc", "def"] | (function() { module.exports = this["abc"]["def"];})()); |
| "commonjs" | ["abc", "def"] | module..exports = require("abc").def; |
| "amd" | "abc" | define(["abc"], function (x) { module.exports = X; }) |
| "amd" | "abc" | amd에 모든것 해당 |

외부 값으로 amd 또는 umd를 적용하면 amd 또는 umd 대상으로 컴파일하지 않으면 중단됩니다.

## target
* `web` : web 브라우저 환경에서 사용하기 위해 컴파일합니다. (기본값)
* `webworker`: webworker로 컴파일합니다.
* `node`: node.js와 같은 환경에서 사용하기 위해 컴파일 합니다. (chunk load에 require 사용)
* `async-node`: node.js와 같은 환경에서 사용하기 위해 컴파일 합니다. (fs 및 VM을 사용하여 청크를 비동기적으로 로드합니다.)
* `node-webkit`: node webkit에서 사용하기 위해 컴파일 하고, jsonp 청크 로딩을 사용하지만 node.js 모듈에서 빌드를 지원합니다. (require("nw.gui")(experimental))
* `electron`: Electron에서 사용하기 위해 컴파일합니다. (node-webkit 특성 모듈을 요구합니다.)
* `electron-renderer`: electron 렌더러 프로세스를 컴파일하고, jsonpTemplatePlugin을 사용하여 타겟을 제공하고, 브라우저 환경을 위해 FunctionModulePlugin을 구현합니다. 또한 commonjs및 built-in 모듈을 위해 NodeTargetPlugin 및 ExternalsPlugin을 제공합니다. (참고: web pack >= 1.12.15가 필요합니다.)

## bail
첫번째 오류를 허용합니다. 대신에 하드한 오류로 보고합니다.

## profile
각 모듈에 대한 타이밍 정보를 캡쳐합니다.
| 힌트 : analyze tool을 사용하여 시각화 하십시오. --json 또는 stats.toJson()은 JSON으로 시각화 통계를 제공합니다.

## cache
* 빌드의 성능을 향상시키기 위해서 모듈 및 청크를 생성합니다.
* 해당 옵션은 watch 모드에서 기본적으로 활성화 됩니다.
* false를 설정하여 해당 옵션을 비활성화 할 수 있습니다.
* 객체를 전달하여 cache를 활성화하고 webpack이 전달된 객체를 캐시로 사용하게 할 수 있습니다. 이렇게 하면 여러 컴파일러 호출간에 캐시 객체를 공유할 수 있습니다.

## debug
* loader들을 debug mode로 전환합니다.

## watch
file 변화를 감지합니다. 해당 옵션은 webpack-stream과 함께 사용할 수 있습니다.

## devtool
디버깅을 활성화 하는 개발자 도구를 선택합니다.

* `eval` : 각 모듈은 eval 및 @sourceURL과 함께 실행됩니다.
* `source-map` : sourcemap이 output에 출력됩니다. output.sourceMapFilename을 참조하십시오
* `hidden-source-map` : `source-map`과 같지만 번들에 참조 주석을 추가하지 않습니다.
* `inline-source-map` : sourcemap은 javascript 파일에 DataUrl로 추가됩니다.
* `eval-source-map` : 각 모듈은 eval과 함께 실행되고 SourceMap은 DataUrl로 eval에 추가됩니다.
* `cheap-source-map` : 열 매핑이 없는 SourceMap 입니다. 로더의 SourceMaps는 사용되지 않습니다.
* `cheap-module-source-map` : 열 매핑이 없는 SourceMap 입니다. 로더의 SourceMaps는 한줄에 하나의 매핑으로 단순화 됩니다.

접두사(prefix) `@`, `#` 또는  `#@` 는 pragma 스타일을 적용합니다. (기본값은 webpack 1에서는 @, webpack 2에서는 # 이며, # 사용이 권장됩니다.)

조합이 가능합니다. 숨겨진 인라인, eval 및 pragma 스타일은 독점적입니다.
| devtool | build speed | rebuild speed | production supported | quality |
| --- | --- | --- | --- | --- |
| eval | +++ | +++ | 지원안함 | 생성된 코드 |
| cheap-eval-source-map | + | ++ | 지원안함 | 변환된 코드 (줄만) |
| cheap-source-map | + | o | 지원 | 변환된 코드 (줄만) |
| cheap-module-eval-source-map | o | ++ | 지원안함 | 원본소스 (줄만) |
|cheap-module-source-map | o | - | 지원 | 원본소스 (줄만) |
| eval-source-map | -- | + | 지원안함 | 원본소스 |
| source-map | -- | -- | 지원함 | 원본소스 |

Example :
```js
{
	devtool: "#inline-source-map"
}
// =>
//# sourceMappingURL=...
```

## devServer
webpack config가 webpack-dev-server CLI에 전달될 때 webpack-dev-server의 동작을 구성하는 데 사용할 수 있습니다.

Example:
```js
{
	devServer: {
		contentBase: "./build",
	}
}
```

## node
다양한 Node 항목에 대해 polyfill 이나 정보를 포함합니다.
* `console` : true or false
* `global` : true or false
* `process` : true, 'mock' or false
* `Buffer` : true or false
* `__filename` : true (컨텍스트 옵션과 관련된 실제 파일 이름) or 'mock' (index.js) 또는 false (일반 node `__dirname`)
* `__dirname` : true (컨텍스트 옵션과 관련된 실제 dirname), 'mock' (/) 또는 false (일반 node.js `__dirname` )
* <node buildin> : true, 'mock', 'empty' or false
```js
// Default:
{
	console: false,
	global: true,
	process: true,
	Buffer: true,
	__filename: "mock",
	__dirname: "mock",
	setImmediate: true
}
```

## amd
require.amd 및 define.amd의 값을 설정합니다.
예) amd: {jQuery: true} (jquery의 1.x amd 버전)

## loader
loader 콘텍스트 내의 커스텀 값

## recordsPath, recordsInputPath, recordsOutputPath
json 파일에서 /로 컴파일러 상태를 저장 또는 로드합니다. 이로인해 모듈과 청크 id가 영구적으로 유지됩니다.

해당 옵션은 절대 경로가 필요합니다. recordsPath는 recordsInputPath 및 recordsOutputPath에 대해 정의되지 않은 상태로 사용됩니다.

이는 컴파일러에 대한 여러 호출간에 핫코드 대체를 사용할 때 필요합니다.