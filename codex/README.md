[설치 프롬프트로 가기](prompts/windows-dev-environment-setup.ko.md)

# Windows Codex CLI 개발 환경 온보딩

Codex Desktop만 설치된 Windows 사용자가 Codex 채팅에 프롬프트를 한 번 붙여 넣어 다음 도구를 준비할 수 있게 돕는 프로젝트입니다.

- Codex CLI
- Git for Windows
- GitHub CLI
- Git 기본 사용자 이름·이메일
- GitHub 브라우저 로그인

## 사용 방법

1. Codex Desktop에서 새 채팅을 엽니다.
2. [설치 프롬프트](prompts/windows-dev-environment-setup.ko.md)의 코드 블록 전체를 복사해 채팅에 붙여 넣습니다.
3. Codex가 검사와 설치를 실행하도록 두고, 이름·이메일·업데이트 여부처럼 직접 결정해야 할 질문에만 답합니다.
4. GitHub 로그인 또는 Windows 관리자 권한 창이 열리면 해당 화면에서 직접 완료합니다.

사용자가 터미널 명령을 직접 입력하거나 설정 파일을 편집할 필요가 없도록 프롬프트를 구성했습니다.

## 프롬프트가 하는 일

1. `winget`과 각 도구의 설치 여부를 확인합니다.
2. 설치된 버전과 최신 버전을 비교합니다.
3. 없는 도구만 설치하고, 업데이트는 사용자 확인 후에만 진행합니다.
4. 기존 Git 설정과 GitHub 로그인을 보존합니다.
5. `codex --version`, `git --version`, `gh --version`, `gh auth status`로 최종 상태를 검증합니다.

## 보안 안내

- 비밀번호, GitHub 토큰, 인증 코드, 개인 키를 Codex 채팅에 보내지 마세요.
- GitHub 로그인과 Windows 관리자 권한 요청은 본인이 표시된 화면에서 직접 처리하세요.
- 프롬프트는 공식 문서와 신뢰할 수 있는 패키지 관리자만 사용하도록 지시합니다.
- `winget`을 사용할 수 없으면 비공식 다운로드 파일을 사용하지 않고, Codex가 안전한 다음 조치를 안내합니다.

## 공식 출처

- [OpenAI Codex 문서](https://developers.openai.com/codex/)
- [Git for Windows 설치](https://git-scm.com/install/windows)
- [GitHub CLI](https://cli.github.com/)

## 범위

초기 버전은 Windows 환경의 설치·업데이트·설정·검증에만 집중합니다. GitHub 저장소 생성, 코드 내려받기, 자동 재부팅, GUI 설치 프로그램 제작은 포함하지 않습니다.

## 라이선스

이 프로젝트는 [MIT 라이선스](LICENSE)를 따릅니다.
