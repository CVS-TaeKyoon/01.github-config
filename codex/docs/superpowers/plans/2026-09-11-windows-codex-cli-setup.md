# Windows Codex CLI 개발 환경 온보딩 Implementation Plan

> **에이전트 작업자용:** REQUIRED SUB-SKILL: 이 계획을 작업별로 구현하려면 `superpowers:subagent-driven-development`(권장) 또는 `superpowers:executing-plans`를 사용한다. 단계에는 추적용 체크박스(`- [ ]`)를 사용한다.

**목표:** 비개발자가 Codex 채팅에 붙여 넣어 Windows 개발 도구를 설치·설정할 수 있는 공개용 문서와 프롬프트를 만든다.

**아키텍처:** 실행 코드는 저장소에 두지 않고, 검증 가능한 절차를 가진 단일 Codex 프롬프트를 제공한다. README는 사용법과 보안 경계를 설명하고, 프롬프트 파일은 검사·설치·업데이트·설정·검증 순서를 지시한다.

**기술 스택:** Markdown, PowerShell, Windows Package Manager(`winget`), Codex CLI, Git for Windows, GitHub CLI

**설계:** `docs/superpowers/specs/2026-09-11-windows-codex-cli-setup-design.md`

## 전역 제약

- 대상 플랫폼은 Windows다.
- 설치 전 각 도구의 명령·현재 버전·최신 버전을 검사한다.
- 업데이트는 사용자 승인 뒤에만 수행한다.
- 비밀번호, 토큰, 개인 키를 채팅에 요청하거나 출력하지 않는다.
- `winget`이 없을 때 비공식 다운로드 경로를 사용하지 않는다.
- 사용자에게 직접 터미널 명령이나 파일 편집을 요구하지 않는다.

---

### 작업 1: 설치 프롬프트 작성

**파일:**
- 생성: `prompts/windows-dev-environment-setup.ko.md`
- 검증: `prompts/windows-dev-environment-setup.ko.md`

**인터페이스:**
- 소비: Windows에서 Codex Desktop을 실행 중인 비개발자
- 생성: 사용자가 전체를 복사해 Codex 채팅에 붙여 넣을 수 있는 Markdown 프롬프트

- [ ] **단계 1: 실패 기준을 텍스트 검증으로 정의한다.**

프롬프트 파일에 다음 필수 문구가 모두 없으면 실패로 본다.

```text
winget
codex --version
git --version
gh --version
gh auth login
git config --global user.name
git config --global user.email
한 번에 하나만
비밀번호
토큰
```

- [ ] **단계 2: 실패함을 확인한다.**

실행:

```powershell
Test-Path prompts/windows-dev-environment-setup.ko.md
```

예상 결과: `False`

- [ ] **단계 3: 최소 프롬프트를 작성한다.**

다음 순서와 행동을 명시한다.

```text
환경 검사 → 도구별 현재/최신 버전 확인 → 설치 또는 승인된 업데이트 → Git 이름/이메일 설정 → GitHub 브라우저 로그인 → 명령 검증 → 결과 요약
```

- [ ] **단계 4: 필수 문구를 검증한다.**

실행:

```powershell
$required = 'winget','codex --version','git --version','gh --version','gh auth login','git config --global user.name','git config --global user.email','한 번에 하나만','비밀번호','토큰'
$text = Get-Content -Raw prompts/windows-dev-environment-setup.ko.md
$required | Where-Object { $text -notlike "*$_*" }
```

예상 결과: 출력 없음

- [ ] **단계 5: 변경 사항을 커밋한다.**

현재 폴더는 Git 저장소가 아니므로 원격 저장소를 만들거나 초기화한 뒤에만 실행한다.

```powershell
git add prompts/windows-dev-environment-setup.ko.md
git commit -m "docs: Windows 개발 환경 설치 프롬프트 추가"
```

### 작업 2: 공개용 README와 라이선스 작성

**파일:**
- 생성: `README.md`
- 생성: `LICENSE`
- 검증: `README.md`, `LICENSE`

**인터페이스:**
- 소비: GitHub에서 저장소를 방문한 Windows 사용자
- 생성: 프롬프트 위치, 보안 경계, 공식 출처를 안내하는 문서와 MIT 라이선스

- [ ] **단계 1: 실패 기준을 텍스트 검증으로 정의한다.**

README에는 프롬프트 링크, Windows 전제, GitHub 로그인 안내, 보안 주의, 세 공식 출처가 있어야 한다.

- [ ] **단계 2: 실패함을 확인한다.**

실행:

```powershell
Test-Path README.md
Test-Path LICENSE
```

예상 결과: 둘 다 `False`

- [ ] **단계 3: 최소 README와 MIT 라이선스를 작성한다.**

README에는 다음 링크를 넣는다.

```text
https://git-scm.com/install/windows
https://cli.github.com/
https://developers.openai.com/
```

- [ ] **단계 4: README와 라이선스를 검증한다.**

실행:

```powershell
$required = 'Windows','prompts/windows-dev-environment-setup.ko.md','비밀번호','토큰','https://git-scm.com/install/windows','https://cli.github.com/','https://developers.openai.com/'
$text = Get-Content -Raw README.md
$required | Where-Object { $text -notlike "*$_*" }
Select-String -Path LICENSE -Pattern 'MIT License'
```

예상 결과: 첫 명령은 출력 없음, 두 번째 명령은 `MIT License`를 출력

- [ ] **단계 5: 변경 사항을 커밋한다.**

```powershell
git add README.md LICENSE
git commit -m "docs: 온보딩 사용법과 라이선스 추가"
```

### 작업 3: 전체 문서 검토

**파일:**
- 검토: `README.md`
- 검토: `prompts/windows-dev-environment-setup.ko.md`
- 검토: `LICENSE`

**인터페이스:**
- 소비: 공개 GitHub 저장소의 방문자
- 생성: 누락된 경로나 안전 규칙이 없는 초기 배포 문서

- [ ] **단계 1: 내부 링크와 금지 범위를 확인한다.**

실행:

```powershell
rg -n "windows-dev-environment-setup\.ko\.md|비밀번호|토큰|winget|gh auth login" README.md prompts/windows-dev-environment-setup.ko.md
```

예상 결과: 각 핵심어가 관련 파일에 최소 한 번 이상 표시

- [ ] **단계 2: 프롬프트의 완료 검증 명령을 확인한다.**

실행:

```powershell
rg -n "codex --version|git --version|gh --version|gh auth status" prompts/windows-dev-environment-setup.ko.md
```

예상 결과: 네 명령이 모두 표시

- [ ] **단계 3: 상태를 확인하고 커밋한다.**

실행:

```powershell
git status --short
```

예상 결과: 새 문서가 표시된다. Git 저장소 초기화와 원격 연결은 사용자가 GitHub 공개 저장소를 만들 준비가 된 뒤 별도 수행한다.
