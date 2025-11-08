- 마지막 업데이트: 2025-09-24
- 상태: 검토중

# 개요
## Bash 히스토리 & 환경 변수 요약
- 히스토리 확장과 주요 환경 변수를 한 페이지에서 확인하도록 정리했습니다.

## 히스토리 확장 단축키
- `!!` : 직전에 실행한 명령을 그대로 재실행
- `!$` : 직전 명령의 마지막 인수(argument)를 재사용
- `!*` : 직전 명령의 모든 인수 재사용
- `!<history_number>` : 해당 번호의 히스토리 명령 실행 (예: `!100`)
- `!?search_term?` : 지정한 문자열이 포함된 가장 최근 명령 실행 (예: `!?ls?`)

## 주요 환경 변수
- `$PATH` : 실행 파일 검색 경로 목록
- `$HOME` : 현재 사용자 홈 디렉터리 경로
- `$USER` : 로그인한 사용자명
- `$PS1` : 셸 프롬프트 형식 지정 문자열
- `$SHELL` : 사용 중인 셸 프로그램 경로
- `$PWD` / `$OLDPWD` : 현재/이전 작업 디렉터리 경로
- `$TERM` : 터미널 타입 정의
- `$LANG` : 시스템 언어/로케일 정보
- `$TZ` : 시스템 시간대(Time Zone)
- `$IFS` : 입력 필드 구분자(Internal Field Separator)

# 핵심 개념
- 히스토리 확장은 반복 명령 입력 시간을 절약하며, `history -w` 등과 함께 사용하면 편의성이 크게 향상됩니다.
- 환경 변수는 로그인 쉘과 서브쉘에 전파되므로 `export` 여부를 명확히 구분해야 합니다.

# 실무/시험 포인트
- 보안 점검 시 `HISTCONTROL`, `HISTSIZE` 설정값 확인으로 명령 흔적 보존 여부를 파악합니다.
- 사용자별 `.bashrc`, `/etc/profile`에 환경 변수 초기화 스크립트를 배치할 때 중복 정의를 피해야 합니다.

# TODO / 후속 연구
- `bind -P` 결과를 정리해 추가 단축키 표를 작성.
- Zsh, Fish 등 다른 셸과 비교 표 추가.

# 참고 자료
- [GNU Bash Manual – History Interaction](https://www.gnu.org/software/bash/manual/) 
- [Advanced Bash-Scripting Guide](https://tldp.org/LDP/abs/html/) – 환경 변수 섹션.
