# npm-scope

> 번역기의 힘을 빌려 일부 오역이 있을 수 있습니다. :D

원문: https://docs.npmjs.com/misc/scope

## Scoped packages

### Description

모든 npm 패키지에는 이름이 있습니다. 일부 패키지 이름에는 범위가 있습니다. 범위는 패키지 이름 (URL 안전 문자, 선행 점 또는 밑줄 없음)에 대한 일반적인 규칙을 따릅니다. 패키지 이름에 사용되면, 범위 앞에 @기호가 오고 그 뒤에 슬래시가 옵니다. 예를 들면

```
@somescope/somepackagename
```

범위는 관련 패키지를 그룹화하는 방법이며 npm이 패키지를 처리하는 방법에 영향을 미칩니다.

각 npm 사용자/조직은 자체 범위를 가지며 범위에서 패키지를 추가 할 수 있습니다. 이것은 당신이 당신 앞에서 당신의 패키지 이름을 가져가는 것에 대해 걱정할 필요가 없다는 것을 의미합니다. 따라서 조직을 위한 공식 패키지를 알리는 좋은 방법이기도 합니다.

범위가 지정된 패키지는 npm@2 기본 npm 레지스트리에서 게시 및 설치되고 기본 npm 레지스트리에서 지원됩니다. 범위가 지정되지 않은 패키지는 범위가 지정된 패키지에 종속적 일 수 있으며 그 반대의 경우도 마찬가지입니다. npm 클라이언트는 범위가 지정되지 않은 레지스트리와 역 호환되므로 범위가 지정된 것과 지정되지 않은 레지스트리를 동시에 사용하는 데 사용할 수 있습니다.

### Installing scoped packages

범위가 지정된 패키지는 일반 설치 폴더의 하위 폴더에 설치됩니다. 예를 들어 다른 패키지가 `node_modules/packagename`에 설치되어 있으면 범위가 지정된 모듈이 `node_modules/@myorg/packagename`에 설치됩니다. 범위 폴더 (@myorg)는 단순히 @기호 앞에 오는 범위의 이름이며 범위가 지정된 패키지를 원하는 개수 만큼 포함 할 수 있습니다.

범위가 지정된 패키지는 이름 앞에 @기호를 앞에 붙여서 설치됩니다.

**npm install:**

```
npm install @myorg/mypackage
```

Or in ***package.json:***

```
"dependencies": {
  "@myorg/mypackage": "^1.3.0"
}
```

@ 기호가 생략 된 두 경우 모두 npm은 대신 GitHub에서 설치를 시도합니다. [npm-install](https://docs.npmjs.com/cli/install)을 참고 하십시오.

### Requiring scoped packages

범위가 지정된 패키지는 범위 폴더에 설치되므로 범위에 코드가 필요할 때 범위의 이름을 포함시켜야합니다.

예를 들면

```
require('@myorg/mypackage')
```

노드가 범위 폴더를 처리하는 방법에 특별한 것은 없습니다. 이렇게 하려면 @myorg라는 폴더에 mypackage 모듈이 있어야합니다.

### Publishing scoped packages

범위 패키지는 npm@2의 CLI를 통해 게시 할 수 있으며 기본 npm 레지스트리를 포함하여 지원되는 모든 레지스트리에 게시 할 수 있습니다.

(2015-04-19 현재 및 npm 2.0 이상인 경우 기본 npm 레지스트리는 범위가 지정된 패키지를 지원합니다.)

원하는 경우 범위를 레지스트리와 연결할 수 있습니다. 아래를 보십시오.

#### Publishing public scoped packages to the primary npm registry

공용 범위 패키지를 게시하려면 초기 공개와 함께 `--access public`을 지정해야합니다. 이렇게하면 게시 한 후 `npm access public`을 실행 한 것처럼 패키지를 게시하고 공개 액세스를 설정합니다.

#### Publishing private scoped packages to the npm registry

개인 범위 패키지를 npm 레지스트리에 게시하려면 npm [개인 모듈](https://www.npmjs.com/private-modules) 계정이 있어야합니다.

그런 다음 `npm publish` 또는 `npm publish --access restricted`로 모듈을 게시 할 수 있으며 npm 레지스트리에 액세스가 제한되어 있습니다. 원하는 경우 npm 액세스 또는 npmjs.com 웹 사이트에서 액세스 권한을 변경할 수 있습니다.

### Associating a scope with a registry

범위는 별도의 레지스트리와 연결 할 수 있습니다. 이를 통해 기본 npm 레지스트리의 패키지와 npm Enterprise와 같은 하나 이상의 개인 레지스트리를 혼합하여 사용할 수 있습니다.

로그인 시 스코프를 레지스트리와 연결 할 수 있습니다.

예를 들어

```
npm login --registry=http://reg.example.com --scope=@myco
```

범위는 레지스트리와 다 대 일 관계가 있습니다. 하나의 레지스트리는 여러 범위를 호스트 할 수 있지만 하나의 범위는 하나의 레지스트리를 가리 킵니다.

`npm config`를 사용하여 레지스트리와 범위를 연결할 수도 있습니다.

```
npm config set @myco:registry http://reg.example.com
```

범위가 레지스트리와 연관되면 해당 범위를 가진 패키지에 대한 `npm install`은 해당 레지스트리에서 패키지를 대신 요청합니다. 범위가 포함 된 패키지 이름에 대한 `npm publish`는 해당 레지스트리에 대신 게시됩니다.
