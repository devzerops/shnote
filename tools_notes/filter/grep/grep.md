- 마지막 업데이트: 2025-09-26
- 상태: 초안

# 개요
`grep`은 텍스트 스트림에서 패턴을 검색하는 CLI 도구입니다. GNU grep은 기본 BRE(Basic Regular Expression)를 사용하며 `-E`(ERE), `-F`(고정 문자열), `-P`(Perl 호환) 등의 확장 옵션을 제공합니다.

## 기본 사용 예시
```bash
# 현재 디렉터리 전체에서 문자열 "pattern" 검색 (라인 번호, 하이라이트 포함)
grep -RIn --color=auto 'pattern' .

# 파일 목록에서 "test" 문자열 제외
ls | grep -v 'test'

# 확장 정규식(ERE) 사용
grep -E 'foo|bar' file.txt

# 대소문자 무시, 단어 경계 검색
grep -i -w 'error' /var/log/syslog

# C 소스에서 TODO 주석 찾기 (Perl 정규식)
grep -RPn "//\s*TODO" src/
```

## 옵션 정리
| 옵션 | 설명 |
|------|------|
| `-R` / `-r` | 재귀 검색 (심볼릭 링크 포함/제외) |
| `-n` | 라인 번호 표시 |
| `-I` | 바이너리 파일 무시 |
| `-o` | 일치한 문자열만 출력 |
| `-H` | 파일명 강제 표시 |
| `-l` / `-L` | 일치/불일치 파일명만 출력 |
| `--exclude`, `--include` | 특정 파일 패턴 제외/포함 |
| `--exclude-dir` | 디렉터리 제외 |
| `-F` | 고정 문자열 검색 (정규식 해석 미사용) |

## 확장 활용
- 패턴 파일 사용: `grep -f patterns.txt logfile`
- 하이라이트 강제: `grep --color=always` (파이프 후 egrep 등) 
- 맥OS: BSD grep은 `-P` 미지원 → Homebrew `ggrep` 권장.
- 멀티코어 엑셀러레이션은 ripgrep(`rg`) 또는 `ugrep` 등 대체 도구 검토.

# 핵심 개념
- BRE와 ERE의 차이를 파악하고, 메타문자 `+`, `?`, `|` 등을 사용할 때는 `-E` 옵션을 고려합니다.
- 로케일/인코딩에 따라 매칭 결과가 달라질 수 있으므로, 성능이 중요하면 `LC_ALL=C`로 지정해 바이트 단위 비교를 수행합니다.
- `grep -F` 모드를 활용하면 긴 패턴 목록을 빠르게 검사할 수 있습니다 (Aho–Corasick 기반 구현).

# 실무/시험 포인트
- 로그 분석: `tail -F app.log | grep --line-buffered 'ERROR'` 형태로 실시간 필터링.
- CI/CD: `grep -q 'SUCCESS' artifact.log`로 조건 분기.
- 보안 감사: `grep -RP "AKIA[0-9A-Z]{16}"`처럼 정규식으로 민감 정보 검사.
- 자격증/필기시험에서 정규식 메타문자(`^`, `$`, `.` 등) 해석 문제가 자주 출제됩니다.

# TODO / 후속 연구
- GNU grep 3.8의 `--binary-files=without-match` 기본 동작과 경고 메시지 변화 정리.
- ripgrep(`rg`)와 성능/기능 비교 표 작성.
- `grep --mmap`, `--no-mmap` 옵션 차이 및 대용량 로그 처리 시 주의사항 조사.

# 참고 자료
- GNU grep Manual – <https://www.gnu.org/software/grep/manual/>
- POSIX Regular Expression Specification (IEEE 1003.1).
- Red Hat System Administration Guide – *Text processing with grep/egrep*.
