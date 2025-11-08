- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## tr

문자를 자르고 문자를 치환하는 기능을 가진 도구이다.

``` bash
cat auth.log | tr -d 'Nov''*:*:*''pam_unix(*)''cresess''CRO''16''0-9'| tr 'bt\-XDA' "dodo " | awk '{print $1}' | uniq
```

소문자를 대문자로 치환

``` bash
cat auth.log | tr '[a-z]' '[A-Z]'
```

``` bash
cat auth.log | tr '[:lower:]' '[:upper:]'
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
