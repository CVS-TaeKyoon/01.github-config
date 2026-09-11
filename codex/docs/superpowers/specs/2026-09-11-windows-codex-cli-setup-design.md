# Windows Codex CLI 개발 환경 온보딩 설계

## 목적

Codex Desktop만 설치된 Windows 사용자가 채팅에 프롬프트 하나를 붙여 넣어 Codex CLI, Git for Windows, GitHub CLI를 안전하게 설치·업데이트·검증하고 Git과 GitHub 로그인을 설정하게 한다.

## 범위

- Windows와 PowerShell, `winget` 사용 가능 여부를 확인한다.
- Codex CLI, Git for Windows, GitHub CLI의 설치 여부·현재 버전·업데이트 가능 여부를 확인한다.
- 미설치 도구만 설치한다. 업데이트가 있으면 현재/최신 버전을 보여 주고 사용자에게 승인받는다.
- Git 전역 사용자 이름과 이메일을 항목별로 물어 설정한다. 기존 값이 있으면 변경 여부를 물어본다.
- GitHub CLI 브라우저 로그인을 시작하고 로그인 상태를 확인한다.
- 설치와 설정 결과를 명령으로 검증한다.

## 제외 범위

- GUI 설치 프로그램 제작, 자동 재부팅, GitHub 저장소 생성·푸시, 토큰·비밀번호 입력 대행은 하지 않는다.
- `winget`이 없을 때 임의의 다운로드 사이트나 비공식 설치 파일을 사용하지 않는다.

## 파일 구조

- `README.md`: 저장소 소개, 사용법, 보안 주의사항, 공식 출처를 제공한다.
- `prompts/windows-dev-environment-setup.ko.md`: Codex 채팅에 복사할 단일 프롬프트를 제공한다.
- `LICENSE`: 공개 저장소의 재사용 조건을 명시한다.

## 대화와 안전 원칙

- 사용자에게 터미널 명령이나 파일 편집을 요구하지 않는다. Codex가 검사와 실행을 수행한다.
- 이름, 이메일, 업데이트 승인처럼 사용자 결정이 필요한 질문은 한 번에 하나만 한다.
- GitHub 로그인과 관리자 권한 창의 입력은 사용자가 직접 한다. 비밀번호·토큰·개인 키는 채팅으로 묻거나 표시하지 않는다.
- 이미 설치되었거나 이미 로그인된 구성은 보존한다. 실패 시 성공한 구성이나 기존 설정을 되돌리지 않는다.

## 완료 조건

다음 명령이 성공하고, Git 사용자 정보와 GitHub 인증 상태가 확인되면 완료다.

```powershell
codex --version
git --version
gh --version
git config --global user.name
git config --global user.email
gh auth status
```
