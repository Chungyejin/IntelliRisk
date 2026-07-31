# 🦟 IntelliRisk — Dengue Risk Monitoring (ESP32 + Flask + MySQL + MQTT)

> **ESP32, Flask, MySQL, MQTT**를 결합하여 온·습도 등 환경 데이터를 실시간으로 수집·분석하고, 액추에이터 제어까지 연결한 **End-to-End IoT 풀스택 프로젝트**입니다.

---

## 📌 Project Overview (프로젝트 개요)

뎅기열 매개 모기의 서식 환경은 온도 및 습도와 같은 기상 조건에 큰 영향을 받습니다.  
**IntelliRisk**는 엣지 디바이스(ESP32)에서 센서 데이터를 측정하여 MQTT 브로커로 전송하고, Flask 백엔드 서버에서 이를 가공·저장하여 웹 실시간 모니터링 및 원격 제어를 가능하게 하는 통합 시스템입니다.

본 프로젝트는 확장성과 유지보수성을 고려하여 Flask **Application Factory Pattern** 및 **모듈화 아키텍처**를 적용하여 설계되었습니다.

---

## 💡 What Problem Does It Solve? (해결하려는 문제)

* **실시간 데이터 수집 및 시각화**: ESP32 센서로부터 전송되는 실시간 환경 데이터를 웹 대시보드에서 즉각 확인
* **데이터 영속성 확보**: 수집된 센서 수치 및 원격 제어 이력을 관계형 데이터베이스(MySQL)에 안전하게 기록 및 분석 기반 마련
* **양방향 제어 (Bi-directional Control)**: 웹 UI에서 MQTT 토픽을 발행(Publish)하여 현장의 LED/액추에이터를 원격으로 제어
* **역할 기반 권한 관리 (RBAC)**: 관리자(Admin)와 일반 사용자 간의 웹 접근 권한 분리

---

## 🛠 Tech Stack (기술 스택)

| 분류 | 기술 스택 |
| :--- | :--- |
| **Hardware / IoT** | ESP32, MQTT Protocol |
| **Backend** | Python 3.x, Flask (Application Factory), SQLAlchemy |
| **Database** | MySQL |
| **Frontend** | HTML5, CSS3, JavaScript (Jinja2 Template Engine) |
| **Architecture** | Event-Driven Architecture (via MQTT), Role-Based Access Control |

---

## 🏗 System Architecture (시스템 구조)

```text
[ ESP32 + 센서 ] ──(MQTT Publish)──> [ MQTT Broker ]
                                           │
                                    (MQTT Subscribe)
                                           ▼
[ Web Dashboard ] <──(Jinja2/State)── [ Flask Backend ] ──(ORM)──> [ MySQL DB ]

```

1. **ESP32 + sensors**: 센서 측정값을 MQTT 토픽으로 주기적 발행
2. **MQTT broker**: 메시지 루팅 및 비동기 이벤트 전달
3. **Flask backend**: 토픽 구독(Subscribe), 페이로드 디코딩, 인메모리 상태 업데이트(`state.py`) 및 DB 영속화
4. **Database (MySQL via SQLAlchemy)**: 센서 수치, 액추에이터 이벤트, 유저/권한 데이터 관리
5. **Web UI**: 실시간 대시보드 렌더링 및 제어 인터페이스 제공

---

## 📁 Application Structure (코드 구조)

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

## 🌐 Web Routes (주요 엔드포인트)

* **`GET /`** : 메인 홈 페이지
* **`GET /login`** : 사용자 인증 페이지
* **`GET /adm`** : 권한 기반 관리자 전용 대시보드 (Admin Role 필요)
* **`GET /publish`** : 원격 데이터 발행 및 액션 페이지 (Role-aware)
* **`GET /tempo_real`** : `dengue_values` (인메모리) 기반의 **실시간 센서 모니터링 대시보드**
* **`POST /publish_message`** : JSON 형태의 `{topic, message}`를 MQTT로 제어 발행 및 DB 저장
* **`POST /desligar_led`** : 액추에이터 토픽으로 LED OFF 제어 명령 전송

---

## 📡 MQTT Topics & Data Model

### MQTT Topics (예시)

* `dengue1`, `dengue2`, `dengue3`: 각 구역별 센서 수치 수집 토픽
* `topico_led`: 원격 액추에이터/LED 상태 제어 토픽

### Data Model (영속성 관리)

* **Sensor Readings**: 토픽명, 타임스탬프, 측정값, 메타데이터 저장
* **Actuator Events**: 토픽명, 제어 일시, 제어 명령 내역 기록
* **Users & Roles**: 역할 기반 권한 제어(RBAC)를 위한 사용자 및 권한 데이터

---

## 🔒 Security & Configuration Notes

* **환경변수 분리**: DB 접속 정보 및 비밀키(Secret Key) 등 민감 정보는 소스코드 외부에 환경변수로 관리
* **중앙화된 에러 처리**: `error_handlers.py`를 통해 예외 상황에서도 서버 중단 없이 일관된 에러 응답 제공
* **안전한 DB 스키마 관리**: 초기화 시 파괴적인 쿼리(`DROP TABLE`)를 지양하고 멱등성을 지닌 데이터베이스 세팅 준수

```

```
