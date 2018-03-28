# npm ci

5.7.0 부터 npm ci 커맨드를 사용할 수 있게 되었습니다. 보통, ci라 함은 Contiuouse Integration (지속적인 통합) 의 개념 으로써, 오픈소스 개발시에 많은 분들이 travis ci를 사용하시는 것을 볼 수 있습니다. (Jenkins와 유사하다고 볼 수 있겠네요) 아직 많은 기능은 탑재하지 못하더라도, npm ci를 통해서 협업이 많은 개발자들이 개발환경을 (pakcage에 대한) 지속적으로 통합할 수 있도록 하는 커맨드라고 볼 수 있겠습니다.
----

## 장점
### 속도

npm blog에서는 `npm ci`의 장점을 빠른 설치 속도라고 이야기 하고 있습니다. `npm install`과 커맨드가 유사하나, npm install을 사용하는 것보다 두배정도가 빠르며, 이를통해 CI/CD가 잦은 조직에서 중요한 성능 향상을 나타냅니다.

### 안정성

npm install 커맨드는 `package.json`내의 dependencies와 devDependencies를 기준으로 패키지 파일을 설치하게 되어 있습니다. 이에 반해, npm ci는 package.json보다 `package-lock.json`이 우선합니다. package-lock.json 등의 lockfile을 기준으로 package를 설치하게 되어 있으므로, 규모가 큰 조직에서 package에 대한 lockfile이 승인되면, npm ci를 활용하여 package-lock.json에 명시되어 있는 패키지를 설치하도록 합니다.

만약, package.json내의 파일과 package-lock.json 내의 버전 등이 다르면, package-lock.json을 기준으로 package.json 파일을 수정하며, 명시되지 않은 부분에서는 오류를 발생시키므로, Application 관리에 있어서 안정성을 확보할 수 있다고 이야기 하고 있습니다.

---
## 설치
최신의 npm 으로 설치합니다. (현재 5.8.0까지 release 되어 있습니다.)
```bash
npm install -g npm@latest
```

### 실행

커맨드는 간단합니다. package-lock.json이 생성되어 있는 project내에서 npm ci 커맨드를 입력하면 됩니다.
```bash
npm ci
```

만약 현재 개발하고 계신 환경에서 production 레벨까지만 package를 설치하고 싶으시면, npm install과 같이 `--only=production`으로 설치하시면 됩니다.

```bash
npm ci --only=production
```

ci command를 실행하면, 모두 기본적으로 npm을 통해 설치된 node_modules 폴더를 삭제하며, package-lock.json 기준으로 package를 다시 설치합니다.

---

npm ci는 travis ci와도 같이 사용이 가능합니다. npm package를 통한 Application 통합이 잦은 프론트엔드 환경에서 퍼포먼스 또는 안정성에서 큰 이득을 얻을 수 있지 않을까 생각합니다.