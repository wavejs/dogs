# Visual Studio Code - Tips

## Table of Contents

* [Mac에서 터미널 code 명령어 Path 설정하기](#mac에서-터미널-code-명령어-path-설정하기)
* [줄 끝 공백 제거 기능을 Markdown 파일은 제외하기](#줄-끝-공백-제거-기능을-markdown-파일은-제외하기)

---

### Mac에서 터미널 code 명령어 Path 설정하기

참고: [Running VS Code on Mac](https://code.visualstudio.com/docs/setup/mac)

아래는 `heredoc`을 활용하여 마지막 라인에 추가하는 방법입니다.

```bash
cat << EOF >> ~/.bash_profile

# Visual Studio Code
export PATH="\$PATH:/Applications/Visual Studio Code.app/Contents/Resources/app/bin/"
EOF
```

[VS Code](https://code.visualstudio.com/docs/setup/mac) 문서에 나와있는 수동 추가 방법으로 그대로 사용하면 `$PATH` 부분의 변수값이 출력되어 원하지 않는 결과가 삽입될 수 있습니다.

그래서 `$PATH`앞에 `\`를 추가합니다.

### 줄 끝 공백 제거 기능을 Markdown 파일은 제외하기

사용자 설정에서 아래와 같이 Markdown에 대한 설정을 별도로 지정합니다.

```json
"files.trimTrailingWhitespace": true,
"[markdown]": {
    "files.trimTrailingWhitespace": false
},
```
