# 🦟 IntelliRisk — IoT 하드웨어 연동형 감염병 위험 예측 플랫폼

> **ESP32, Flask, MySQL, MQTT**를 결합하여 온·습도 등 환경 데이터를 실시간으로 수집·분석하고, 액추에이터 제어까지 연결한 **IoT 풀스택 프로젝트**입니다.

---

## 📌 프로젝트 개요

뎅기를 발생시키는 모기의 서식 환경은 온도 및 습도와 같은 기상 조건에 큰 영향을 받습니다.  
**IntelliRisk**는 엣지 디바이스(ESP32)에서 센서 데이터를 측정하여 MQTT 브로커로 전송하고, Flask 백엔드 서버에서 이를 가공·저장하여 웹 실시간 모니터링 및 원격 제어를 가능하게 하는 통합 시스템입니다.

---

## 💡기능

* **실시간 데이터 수집 및 시각화**: ESP32 센서로부터 전송되는 실시간 환경 데이터를 웹 대시보드에서 즉각 확인
* **데이터 영속성 확보**: 수집된 센서 수치 및 원격 제어 이력을 관계형 데이터베이스(MySQL)에 안전하게 기록 및 분석 기반 마련
* **서버 & 웹**: REST API를 통한 데이터 적재, 뎅기열 위험도 연산 알고리즘 처리, 웹 대시보드를 통한 시각화 및 리포팅.
* **역할 기반 권한 관리 (RBAC)**: 관리자(Admin)와 일반 사용자 간의 웹 접근 권한 분리

---

## 🛠 기술 스택

| 분류 | 기술 스택 |
| :--- | :--- |
| **Hardware / IoT** | ESP32, MQTT Protocol |
| **Backend** | Python, Flask|
| **Database** | MySQL |
| **Frontend** | HTML, CSS, JavaScript  |

---

## 🏗 시스템 구조

```text
[ ESP32 + 센서 ] ──(MQTT Publish)──> [ MQTT Broker ]
                                           │
                                    (MQTT Subscribe)
                                           ▼
[ Web Dashboard ] <─── [ Flask Backend ] ────> [ MySQL DB ]

```

1. **ESP32 + sensors**: 센서 측정값을 MQTT 토픽으로 주기적 발행
2. **MQTT broker**: 메시지 루팅 및 비동기 이벤트 전달
3. **Backend**: 알고리즘 계산, 인메모리 상태 업데이트 및 DB 영속화, 프론트로 정보전달
4. **Database**: 센서 수치, 액추에이터 이벤트, 유저/권한 데이터 관리
5. **Web UI**: 실시간 대시보드 렌더링 및 제어 인터페이스 제공

---
## ⭐ 본인 담당 업무 (Yejin Chung) ⭐

### Flask 기반 RESTful API 아키텍처 설계 및 구현
### 온·습도 수치 기반 뎅기열 위험도 연산 알고리즘 개발
### DB 트랜잭션 최적화 및 API 응답속도 향상 
---

## 📁 코드 구조

`app/` 내부를 역할에 맞게 분리하여 서비스가 확장되더라도 독립적인 수정이 가능하도록 모듈화했습니다.

```text
IntelliRisk-main/
├── app/
│   ├── __init__.py         # Application Factory (create_app) 및 Blueprint/Handler 등록
│   ├── extensions.py       # 인스턴스 공유 (DB SQLAlchemy, LoginManager, MQTT Client 등)
│   ├── web/
│   │   └── routes.py       # 웹 라우팅 (대시보드, 제어 액션)
│   ├── mqtt_handlers.py    # MQTT 연결/메시지 구독, 데이터 영속화 및 상태 갱신
│   ├── error_handlers.py   # 중앙 집중식 HTTP 에러 핸들링
│   └── state.py            # 대시보드 실시간 렌더링용 인메모리 State (dengue_values)
├── controllers/            # 기능별 엔드포인트 모듈 (Auth, Users, Sensors 등)
├── models/                 # SQLAlchemy 데이터베이스 모델 정의
├── views/                  # HTML 템플릿 파일 (Jinja2)
├── static/                 # Frontend 자원 (CSS, JS)
└── docs/                   # DB 스키마 (schema.sql) 및 아키텍처 다이어그램

```

---
## 📸 스크린 샷

### 1. ERD
시스템의 핵심 엔티티(Users, Devices, Sensors, Actuators)와 엔티티 간의 관계(1:N, 상속/개념적 분리)를 정의한 개념 모델입니다.

### 2. 물리적 데이터 베이스 스키마

개념 모델을 바탕으로 실제 RDBMS(MySQL) 테이블, 데이터 타입, 외래키(FK) 관계 및 제약 조건을 반영한 물리적 스키마입니다.
| Conceptual Model (ERD) | Physical Database Schema |
| :---: | :---: |
| ![Conceptual Model](https://github.com/Chungyejin/IntelliRisk/blob/main/docs/diagrams/architecture.jpg)  | ![Physical Schema](https://github.com/Chungyejin/IntelliRisk/blob/main/docs/database/logical_model.png?raw=true)  |

### 3. 시스템 화면
| 화면 1 | 화면 2 |
| :---: | :---: |
| <img src="https://github.com/user-attachments/assets/638825a6-7b35-4d36-8c6a-6f54e289bc61" width="100%"> |<img src="https://github.com/user-attachments/assets/0316fda4-4dcb-4a0d-9499-e9d91f2c8fae" width="100%">|


| 화면 3 | 화면 4 |
| :---: | :---: |
| <img src="https://github.com/user-attachments/assets/61cd375b-c995-4298-864b-a6bbff0a2424" width="100%"> | <img src="https://github.com/user-attachments/assets/e80a4598-8b3c-47c6-9b3b-41142cf1510f" width="100%"> |

| 화면 5 | 화면 6 |
| :---: | :---: |
| <img src="https://github.com/user-attachments/assets/6d0c5161-9b97-46a8-bef3-6ca369ec0bb0" width="100%"> | <img src="https://github.com/user-attachments/assets/18bcc3c1-5de0-4238-be01-ae33d8f90cfe" width="100%"> |


---
## 🌐 주요 엔드포인트

* **`GET /`** : 메인 홈 페이지
* **`GET /login`** : 사용자 인증 페이지
* **`GET /adm`** : 권한 기반 관리자 전용 대시보드 (Admin Role 필요)
* **`GET /publish`** : 원격 데이터 발행 및 액션 페이지 (Role-aware)
* **`GET /tempo_real`** : `dengue_values` (인메모리) 기반의 **실시간 센서 모니터링 대시보드**
* **`POST /publish_message`** : JSON 형태의 `{topic, message}`를 MQTT로 제어 발행 및 DB 저장
* **`POST /desligar_led`** : 액추에이터 토픽으로 LED OFF 제어 명령 전송

---

## 팀원 및 담당 업무

| 이름 | 역할 |
|---|---|
| **Yejin Chung** | **백엔드 개발자** | 
| **Ana Flávia** |  **QA 엔지니어** |
| **Isabella Berkembrock** | **프로젝트 매니저,백앤드 개발자** |
| **Michele Cristina Otta** |  **프론트엔드 개발자**|
