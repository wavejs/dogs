# Visual Studio Code - Shortcuts

> VS Code의 키 바인딩 문서를 번역한 글입니다.  
> 일부 오역이 있을 수 있습니다.

* [Key Bindings](https://code.visualstudio.com/docs/getstarted/keybindings)

## Basic Editing

| Key | Command | Command id |
| --- | --- | --- |
| ⌘X | 절단 선 (빈 선택) | `editor.action.clipboardCutAction` |
| ⌘C | 행 복사 (빈 선택) | `editor.action.clipboardCopyAction` |
| ⇧⌘K | 행 삭제 | `editor.action.deleteLines` |
| ⌘Enter | 아래에 선 삽입 | `editor.action.insertLineAfter` |
| ⇧⌘Enter | 위의 선 삽입 | `editor.action.insertLineBefore` |
| ⌥↓ | 줄을 아래로 이동 | `editor.action.moveLinesDownAction` |
| ⌥↑ | 라인 이동 | `editor.action.moveLinesUpAction` |
| ⇧⌥↓ | 줄 바꿈 복사 | `editor.action.copyLinesDownAction` |
| ⇧⌥↑ | 라인 복사 | `editor.action.copyLinesUpAction` |
| ⌘D | 다음 찾기 일치에 선택 추가 | `editor.action.addSelectionToNextFindMatch` |
| ⌘K ⌘D | 마지막 선택 항목을 다음 찾기 항목으로 이동 | `editor.action.moveSelectionToNextFindMatch` |
| ⌘U | 마지막 커서 작업 실행 취소 | `cursorUndo` |
| ⇧⌥I | 선택한 각 줄 끝의 커서 삽입 | `editor.action.insertCursorAtEndOfEachLineSelected` |
| ⇧⌘L | 현재 선택 항목을 모두 선택하십시오. | `editor.action.selectHighlights` |
| ⌘F2 | 현재 단어의 모든 항목 선택 | `editor.action.changeAll` |
| ⌘I | 현재 행 선택 | `expandLineSelection` |
| ⌥⌘↓ | 커서를 아래에 삽입하십시오. | `editor.action.insertCursorBelow` |
| ⌥⌘↑ | 위의 커서 삽입 | `editor.action.insertCursorAbove` |
| ⇧⌘\ | 일치하는 대괄호로 건너 뛰기 | `editor.action.jumpToBracket` |
| ⌘] | 들여 쓰기 | `editor.action.indentLines` |
| ⌘[ | 내어 쓰기 | `editor.action.outdentLines` |
| Home | 라인의 시작으로 이동 | `cursorHome` |
| End | 행의 끝으로 이동 | `cursorEnd` |
| ⌘↓ | 파일 끝으로 이동 | `cursorBottom` |
| ⌘↑ | 파일의 시작으로 이동 | `cursorTop` |
| ^PageDown | 스크롤 다운 | `scrollLineDown` |
| ^PageUp | 스크롤 라인 업 | `scrollLineUp` |
| ⌘PageDown | 아래로 페이지 스크롤 | `scrollPageDown` |
| ⌘PageUp | 페이지 위로 스크롤 | `scrollPageUp` |
| ⌥⌘[ | 접기 (접기) 영역 | `editor.fold` |
| ⌥⌘] | 펼치기 (펼친 곳) 영역 | `editor.unfold` |
| ⌘K ⌘[ | 모든 하위 지역 접기 (접기) | `editor.foldRecursively` |
| ⌘K ⌘] | 모든 소구역 펼치기 (펼치기 해제) | `editor.unfoldRecursively` |
| ⌘K ⌘0 | 모든 지역 접기 (접기) | `editor.foldAll` |
| ⌘K ⌘J | 모든 지역 펼치기 (펼치기 해제) | `editor.unfoldAll` |
| ⌘K ⌘C | 줄 주석 추가 | `editor.action.addCommentLine` |
| ⌘K ⌘U | 행 주석 제거 | `editor.action.removeCommentLine` |
| ⌘/ | 토글 라인 코멘트 | `editor.action.commentLine` |
| ⇧⌥A | 차단 댓글 토글 | `editor.action.blockComment` |
| ⌘F | 찾기 | `actions.find` |
| ⌥⌘F | 바꾸기 | `editor.action.startFindReplaceAction` |
| ⌘G | 다음 찾기 | `editor.action.nextMatchFindAction` |
| ⇧⌘G | 이전 찾기 | `editor.action.previousMatchFindAction` |
| ⌥Enter | 모든 일치 항목을 선택하십시오. | `editor.action.selectAllMatches` |
| ⌥⌘C | 대소 문자 찾기를 토글 | `toggleFindCaseSensitive` |
| ⌥⌘R | 정규식 찾기 전환 | `toggleFindRegex` |
| ⌥⌘W | 전체 단어 찾기 토글 | `toggleFindWholeWord` |
| ^⇧M | 포커스 설정을 위해 Tab 키 사용 토글 | `editor.action.toggleTabFocusMode` |
| *unassigned* | 렌더링 공백을 토글합니다. | `toggleRenderWhitespace` |
| ⌥Z | 단어 감싸기 토글 | `editor.action.toggleWordWrap` |

## Rich Languages Editing

| Key | Command | Command id |
| --- | --- | --- |
| ^Space | 트리거 제안 | `editor.action.triggerSuggest` |
| ⇧⌘Space | 트리거 매개 변수 힌트 | `editor.action.triggerParameterHints` |
| ⇧⌥F | 문서 서식 지정 | `editor.action.formatDocument` |
| ⌘K ⌘F | 형식 선택 | `editor.action.formatSelection` |
| F12 | 정의로 이동 | `editor.action.goToDeclaration` |
| ⌘K ⌘I | 호버 표시 | `editor.action.showHover` |
| ⌥F12 | 픽 정의 | `editor.action.previewDeclaration` |
| ⌘K F12 | 측면 정의 열기 | `editor.action.openDeclarationToTheSide` |
| ⌘. | 빠른 수정 | `editor.action.quickFix` |
| ⇧F12 | 참조 표시 | `editor.action.referenceSearch.trigger` |
| F2 | 심볼 이름 바꾸기 | `editor.action.rename` |
| ⇧⌘. | 다음 값으로 바꾸기 | `editor.action.inPlaceReplace.down` |
| ⇧⌘, | 이전 값으로 바꾸기 | `editor.action.inPlaceReplace.up` |
| ^⇧⌘→ | AST 선택 확장 | `editor.action.smartSelect.grow` |
| ^⇧⌘← | 축소 AST 선택 | `editor.action.smartSelect.shrink` |
| ⌘K ⌘X | 트림 공백 제거 | `editor.action.trimTrailingWhitespace` |
| ⌘KM | 언어 모드 변경 | `workbench.action.editor.changeLanguageMode` |

## Navigation

| Key | Command | Command id |
| --- | --- | --- |
| ⌘T | 모든 기호 표시 | `workbench.action.showAllSymbols` |
| ^G | 라인으로 이동 ... | `workbench.action.gotoLine` |
| ⌘P | 파일로 이동 ..., 빠른 열기 | `workbench.action.quickOpen` |
| ⇧⌘O | 기호로 이동 ... | `workbench.action.gotoSymbol` |
| ⇧⌘M | 문제 표시 | `workbench.actions.view.problems` |
| F8 | 다음 오류 또는 경고로 이동 | `editor.action.marker.next` |
| ⇧F8 | 이전 오류 또는 경고로 이동 | `editor.action.marker.prev` |
| ⇧⌘P | 모든 명령 표시 | `workbench.action.showCommands` |
| ^⇧Tab | 편집기 그룹 기록 탐색 | `workbench.action.openPreviousRecentlyUsedEditorInGroup` |
| ^- | 돌아 가기 | `workbench.action.navigateBack` |
| ⌃⇧- | 앞으로 이동 | `workbench.action.navigateForward` |

## Editor/Window Management

| Key | Command | Command id |
| --- | --- | --- |
| ⇧⌘N | 새창 | `workbench.action.newWindow` |
| ⇧⌘W | 창 닫기 | `workbench.action.closeWindow` |
| ⌘W | 편집기 닫기 | `workbench.action.closeActiveEditor` |
| ⌘KF | 폴더 닫기 | `workbench.action.closeFolder` |
| *unassigned* | 편집기 그룹 사이주기 | `workbench.action.navigateEditorGroups` |
| ⌘\ | 스플릿 편집기 | `workbench.action.splitEditor` |
| ⌘1 | 첫 번째 편집기 그룹에 초점 맞추기 | `workbench.action.focusFirstEditorGroup` |
| ⌘2 | 두 번째 편집기 그룹에 초점 맞추기 | `workbench.action.focusSecondEditorGroup` |
| ⌘3 | 세 번째 편집기 그룹에 초점 맞추기 | `workbench.action.focusThirdEditorGroup` |
| ⌘K ⌘← | 왼쪽의 편집기 그룹에 초점 맞추기 | `workbench.action.focusPreviousGroup` |
| ⌘K ⌘→ | 오른쪽 편집기 그룹에 초점 맞추기 | `workbench.action.focusNextGroup` |
| ⌘K ⇧⌘← | 왼쪽으로 편집기 이동 | `workbench.action.moveEditorLeftInGroup` |
| ⌘K ⇧⌘→ | 편집기 오른쪽으로 이동 | `workbench.action.moveEditorRightInGroup` |
| ⌘K ← | 활성 편집기 그룹을 왼쪽으로 이동 | `workbench.action.moveActiveEditorGroupLeft` |
| ⌘K → | 활성 편집기 그룹을 오른쪽으로 이동하십시오. | `workbench.action.moveActiveEditorGroupRight` |
| ^⌘→ | 편집기를 다음 그룹으로 이동 | `workbench.action.moveEditorToNextGroup` |
| ^⌘← | 편집기를 이전 그룹으로 이동 | `workbench.action.moveEditorToPreviousGroup` |

## File Management

| Key | Command | Command id |
| --- | --- | --- |
| ⌘N | 새로운 파일 | `workbench.action.files.newUntitledFile` |
| *unassigned* | 파일 열기 ... | `workbench.action.files.openFile` |
| ⌘S | 저장 | `workbench.action.files.save` |
| ⌥⌘S | 모두 저장 | `workbench.action.files.saveAll` |
| ⇧⌘S | 다른 이름으로 저장... | `workbench.action.files.saveAs` |
| ⌘W | 닫기 | `workbench.action.closeActiveEditor` |
| ⌥⌘T | 기타 닫기 | `workbench.action.closeOtherEditors` |
| ⌘KW | 그룹 닫기 | `workbench.action.closeEditorsInGroup` |
| *unassigned* | 다른 그룹 닫기 | `workbench.action.closeEditorsInOtherGroups` |
| *unassigned* | 왼쪽에서 왼쪽 그룹 닫기 | `workbench.action.closeEditorsToTheLeft` |
| *unassigned* | 그룹을 오른쪽으로 닫기 | `workbench.action.closeEditorsToTheRight` |
| ⌘K ⌘W | 모두 닫기 | `workbench.action.closeAllEditors` |
| ⇧⌘T | 닫힌 편집기 다시 열기 | `workbench.action.reopenClosedEditor` |
| ⌘K Enter | 열기 유지 | `workbench.action.keepEditor` |
| ^Tab | 다음 열기 | `workbench.action.openNextRecentlyUsedEditorInGroup` |
| ^⇧Tab | 이전 열기 | `workbench.action.openPreviousRecentlyUsedEditorInGroup` |
| *unassigned* | 활성 파일의 경로 복사 | `workbench.action.files.copyPathOfActiveFile` |
| *unassigned* | Windows에서 활성 파일 공개 | `workbench.action.files.revealActiveFileInWindows` |
| ⌘KO | 새 창에 열린 파일 표시 | `workbench.action.files.showOpenedFileInNewWindow` |
| *unassigned* | 열린 파일 비교 | `workbench.files.action.compareFileWith` |

## Display

| Key | Command | Command id |
| --- | --- | --- |
| ⌃⌘F | 전체 화면 토글 | `workbench.action.toggleFullScreen` |
| ⌘K Z | 젠 모드 토글 | `workbench.action.toggleZenMode` |
| Escape Escape | 젠 모드에서 나가기 | `workbench.action.exitZenMode` |
| ⌘= | 확대 | `workbench.action.zoomIn` |
| ⌘- | 축소 | `workbench.action.zoomOut` |
| ⌘Numpad0 | 재설정 확대 | `workbench.action.zoomReset` |
| ⌘B | 사이드 바 가시성 토글 | `workbench.action.toggleSidebarVisibility` |
| ⇧⌘E | 탐색기 표시 / 포커스 전환 | `workbench.view.explorer` |
| ⇧⌘F | 검색보기 | `workbench.view.search` |
| ⌃⇧G | 소스 제어 표시 | `workbench.view.scm` |
| ⇧⌘D | 디버그 표시 | `workbench.view.debug` |
| ⇧⌘X | 확장 프로그램 표시 | `workbench.view.extensions` |
| ⇧⌘U | 출력 표시 | `workbench.action.output.toggleOutput` |
| ⌃Q | 빠른 열기 표시 | `workbench.action.quickOpenView` |
| ⇧⌘C | 새 명령 프롬프트 열기 | `workbench.action.terminal.openNativeConsole` |
| ⇧⌘V | 마크다운 미리보기 토글 | `markdown.showPreview` |
| ⌘K V | 측면에 미리보기 열기 | `markdown.showPreviewToSide` |
| ⌃\` | 통합 터미널 토글 | `workbench.action.terminal.toggleTerminal` |

## Search

| Key | Command | Command id |
| --- | --- | --- |
| ⇧⌘F | 검색보기 | `workbench.view.search` |
| ⇧⌘H | 파일에서 바꾸기 | `workbench.action.replaceInFiles` |
| ⌥⌘C | 대 / 소문자 전환 | `toggleSearchCaseSensitive` |
| ⌥⌘W | 전체 단어 맞추기 토글 | `toggleSearchWholeWord` |
| ⌥⌘R | 정규 표현식 사용 토글 | `toggleSearchRegex` |
| ⇧⌘J | 검색 세부 사항 전환 | `workbench.action.search.toggleQueryDetails` |
| F4 | 다음 검색 결과 집중 | `search.action.focusNextSearchResult` |
| ⇧F4 | 초점 이전 검색 결과 | `search.action.focusPreviousSearchResult` |
| ⌥↓ | 다음 검색 용어 표시 | `search.history.showNext` |
| ⌥↑ | 이전 검색 용어 표시 | `search.history.showPrevious` |

## Preferences

| Key | Command | Command id |
| --- | --- | --- |
| ⌘, | 사용자 설정 열기 | `workbench.action.openGlobalSettings` |
| *unassigned* | 작업 공간 설정 열기 | `workbench.action.openWorkspaceSettings` |
| ⌘K ⌘S | 키보드 단축키 열기 | `workbench.action.openGlobalKeybindings` |
| *unassigned* | 사용자 스니펫 열기 | `workbench.action.openSnippets` |
| ⌘K ⌘T | 색상 테마 선택 | `workbench.action.selectTheme` |
| *unassigned* | 디스플레이 언어 구성 | `workbench.action.configureLocale` |

## Debug

| Key | Command | Command id |
| --- | --- | --- |
| F9 | 중단 점 토글 | `editor.debug.action.toggleBreakpoint` |
| F5 | 스타트 | `workbench.action.debug.start` |
| F5 | 잇다 | `workbench.action.debug.continue` |
| ^F5 | 시작 (디버깅하지 않음) | `workbench.action.debug.run` |
| F6 | 중지 | `workbench.action.debug.pause` |
| F11 | 들어가기 | `workbench.action.debug.stepInto` |
| ⇧F11 | 스텝 아웃 | `workbench.action.debug.stepOut` |
| F10 | 스텝 오버 | `workbench.action.debug.stepOver` |
| ⇧F5 | 중지 | `workbench.action.debug.stop` |
| ⌘K ⌘I | 호버 표시 | `editor.debug.action.showDebugHover` |

## Tasks

| Key | Command | Command id |
| --- | --- | --- |
| ⇧⌘B | 빌드 작업 실행 | `workbench.action.tasks.build` |
| *unassigned* | 테스트 작업 실행 | `workbench.action.tasks.test` |

## Extensions

| Key | Command | Command id |
| --- | --- | --- |
| *unassigned* | 확장 프로그램 설치 | `workbench.extensions.action.installExtension` |
| *unassigned* | 설치된 확장 프로그램 표시 | `workbench.extensions.action.showInstalledExtensions` |
| *unassigned* | 오래된 확장명 표시 | `workbench.extensions.action.listOutdatedExtensions` |
| *unassigned* | 추천 확장 프로그램보기 | `workbench.extensions.action.showRecommendedExtensions` |
| *unassigned* | 인기 확장 프로그램 표시 | `workbench.extensions.action.showPopularExtensions` |
| *unassigned* | 모든 확장 기능 업데이트 | `workbench.extensions.action.updateAllExtensions` |

## References

* [Keyboard shortcuts for macOS](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
