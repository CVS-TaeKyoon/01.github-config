# Windows 개발 환경 자동 설정 요청 프롬프트

아래 전체 내용을 Claude 채팅에 붙여 넣으세요.

```text
나는 개발 경험이 거의 없는 Windows 사용자야. 이 컴퓨터에는 Claude Desktop과 Git for Windows가 이미 설치되어 있어. 내가 터미널에 명령을 입력하거나 파일을 직접 고치게 하지 말고, 네가 이 컴퓨터에서 필요한 검사와 설치를 직접 진행해 줘.

목표는 Claude Code CLI와 GitHub CLI를 사용할 수 있게 만들고 Git 기본 사용자 설정, GitHub 로그인, Claude Code 로그인을 끝내는 것이야. Git for Windows는 이미 설치되어 있으니 설치 여부를 확인하지 않아도 돼. 설치를 시작하기 전에 아래 원칙을 지켜.

- Windows 환경인지, PowerShell을 사용할 수 있는지 먼저 확인해. 검사 결과는 쉬운 한국어로 알려 줘.
- 비밀번호, GitHub 토큰, Anthropic 계정 비밀번호, 인증 코드, API 키, 개인 키는 나에게 채팅으로 묻거나 채팅에 표시하지 마. 로그인·관리자 권한 창이 나오면 내가 직접 입력하도록 안내해.
- 내게 무엇을 물을 때는 한 번에 하나만 물어. 여러 질문을 목록으로 한꺼번에 하지 마.
- 내가 직접 터미널 명령을 실행하라고 시키지 마. 필요한 명령은 네가 실행해. 단 하나의 예외는 6-2단계의 Claude Code 로그인이야. 그 로그인은 터미널 화면 안에서 진행되고 본인이 직접 완료해야 하므로, 그때만 내가 할 일을 안내해 줘.
- 설치에는 공식 출처만 사용해. winget은 Microsoft Store의 앱 설치 관리자만, Claude Code는 `https://claude.ai/install.ps1` 공식 설치 스크립트만, GitHub CLI는 winget의 `GitHub.cli` 공식 패키지만 사용해. 이 세 경로 외의 설치 스크립트, 임의의 다운로드 사이트, 출처 불명의 파일, 비공식 패키지는 사용하지 마.
- 설치, 업데이트, 설정 실패 시 이미 성공한 도구나 기존 설정을 되돌리거나 덮어쓰지 마. 원인과 안전한 다음 행동을 설명한 뒤 필요한 경우에만 질문해.

다음 절차를 순서대로 진행해.

1. 가장 먼저 winget(Windows 패키지 관리자)을 확인하고, 없으면 winget부터 준비해. 뒤에서 GitHub CLI를 설치할 때 필요해.
   - `winget --version`을 실행해 winget을 쓸 수 있는지 확인해. 쓸 수 있으면 바로 2단계로 가.
   - 쓸 수 없으면 `(Get-AppxPackage Microsoft.DesktopAppInstaller).Version`을 실행해 앱 설치 관리자(App Installer)가 있는지 확인해. winget은 이 앱 안에 들어 있어.
   - 앱 설치 관리자는 있는데 `winget` 명령만 인식되지 않는 경우가 가장 흔해. 이때는 `Add-AppxPackage -RegisterByFamilyName -MainPackage Microsoft.DesktopAppInstaller_8wekyb3d8bbwe`를 실행해 등록해. 이 명령은 새로 내려받는 파일이 없고 관리자 권한도 필요 없어.
   - 등록한 뒤 새 PowerShell 세션에서 `winget --version`을 다시 확인해.
   - 그래도 인식되지 않거나 앱 설치 관리자 자체가 없으면, Microsoft Store 공식 페이지 `https://apps.microsoft.com/detail/9nblggh4nns1`에서 "앱 설치 관리자"를 설치하도록 나에게 안내해. 이 화면은 내가 직접 설치 버튼을 눌러야 해. 내가 완료했다고 말하면 `winget --version`을 다시 확인해.
   - 여기까지 해도 winget을 쓸 수 없으면 멈추지 말고 그 사실만 알려 준 뒤 2단계로 가. winget은 GitHub CLI 설치에만 필요하고 Claude Code는 winget 없이 설치할 수 있어. 이때는 5단계를 건너뛰고 마지막 요약에서 GitHub CLI가 미완료임을 알려 줘.

2. `claude --version`과 `gh --version`을 각각 확인해. 명령을 찾지 못한 경우와 실행은 되지만 오류가 나는 경우를 구분해.

3. Claude Code CLI를 확인하고 필요하면 설치해.
   - `claude --version`이 버전을 출력하면 이미 설치된 상태야. `claude doctor`를 실행해 설치 상태와 설정에 문제가 없는지 확인하고, 결과를 쉬운 말로 알려 줘.
   - 설치되어 있지 않으면 PowerShell에서 공식 설치 명령 `irm https://claude.ai/install.ps1 | iex`를 실행해. 이 설치는 관리자 권한이 필요하지 않아.
   - 설치 직후에는 PATH가 아직 반영되지 않을 수 있어. 새 PowerShell 세션에서 `claude --version`을 다시 확인해. 그래도 인식되지 않으면 어떤 창을 닫고 다시 열어야 하는지 한 가지씩 안내해.
   - Claude Code 공식 설치본은 백그라운드에서 자동으로 최신 버전을 받아. 따로 업데이트를 실행하거나 업데이트 여부를 나에게 묻지 마.

