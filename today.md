# 네트워크·시스템 학습 노트
- 마지막 업데이트: 2025-09-24
- 상태: 정리 진행 중 (기존 브레인덤프 재구성)

## 인터페이스 & 전송 장비
- **USB Type-C / Thunderbolt**: 고속 데이터·전력 전송을 지원하는 범용 커넥터, 썬더볼트는 PCIe 기반 추가 기능 제공.
- **Firebolt / CATV**: 전송 매체 유형 비교 참고 (케이블 TV 계열 포함).
- **Token Bus / Token Ring / XSMA**: 토큰 기반 매체 접근 제어(MAC) 개념 복습 대상.
- **ATM 셀 스위칭**: 고정 길이(53바이트) 셀 기반 패킷망, 실무에서 축소되고 있으나 개념 유지.
- **신호 변환 장치(DCE)**와 **종단 장비(DTE)**: 모뎀 등 DCE가 회선 신호를 변조/복조, 라우터는 DTE 역할.
- **통신제어장치(CCU)**: 다중 접속 제어·경로 설정 등 전송 제어 담당.

## 전송 회선 & 접속 방식
- **2선식 vs 4선식 회선**: 동일 회선 사용(2W)과 상·하행 분리(4W) 비교.
- **Point-to-Point / Multipoint / Line Concentration / Line Multiplexing**: 회선 구성 방식 정리.
- **Multipoint 운용**: 폴링, 선택(Selection), 경쟁(Contention) 접근 방식 차이.

## 프로토콜 & 서비스 개념
- **802.14 WB**, **SNTP**, **DNS**, **SNMP**: 네트워크 관리/동기화/네임서비스 재점검.
- **Connectionless 통신**: 상태 유지 없이 Best-Effort 제공.
- **IPv6 Flow Label**: 동일 플로우 패킷에 빠른 처리 적용.
- **DHCP DORA, FCS, Manchester Encoding, IEEE 802.3** 등 물리·데이터링크 계층 항목 복습.
- **NGFW, 프론트홀/백홀, RAN, DC-RU-DU**: 통신 인프라 구성요소 확인.

## 애플리케이션 계층 & 세션 유지
- HTTP 기반 세션 유지 기법: 쿠키, 세션, hidden field, URL rewriting 비교.
- RSS(Really Simple Syndication): XML 기반 콘텐츠 업데이트 배포 형식.

## 소프트웨어 아키텍처 개념 리마인더
- **라이브러리 vs API vs 프레임워크**: 프레임워크는 IoC 기반, 라이브러리는 코드와 API 제공.
- **DI(Dependency Injection)**, **AOP(Aspect-Oriented Programming)**, **CID**, **IPO**: 객체지향 설계 키워드.
- **하이퍼바이저**, **JPA**, **Beans**, **컨테이너**: 서버/애플리케이션 플랫폼 개념 복습.

## 암호·보안 관련 키워드
- **Perfect Forward Secrecy (PFS)**, **TLS 1.0 취약점(POODLE, RC4)**, **RFC 5246**, **Session Splicing** 참고.
- **Ping of Death 변형(pingbed)** 등 고전 공격 재검토.

## 참고 링크
- Security Onion 사용자 그룹: <https://groups.google.com/g/security-onion/c/KC5GlhM5RVw/discussion>
- Access Control 자료: <https://www.bluesplatter.com/is_certification/ISCert-04IS-02AccessControl/>

## 추가 학습 TODO
- CRC 연산 (CCITT, ITU-T X.509 포함)
- DCE/DSU 구성과 동기식 전송 장비
- 버퍼, 스택, 동기화 기본 개념 요약 작성
- Manchester 방식과 4상 위상 변조(PSK) 계산 예제 풀어보기
- NGFW, RAN 구성요소, 백홀/프론트홀 차이 정리
