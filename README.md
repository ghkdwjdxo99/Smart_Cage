# Smart_Cage

## 프로젝트 목표
반려동물 임시 보관을 위한 스마트 케이지 시스템 구축을 목표로 합니다.

Raspberry Pi 4를 사용해 서버를 구현

STM32를 사용해 Cage의 온습도, 조도, DC모터를 **실시간 제어**

Arduino를 사용해 관리자 시스템 구현 (**사용자 제어 가능**)
- 환경 센서(온도, 습도, 조도)를 활용한 실시간 상태 모니터링 기능 구현
- 서버 기반의 사용자 제어 시스템 개발 (조명, 환기창, 선풍기 등)
- 입력 센서와 출력 제어를 분리한 효율적인 보드 아키텍처 설계
- TCP/IP 기반 Wi-Fi 통신을 통한 장치 간 연동 및 통합 관리


## 구성도
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/c71615dc-08b6-4864-b9bf-99865a3c03e5" />


## 흐름도
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/fa78d1c5-d76d-4b0e-ba7c-5e9596036376" />


## 명칭 및 역할 정리
| 명칭 | 역할 | 보드 |
|------|------|------|
| **User (사용자)** | 사용자 명령 입력 및 상태 확인 인터페이스 | LDplayer4 (모바일) |
| **Server (서버)** | - 전체 시스템의 중앙 제어 서버<br>- 모든 CCU로부터 케이지 상태 수신 및 저장<br>- 사용자 제어 요청 처리<br>- 관리자 MCU와 Bluetooth 통신<br>- 케이지 할당, 기기 동작 상태, 대여 시간 등 기록 | Raspberry Pi |
| **CCU (Cage Control Unit)** | - 각 케이지에 1대씩 설치<br>- 온도, 습도, 조도 센서 측정<br>- 내부 조명 LED 및 상태 LED 제어<br>- 창문 개폐 모터 제어<br>- 선풍기 모터 제어<br>- LCD 상태 출력<br>- Wi-Fi로 서버와 직접 통신 | STM32 |
| **MCU (Manager Control Unit)** | - 관리자 전용 제어 보드<br>- 서버(Raspberry Pi)와 Bluetooth로 통신<br>- 케이지 상태 전체 모니터링<br>- 관리자 제어 명령 전송 (창문, LED, 팬, 부저 등) | Arduino + Bluetooth 모듈 |


## 사용 모듈
**CCU (Cage Control Unit) 모듈**
| 모듈 이름 | 역할 |
|-----------|------|
| DHT11 | 온습도 측정 |
| CDS 센서 (LDR) | 조도 측정 |
| LED × 2 | 상태 표시 (내부 LED, 외부 상태 LED) |
| 서보 모터 (SG-90) | 창문 개폐 |
| ESP-01 (Wi-Fi 모듈) | TCP/IP로 Raspberry Pi와 통신 |

**MCU (Manager Control Unit) 모듈**
| 모듈 이름 | 역할 |
|-----------|------|
| HC-06 | 블루투스 통신 (서버와 통신) |
| LCD 16x2 | GUI 구성, 관리자 컨트롤 |
| 조이스틱 모듈 | 커서 이동, 메뉴 선택 |

**Server (Raspberry Pi) 기능**
| 모듈/기능 | 역할 |
|-----------|------|
| MariaDB | 케이지 상태, 사용자, 제어 기록 저장 |
| Bluetooth 모듈 (내장) | Arduino와 관리자 통신 처리 |
| TCP/IP 소켓 서버 | 사용자 앱 및 각 케이지(CCU)와의 통신 |
