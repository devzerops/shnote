- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## GCC LANG tips
------------------------
**GCC(GNU Compiler Collection)**


>화면 출력

| 명령어 | 설명 | 사용법 | 결과 |
| --- | --- | --- | --- |
| gcc -o (name) | 출력할 결과물의 담을 이름을 결정하는 명령어  | gcc -o test.out test.c | test.out |
| gcc -g | 바이너리 파일에 표준 디버깅 정보를 포함시킴 | gcc -g test.c | a.out |
| gcc -v | 컴파일 과정의 명령어와 gcc 버전 정보 표시 | gcc -v test.c | a.out (+화면출력도 함) |
| gcc -E | 전처리 과정 화면에 출력함 | gcc -E test.c | (화면에 출력) |


> 에러 옵션  

| 명령어 | 설명 | 사용법 | 결과 |
| --- | --- | --- | --- |
| gcc -W | 가능한 모호한 코딩에 대해 경고 메시지 출력 | gcc -W test.c | a.out |
| gcc -Wall | 모든 모호한 코딩에 대한 경고 메시지 출력 | gcc -Wall test.c | a.out |
| gcc -Werror | gcc가 보고하는 모든 경고를 오류로 변환 | gcc -Werror test.c | a.out |

> 단계별 출력 방법

| 명령어 | 설명 | 사용법 | 결과 |
| --- | --- | --- | --- |
| gcc -E -o | 전처리 완료된 파일 저장 | gcc -E -o test.i test.c | test.i |
| gcc -S | 어셈블리까지 컴파일하고  어셈블리 파일 저장 | gcc -S  test.c | test.s |
| gcc -C | 링킹 과정없이 컴파일된 코드 출력 | gcc -C test.c | a.out |
| gcc -c | 오브젝트(기계어) 파일로 저장 | gcc -c test.c | test.o |
| gcc  | 그냥 출력하기 | gcc test.c | test.out |
| gcc -save-temps  | 컴파일과정중 생기는 파일을 지우지않음 | gcc -save-temps test.c | test.c ->    a.out, test.c   test.i, test.o   test.s |

> 참고하면 좋은 확장자

| 확장자 | 설명 |
| --- | --- |
| .c | c 소스파일 |
| .i | 전처리 된 C 파일 |
| .o | 오브젝트 파일 |
| .S .s | 어셈블리어로 된 파일 |
| .a .so | 컴파일된 라이브러리 파일 |

> gcc 최적화 명령어들


| option | Explanation | execution time | code size | memory usage | compile time |
| --- | --- | --- | --- | --- | --- |
| gcc -O0 | \-모든 최적화 비활성화   \-컴파일 빌드시간 증가   \-디버깅 정확도 증가 | +1 | +1 | +1 | +1 |
| gcc -O or -O1 | \-core 최적화를 활성화   \-스텍 사용량 증가   \-디버깅 정확도 감소 | \-1 | \-1 | +1 | +1 |
| gcc -O2 | \-디버깅 정확도 감소   \-O1보다 코드커질 가능성 있음   \-성능최적화 O1보다 우위 | \-2 | default | +1 | +2 |
| gcc -O3 | \- 디버깅 정확도 감소   \-가장 높은레벨의 최적화   \-모든 함수 인라인함수와 같은취급 | \-3 | default | +1 | +3 |

> gcc -O 최적화 표

| option | Explanation | execution time | code size | memory usage | compile time |
| --- | --- | --- | --- | --- | --- |
| gcc -Ofast | \-표준문법을 무시하고 최적화   \-O3에서 최적화 수행   \-O3보다 코드크기가 증가할수도 있음 | \-3 | default | +1 | +3 |
| gcc -Omax    | \-성능 최적화를 우선으로 함   \-Ofast레벨의 최적화를함   \-디버깅 어려움   \-Ofast보다 코드크기 증가할수있음 | \-3 | ? | ? | +3 |
| gcc -Omin    | \-가장작은 크기의 코드제공을 목표   \-O1에 보다 디버깅하기 어려움   \-전역 메모리 액세스를 최적화   \-사용치 않는코드 제거 | ? | \-3 | ? | ? |
| gcc -Os | \-코드 크기 늘리지않고 고성능 제공 | default | \-2 | default | +2 |
| gcc -Oz    | \-최대수준의 코드 크기 최적화 목표   \-성능 최적화는 무시   \-loop는 do while 사용   \-inline 최적화 비활성화 | ? | \-3 | ? | ? |

상황에 따라 약간식 변동이 있기도하고 해서 정확한건 아니지만 어느정도는 참고사항으론 볼만 함



| 최적화 목표 (이건 공식표) | 최적화 레벨 |
| --- | --- |
| smaller code size | \-Oz , -Omin |
| fater execution time | \-O2, -O3, -Ofast, -Omax |
| Good debug experience without code bloat | \-O1 |
| Better correlation between source code and generated code | \-O0 |
| Faster compile and build time | \-O0 |
| Balanced code size reduction and fast performance | \-Os |

----------

>그 외 라이브러리 기능

| 명령어 | 설명 | 사용법 | 결과 |
| --- | --- | --- | --- |
| gcc -l | 헤더파일 포함 디렉토리를 추가한다. | gcc file1.c -Llibs -lmylib -o outfile | outfile |
| gcc -L | 라이브러리 검색 경로 추가 | gcc file1.c -Llibs -lmylib -o outfile | outfile |
| gcc -static | 정적 라이브러리로 링킹 수행 | gcc -static src.c obj.o -o test | test |
| gcc -shared | 공유 라이브러리 우선 링크하도록 해줌 | gcc -shared -o test.so obj.o | test.so |
| gcc -s | 실행 파일에서 모든 기호 테이블 및 재배치 정보를 제거 | gcc -s test.c | a.out |

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
