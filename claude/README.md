# Windows Claude Code 개발 환경 온보딩

Claude Desktop과 Git for Windows가 설치된 Windows 사용자가 Claude 채팅에 프롬프트를 한 번 붙여 넣어 다음 도구를 준비할 수 있게 돕는 프로젝트입니다.

- winget (Windows 패키지 관리자, 없으면 준비)
- Claude Code CLI
- GitHub CLI
- Git 기본 사용자 이름·이메일
- GitHub 브라우저 로그인
- Claude Code 로그인

## 전제 조건

- Claude Desktop 설치 완료 — [설치 매뉴얼](docs/2.%20Claude_Desktop_설치_매뉴얼.pdf)
- Git for Windows 설치 완료 — [설치 매뉴얼](docs/1.%20Git_for_Windows_설치_매뉴얼.pdf)
- Claude Code를 쓸 수 있는 유료 구독(Pro, Max, Team, Enterprise 중 하나)

Claude Desktop에는 Claude Code가 이미 들어 있습니다. 그런데도 CLI를 따로 설치하는 이유는 터미널에서 `claude` 명령을 직접 실행하고, 편집기나 스크립트에서 Claude Code를 불러 쓰기 위해서입니다.

## 사용 방법

1. Claude Desktop에서 새 채팅을 엽니다.
2. [설치 프롬프트](prompts/windows-dev-environment-setup.ko.md)의 코드 블록 전체를 복사해 채팅에 붙여 넣습니다.
3. Claude가 검사와 설치를 실행하도록 두고, 이름·이메일·업데이트 여부처럼 직접 결정해야 할 질문에만 답합니다.
4. GitHub 로그인 또는 Windows 관리자 권한 창이 열리면 해당 화면에서 직접 완료합니다.
5. Claude Code 로그인이 필요하면 안내에 따라 새 PowerShell 창에서 `claude`를 한 번 실행합니다.

Claude Code 로그인을 뺀 나머지 과정에서는 사용자가 터미널 명령을 직접 입력하거나 설정 파일을 편집할 필요가 없도록 프롬프트를 구성했습니다.

화면 캡처를 보며 따라가려면 [환경 구축 프롬프트 실행 매뉴얼](docs/3.%20환경_구축_프롬프트_실행_매뉴얼.pdf)을 참고하세요. Code 화면으로 전환하고 모델을 설정한 뒤 프롬프트를 실행하는 과정을 단계별로 안내합니다.

## 프롬프트가 하는 일

1. `winget`을 가장 먼저 확인하고, 없으면 Microsoft 공식 절차로 준비합니다. 끝내 사용할 수 없으면 GitHub CLI만 건너뛰고 나머지는 계속 진행합니다.
2. Claude Code CLI가 없으면 공식 설치 스크립트로 설치합니다. 이 설치본은 백그라운드에서 자동으로 최신 버전을 받으므로 따로 업데이트하지 않습니다.
3. GitHub CLI는 설치된 버전과 최신 버전을 비교해 없는 경우에만 설치하고, 업데이트는 사용자 확인 후에만 진행합니다.
4. 기존 Git 설정과 GitHub 로그인을 보존합니다.
5. `winget --version`, `claude --version`, `claude doctor`, `gh --version`, `git config --global user.name`, `git config --global user.email`, `gh auth status`로 최종 상태를 검증합니다.

## 보안 안내

- 비밀번호, GitHub 토큰, Anthropic 계정 비밀번호, 인증 코드, API 키, 개인 키를 Claude 채팅에 보내지 마세요.
- GitHub 로그인, Claude Code 로그인, Windows 관리자 권한 요청은 본인이 표시된 화면에서 직접 처리하세요.
- 프롬프트는 Microsoft Store의 앱 설치 관리자, Claude Code 공식 설치 스크립트(`https://claude.ai/install.ps1`), winget의 공식 패키지 세 경로만 사용하도록 지시합니다. 그 밖의 설치 스크립트나 다운로드 파일은 사용하지 않습니다.
- `winget`을 사용할 수 없으면 Microsoft 공식 절차로 등록을 시도하고, 그래도 안 되면 Microsoft Store 공식 페이지를 안내합니다. 비공식 다운로드 파일은 사용하지 않습니다.

## 공식 출처

- [Claude Code 설치 문서](https://code.claude.com/docs/en/setup)
- [Claude Desktop 시작하기](https://code.claude.com/docs/en/desktop-quickstart)
- [Git for Windows 설치](https://git-scm.com/downloads/win)
- [GitHub CLI](https://cli.github.com/)
- [winget 설치 문서](https://learn.microsoft.com/windows/package-manager/winget/)

## 범위

초기 버전은 Windows 환경의 설치·설정·검증에만 집중합니다. Claude Desktop과 Git for Windows 설치, GitHub 저장소 생성, 코드 내려받기, 자동 재부팅, GUI 설치 프로그램 제작은 포함하지 않습니다.

## 라이선스

이 프로젝트는 [MIT 라이선스](LICENSE)를 따릅니다.
