이 프로젝트는 여러 언어로 제공됩니다.
- [🇺🇸 English](README.md)
- [🇧🇷 Português](README.pt.md)

---

<h1> IntelliRisk: 스마트 감염병 예보 플랫폼</h1>

**IntelliRisk**는 IoT 하드웨어 시스템과 웹 플랫폼을 연동하여 특정 지역의 뎅기열 유행 위험을 실시간으로 지능형 모니터링하는 스마트 감염병 예보 플랫폼입니다. 

엣지 디바이스(ESP32)단에서 수집된 환경 데이터(온도·습도)를 분석하여 감염병 발생 가능성을 예측하고, 이를 데이터베이스화하여 웹 대시보드로 시각화합니다. 이와 동시에 엣지 단의 물리 액추에이터(LED, LCD)를 제어하여 로컬 환경에서도 직관적인 경고 정보를 동기화하는 엔드투엔드(End-to-End) 시스템입니다.

---

##  하드웨어 구성 및 기술 스택 (System Components)

* **하드웨어 및 센서 (Hardware & Sensors):** `ESP32` 메인 보드, `DHT11` 온습도 센서
* **액추에이터 (Actuators):** 3색 경고 `LED`, 실시간 스태터스 표시용 `LCD 디스플레이`
* **백엔드 아키텍처 (Backend):** `Python (Flask)` 기반 REST API 서버
* **데이터 스토리지 (Database):** `MySQL` (시계열 환경 데이터 및 리스크 로그 저장)

---

##  핵심 기능 및 데이터 파이프라인 (Key Workflows)

### 1. 실시간 데이터 수집 및 수신 (Data Ingestion)
* ESP32에 탑재된 DHT11 센서가 주기적으로 온습도 데이터를 정밀 측정합니다.
* 수집된 물리 데이터는 Wi-Fi 모듈을 통해 Flask 백엔드 서버의 REST API 엔드포인트로 전송됩니다.

### 2. 리스크 분석 및 데이터베이스화 (Analysis & Storage)
* Flask 서버는 수신된 데이터를 바탕으로 뎅기열 발생 위험 알고리즘을 연산합니다.
* 연산된 리스크 등급 정보는 수집 시간 로그와 함께 MySQL DB에 안정적으로 적재(Insert)됩니다.

### 3. 실시간 양방향 피드백 (Hardware & Web Sync)
* **하드웨어 제어:** 위험도 결과에 따라 ESP32와 연결된 LCD 창의 텍스트가 가변 업데이트되며, 위험 등급별 경고 LED가 실시간으로 토글됩니다.
* **웹 모니터링 대시보드:** 관리자 및 사용자는 웹 아키텍처를 통해 지역별 온습도 추이와 뎅기열 유행 위험도 통계를 한눈에 시각적으로 파악할 수 있습니다.

---

## 대시보드 및 시스템 스크린샷

<table width="100%">
  <tr>
    <td width="50%"><img src="https://github.com/user-attachments/assets/638825a6-7b35-4d36-8c6a-6f54e289bc61" width="100%"/></td>
    <td width="50%"><img src="https://github.com/user-attachments/assets/0316fda4-4dcb-4a0d-9499-e9d91f2c8fae" width="100%"/></td>
  </tr>
  <tr>
    <td width="50%"><img src="https://github.com/user-attachments/assets/61cd375b-c995-4298-864b-a6bbff0a2424" width="100%"/></td>
    <td width="50%"><img src="https://github.com/user-attachments/assets/18bcc3c1-5de0-4238-be01-ae33d8f90cfe" width="100%"/></td>
  </tr>
  <tr>
    <td width="50%"><img src="https://github.com/user-attachments/assets/653192c4-7066-4d67-b99a-13b575ae5b11" width="100%"/></td>
    <td width="50%"><img src="https://github.com/user-attachments/assets/57bde822-8a70-4d15-8af4-6a15fda5df34" width="100%"/></td>
  </tr>
  <tr>
    <td width="50%"><img src="https://github.com/user-attachments/assets/e80a4598-8b3c-47c6-9b3b-41142cf1510f" width="100%"/></td>
    <td width="50%"><img src="https://github.com/user-attachments/assets/6d0c5161-9b97-46a8-bef3-6ca369ec0bb0" width="100%"/></td>
  </tr>
</table>

---

##  팀원 및 역할

**Yejin Chung** <br>
**Ana Flávia** 
**Isabella Berkembrock**
**Michele Cristina Otta** 