4. Git 전역 사용자 설정을 확인해.
   - `git config --global user.name`과 `git config --global user.email`을 확인해.
   - 기존 값이 있으면 값 자체를 과도하게 노출하지 말고, 설정되어 있다는 사실과 변경 여부를 물어봐.
   - 이름과 이메일은 각각 한 번에 하나씩 물어보고, 내가 답한 값으로만 `git config --global user.name`과 `git config --global user.email`을 설정해.
   - 이메일 형식이 명백히 잘못된 경우에만 이유를 설명하고 같은 항목을 다시 물어봐.

5. GitHub CLI를 확인하고 필요하면 설치해.
   - 1단계에서 winget을 끝내 쓸 수 없었다면 이 단계 전체를 건너뛰고 6-1단계도 건너뛴 뒤, 마지막 요약에서 GitHub CLI가 미완료임을 알려 줘. 비공식 설치 파일로 대신 설치하지 마.
   - winget의 `GitHub.cli` 공식 패키지 정보로 설치된 버전과 최신 버전을 비교해 쉬운 말로 보여 줘.
   - 설치되어 있지 않으면 설치를 진행해.
   - 이미 최신이면 설치나 업데이트를 건너뛰고 그대로 유지해.
   - 업데이트가 가능하면 현재 버전과 최신 버전을 보여 준 뒤, 업데이트할지 나에게 물어봐. 내가 동의한 경우에만 업데이트해.
   - 설치나 업데이트 뒤에는 필요한 경우 새 PowerShell 세션에서 `gh --version`을 다시 확인해.

6. 로그인 두 가지를 차례로 처리해.

   6-1. GitHub CLI 로그인을 `gh auth status`로 확인해.
   - GitHub CLI를 설치하지 못했다면 이 단계를 건너뛰고 6-2단계로 가.
   - 이미 로그인되어 있으면 기존 로그인을 유지하고 다음으로 가.
   - 로그인되어 있지 않으면 `gh auth login`으로 GitHub.com, HTTPS, 브라우저 로그인 방식을 사용해. 브라우저나 코드 입력 화면이 열리면 내가 직접 완료할 때까지 기다려. 인증 정보를 묻거나 대신 입력하지 마.
   - 내가 완료했다고 말하면 `gh auth status`로 상태를 다시 확인해.

   6-2. Claude Code 로그인 상태를 확인해.
   - 먼저 `claude doctor` 결과를 확인해. 로그인 여부가 분명하지 않으면 `claude -p "ok"`처럼 짧은 비대화형 명령을 한 번 실행해 인증 오류가 나는지 확인해. 이 명령은 터미널 화면을 차지하지 않고 바로 끝나.
   - 이미 로그인되어 있으면 그대로 두고 다음 단계로 가.
   - 로그인되어 있지 않으면 이 단계만 내가 직접 해야 해. Claude Code 로그인은 터미널 화면 안에서 진행되고 계정 인증은 본인이 완료해야 하기 때문이야. 나에게 다음을 한 번에 하나씩 안내해 줘.
     1) 새 PowerShell 창을 연다.
     2) `claude`를 입력하고 실행한다.
     3) 화면 안내에 따라 브라우저에서 로그인을 완료한다.
     4) 로그인이 끝나면 `/exit`를 입력해 Claude Code를 종료한다.
   - Claude Code는 Pro, Max, Team, Enterprise 중 하나의 유료 구독이 필요해. 무료 플랜에서는 사용할 수 없어. 로그인 화면에서 업그레이드하라는 안내가 나오면 그 사실을 나에게 쉬운 말로 알려 줘.
   - 내가 완료했다고 말하면 로그인 상태를 다시 확인해.

7. 마지막으로 아래 항목을 직접 실행해 모두 확인하고, 결과를 표가 아닌 짧은 목록으로 요약해. 설치하지 못한 도구의 항목은 실행하지 말고 미완료로 표시해.
   - `winget --version`
   - `claude --version`
   - `claude doctor`
   - `gh --version`
   - `git config --global user.name`
   - `git config --global user.email`
   - `gh auth status`

완료되지 않은 항목이 있으면 성공한 항목은 그대로 두고, 실패한 항목·원인·다음에 내가 해야 할 한 가지 행동만 쉬운 말로 알려 줘. 완료되면 Claude Code와 GitHub CLI가 준비되었다고 알려 주고, 다음에 터미널에서 `claude`로 작업을 시작하거나 GitHub 저장소를 만들 수 있다고 짧게 안내해.
```

## 포함된 도구

- winget (Windows 패키지 관리자, 없으면 준비)
- Claude Code CLI
- GitHub CLI

## 전제

- Claude Desktop 설치 완료
- Git for Windows 설치 완료

프롬프트는 기존 설치와 설정을 먼저 확인하므로, 이미 설치된 도구를 불필요하게 다시 설치하지 않습니다.
