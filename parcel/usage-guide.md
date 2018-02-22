# parcel-bundler

webpack의 대항마로 떠오른 parcel-bundler 사용 가이드 입니다.

## Getting Started

### 1) 설치하기

parcel-bundler를 npm을 통해 설치합니다. (현재까지는 global로 설치해야 정상적으로 동작하는 것으로 보입니다.) - 현재까지 1.6.2 버전이 최신 버전입니다.

```bash
npm install -g parcel-bundler
```

1.6.2 버전까지는 typescript, sourcemap을 기본으로 지원합니다.

### 2) 시작하기
시작하는 방법은 command 또는 package.json의 script 설정을 통해 가능합니다.

```bash
# bash command로 실행합니다.
# dev mode (watch server가 실행됩니다.)
parcel watch ./index.html
parcel ./index.html
# prod mode
parcel build ./index.html
```

```json
// package.json으로 실행합니다.
{
  "scripts" : {
    "dev": "parcel watch ./index.html",
    "prod": "parcel build ./index.html"
  }
}
```

## parcel command
parcel document에서 정보가 많이 나오지 않아, command상에 표시된 내용으로 정리하였습니다.

### 1) port

parcel이 watch 상태일때 설정할 parcel server의 port 번호입니다. default는 1234 입니다.

```bash
# 3000 포트를 개발 서버로 사용합니다.
parcel -p 3000 ./index.html
parcel --port 3000 ./index.html
```

### 2) hmr-port

parcel은 기본적으로 watch server에서 HotModuleReplacement를 지원합니다. hmr-port는 HotModuleReplacement를 지원하기 위한 socket의 포트 번호를 설정할 수 있습니다. default는 random 입니다.

```bash
# socket port를 3000으로 지정합니다.
parcel --hmr-port 3000 ./index.html
```

### 3) hmr-hostname

HotModuleRepleacement를 지원하기 위한 socket server의 host name을 지정합니다. default는 localhost 입니다.

```bash
# socket server host name을 www.test.com으로 지정합니다.
parcel --hmr-hostname http://www.test.com ./index.html
```

### 4) https

https를 지원합니다.

```bash
# build된 파일이 https를 지원하도록 설정합니다.
parcel --https ./index.html
```

### 5) open

Asset들의 번들링이 끝나고난 뒤 자동으로 OS에 설정된 default browser를 엽니다.

```bash
# 자동으로 browser를 엽니다.
parcel ./index.html --open
```

### 6) out-dir

Asset들이 번들링되고 난 후 저장될 path 입니다. 번들링 될 위치를 지정할때 사용합니다.

기본적으로 parcel에서는 실행되는 현재 폴더의 dist 폴더에 번들링된 js, css 등의 Asset들을 위치시킵니다.

```bash
# js, css 등이 ./dist에 배치됩니다.
parcel ./index.html
# js, css 등이 ./build에 위치됩니다.
parcel ./index.html --out-dir ./build
parcel ./index.html -d ./build
```

### 7) out-file

번들링이 끝나고 난뒤의 html의 file 이름을 지정해줍니다.

기본적으로는 번들링 될 때의 파일이름을 기준으로 합니다.

```bash
# index.html로 번들링됩니다.
parcel ./index.html
# main.html으로 번들링됩니다.
parcel ./index.html --out-file main
parcel ./index.html -o main
```

### 8) public-url

번들링 후에 import할 공용 파일 url을 지정합니다.

기본적으로는 out-dir에 위치된 파일 위치를 이용하여 빌드하도록 되어 있습니다.

```bash
# dist에 js, css 들이 들어가 있으며, index.html에서 dist를 경로로 import 합니다.
parcel ./index.html
# dist/assets에 js, css들이 포함되며, index.html에서 assets/xxx.js 형태로 import 합니다.
parcel ./index.html --public-url assets
```

### 9) no-hmr

개발 진행시에는 자동으로 HotModuleRepleacement 기능이 활성화 되나, 해당 옵션으로 비활성화 할 수 있습니다.

```bash
# hmr을 비활성화 시킵니다.
parcel ./index.html --no-hmr
```

### 10) no-cache

parcel로 번들링을 실행할때, 기본적으로 번들링된 파일(assets) 들이 위치하는 폴더에 .cache 폴더가 생성되며, 해당 폴더에 캐싱된 번들 내역들이 포함되게 됩니다. 해당 옵션을 사용하면, .cache 폴더에 캐싱을 하지 않도록 설정할 수 있습니다.

default는 활성화 상태입니다.

```bash
# caching을 비활성화 합니다.
parcel ./index.html --no-cache
```

### 11) no-source-maps

parcel bundler는 개발모드에서 기본적으로 .map파일인 source map 파일을 제공합니다. 해당 옵션을 통해서 source map 파일 생성을 비활성화 할 수 있습니다.

```bash
# sourcemap을 비활성화 합니다.
parcel ./index.html --no-source-map
```

### 12) target

parcel이 번들링할 target을 지정합니다. target은 browser, node, electron으로 설정할 수 있습니다. 기본값은 browser 입니다.

```bash
# 런타임 환경을 node로 설정합니다.
parcel ./index.html --target node
parcel ./index.html -t node
```

## Tips

### webpack alias처럼 module resolver를 사용하기

parcel을 시작할때 가장 망설여 지던 부분중 하나가 webpack의 resolve alias 기능입니다. 다양한 컴포넌트 또는 폴더를 '../../../'등으로 import 하기에는 경로 설정이 애매해지고, 점점 ../ 부분이 많아지면서 보기가 좋지 않아, alias 등을 통해 설정을 해줘서 사용하는데, parcel에는 해당 기능을 아직까지는 지원하고 있지 않습니다.

typescript는 tsconfig.json에 path 설정을 통해서 해당 resolver를 지원하나, javascript는 따로 번들링 설정에서 해주어야 합니다. 이를 해결하기 위해서, babel의 plugin인 babel-plugin-module-resolver를 사용하면, parcel에서도 alias 설정을 통해 해결할 수 있습니다.

#### 1) 설치

npm을 사용하여 설치합니다.

```bash
# npm
npm install babel-plugin-module-resolver --dev
# yarn
yarn add babel-plugin-module-resolver --dev
```

#### 2) 사용

.babelrc에 해당 plugin을 통한 alias 설정을 추가합니다.

```json
// .babelrc
{
  "plugins" : [
    [
      "module-resolver",
      {
        // root 폴더 설정
        "root": "./src",
        "alias": {
          "common": "./src/common",
          "test": "./src/test"
        }
      }      
    ]
  ]
}
```

해당 설정 후에 parcel 번들링을 실행하면, 정상적으로 bundling이 되는 것을 확인하실 수 있습니다.
```bash
parcel ./index.html
```

### 개발모드 (dev mode) & 배포모드 (prod mode)

개발모드는 다음과 같은 command를 사용합니다.

```bash
parcel ./index.html
#or
parcel watch ./index.html
```

해당 command를 실행시 자동으로 sourcemap(.map) 파일이 생성되며, hmr 기능이 활성화 됩니다.

배포모드는 build를 붙여 command를 실행합니다.
```bash
parcel build ./index.html
```

build를 붙이면 .map 파일이 생성되지 않으며, hmr 기능이 비활성화 됩니다.