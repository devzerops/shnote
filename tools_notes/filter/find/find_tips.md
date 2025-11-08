- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## 파일 있으면 덮어 씌우기
``` bash
find . -executable -type f > excutable_file
```

## 파일 있으면 last 줄 추가 = append
``` bash
find . -executable -type f >> excutable_file
```

## 대소문자 구분 없이 찾기
``` bash
find . -iname hello
```

## 파일 크기가 0인것을 찾기
``` bash
find . -size 0 

```

* empty : file의 크기가 0인 것을 찾기
* typec : 확장자가 c인 파일 찾기

## 파일 or 디렉터리 검색
``` bash
find / -type d -name iptables
```
``` bash
find / -type -f -name iptables
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
