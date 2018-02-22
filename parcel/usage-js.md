# parcel-bundler using javascript

parcel-bundler는 webpack과 같이 javascript에서도 사용이 가능합니다.
현재 node로 사용 가능한 것은 테스트 해보았으나, 구동시키는 방법은 조금더 확인이 필요할 것 같습니다 :)

## Link documents
* [parcel-bundler use guide](./usage-guide.md)

## Getting Started

### 1) setup

npm으로 global 설치가 아닌, 프로젝트 내 devDependency로 설치합니다.

```bash
npm install parcel-bundler --dev
```

설치만 해주면 우선 시작 가능합니다.

### 2) setting using javascript

setting할 파일에서 parcel-bundler를 import 합니다. (저는 따로 parcel.config.js를 놓았습니다.)

parcel.config.js에서는 parcel에 대한 번들링 설정만 놓고, 번들러의 middleware를 다른 곳에서도 모듈 형태로 사용할 수 있도록 했습니다. (이후 Node.js에서 middleware를 express 형태로 사용이 가능합니다.)

*parcel.config.js*

```js
const Bundler = require('parcel-bundler')
const path = require('path')

// develop, production 모드에 따라서 번들링 설정을 다르게 할 예정입니다.
const isProd = process.env.NODE_ENV === 'production'

const defaultOption = {
  outDir: path.join(__dirname, '..', 'dist'),
  // index.html을 불러오는 경우, publicUrl을 /로 설정해주어야 정상적으로 import가 가능했습니다.
  publicUrl: '/',
  detailReport: true,
  cache: true,
  minify: true,
  sourceMaps: true,
  http: ture
}

// 모드에 따라 옵션을 설정합니다.
let option
if (isProd) {
  option = {
    ...
  }
} else {
  option = {
    ...
  }
}

// 번들러를 선언해줍니다.
const bundler = new Bundler('번들링할 파일 경로', Object.assign({}, defaultOption, option))

module.exports = bundler

```

### 3) Node.js Express에서 사용

express에서 해당 bundler를 가져와 middleware function을 실행합니다.

```js
const express = require('express')
const bundler = require('parcel.config')

const app = express()

app.use(bundler.middleware())
```

Application 로딩시에 bundler에서 파일을 번들링합니다.

### 4) command

앞서 NODE_ENV를 통해서 분기처리를 했기 때문에, command에서 NODE_ENV를 설정해 줍니다.

```bash
# development
NODE_ENV=production & server.js

NODE_ENV=development & server.js
```