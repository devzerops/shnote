- 마지막 업데이트: 2025-09-26
- 상태: 초안

# 개요
전송 품질을 보장하기 위해 통신 시스템은 오류 제어(ARQ), 신호 디지털화(PCM), 연결 관리(가상회선), 매체 공유(FDMA/TDMA/CDMA/OFDMA), 샘플링 요건(Shannon/Nyquist)을 결합해 설계된다. 본 문서는 각 개념의 핵심 흐름과 실무에서 확인해야 할 체크포인트를 정리한다.

## ARQ(Automatic Repeat reQuest)
### 종류 비교
| 방식 | 윈도 크기 | 재전송 단위 | 장점 | 주의 사항 |
|------|----------|-------------|------|-----------|
| Stop-and-Wait | 1 | 단일 프레임 | 구현이 단순, ACK/RTO 로직만 유지 | RTT가 긴 회선에서 비효율, 파이프라인 채우지 못함 |
| Go-Back-N | N | 에러 발생 지점 이후 모든 프레임 | 연속 전송 가능, ACK 누락 처리 간단 | 재전송 폭주 가능, 송신 버퍼 필요 |
| Selective Repeat | N | 오류 프레임만 | 대역 효율 최고, 고지연 회선 적합 | 수신측 버퍼·시퀀스 관리 복잡, 타이머 다중화 필요 |

**흐름 요약**
1. 송신자는 윈도 크기 내에서 프레임을 전송하고 타이머를 가동한다.
2. 수신자는 오류 없는 프레임에 대해 ACK, 오류 감지 시 NAK 또는 다음 시퀀스에 대한 ACK을 보낸다.
3. 타임아웃 또는 NAK 발생 시 지정된 범위의 프레임을 재전송한다.

> 참고: ITU-T X.25 Annex A, RFC 1662(HDLC 프레이밍), Tanenbaum & Wetherall *Computer Networks* 5판 Chapter 3.

## PCM(Pulse Code Modulation)
1. **샘플링**: 입력 아날로그 신호를 일정 주기(`T_s`)로 측정함. 샘플링 주파수 `f_s`는 나이퀴스트 조건을 만족해야 한다.
2. **양자화**: 샘플 값을 가장 가까운 이산 값으로 근사. 양자화 단계 `L`에 따라 SNR가 결정된다.
3. **부호화**: 양자화된 값을 비트열로 변환. 예) 8비트 PCM → 64 kbps(음성 8 kHz × 8 bit).
4. **전송/복호화**: 라인코딩(NRZ, RZ 등)을 거쳐 전송, 수신 측에서 역과정 수행.

**실무 메모**
- ITU-T G.711: μ-law/A-law PCM 표준(8 kHz, 8 bit → 64 kbps).
- ITU-T G.722, G.726 등 적응형 Differential PCM(ADPCM) 규격은 파형 예측으로 비트레이트를 48/40/32 kbps 수준으로 축소.
- VoIP 게이트웨이 구성 시 G.711 ↔ G.729 트랜스코딩 비용과 QoS 보장을 고려 (Cisco SRND, ITU-T Y.1540).

## 가상회선 패킷 교환(Virtual Circuit Switching)
1. **호 설정(Call Setup)**: 신호 채널로 경로 예약, 각 노드가 VC ID/Label을 할당. (예: X.25, Frame Relay, MPLS LSP).
2. **데이터 전송**: 흐름 제어, 순서 보장, 상태 유지. 각 패킷은 VC ID를 사용하므로 라우팅 오버헤드가 작다.
3. **호 해제(Teardown)**: 리소스 반환, 상태 테이블 제거.

**장점**: QoS 보장, 순서 보장, 오류 회복 용이 (참고: RFC 3031 MPLS Architecture).
**단점**: 제어 채널 필요, 장애 시 재설정 비용.

## 다중접속/다중화 기법
| 방식 | 자원 분할 | 대표 표준 | 특징 |
|------|-----------|-----------|-------|
| FDMA | 주파수 대역 | 아날로그 셀룰러, 위성 | 대역폭 고정, 간단하지만 가드밴드 필요 |
| TDMA | 시간 슬롯 | GSM, IEEE 802.11 기본 DCF | 동기화 필요, 슬롯 스케줄링으로 QoS 조정 |
| CDMA | 확산 코드 | IS-95, WCDMA | 코드 분할, 주파수 재사용 우수, 근접 효과 문제 (Near-Far Effect) |
| OFDMA | 직교 서브캐리어 + 시간 | LTE/LTE-A/5G NR 다운링크, IEEE 802.16 | 서브캐리어 동적 할당, MIMO와 결합, CFO 관리 중요 |

**OFDMA 실무 포인트** (참고: 3GPP TS 36.211, TS 38.211)
- 서브캐리어 Spacing: LTE 15 kHz, 5G NR은 15/30/60/120/240 kHz (SCS)로 계층화되어 프레임 기간 `1 ms/2^μ`.
- 스케줄러는 CQI(채널 품질 지표), MCS 테이블을 기반으로 PRB(Resource Block)를 할당.
- 주파수 오프셋 보정(CFO)과 동기화 신호(SSB)를 통해 무전기 간 직교 유지.

## Shannon / Nyquist 샘플링 정리
- **나이퀴스트-샤논 샘플링 이론**: 유한 대역폭 `B`(Hz)의 신호는 `f_s ≥ 2B`로 샘플링하면 정보 손실 없이 복원 가능.
- **채널 용량**: `C = B log2(1 + S/N)` bits/s. 동일 대역폭에서 SNR이 3 dB 증가할 때 용량은 약 1 bit/s/Hz 상승.
- **예시**: 4 kHz 음성 채널 → 8 kHz 샘플링, 8 bit PCM → 64 kbps. SNR 30 dB, BW 3 kHz → `C ≈ 3k × log2(1 + 1000) ≈ 29.9 kbps`.

> 참고: Claude Shannon, “A Mathematical Theory of Communication,” *Bell System Technical Journal* (1948); Proakis & Salehi, *Digital Communications*, 5th ed.; ITU-T G.711.

## 체크리스트
- [ ] ARQ 설계 시 RTT, 프레임 길이, 재전송 제한 설정.
- [ ] PCM 회선은 샘플링/양자화 규격(예: G.711, G.722) 확인.
- [ ] 가상회선 네트워크는 VC ID/Label 계획과 장애 복구 시나리오를 마련.
- [ ] 다중접속 선택 시 스펙트럼 효율과 장비 지원(예: OFDMA FFT 크기) 확인.
- [ ] 채널 설계 시 SNR과 대역폭을 측정해 샤논 용량과 목표 전송률을 비교.

## TODO / 후속 연구
- [ ] ARQ × FEC 혼합 하이브리드 ARQ(HARQ) 사례 정리.
- [ ] PCM vs ADPCM 품질 비교 그래프 추가.
- [ ] MPLS LSP 실패 대비 Fast Reroute 시나리오 문서화.
- [ ] 5G NR의  numerology(서브캐리어 간격) 표 추가.

## 참고 자료
- Tanenbaum & Wetherall, *Computer Networks*, 5th ed.
- ITU-T G.711, G.722, X.25 Recommendations.
- 3GPP TS 36.300, TS 38.300 (OFDMA numerology 및 스케줄링).
- Proakis & Salehi, *Digital Communications*.
