# Aircraft Engine Remaining Useful Life (RUL) Prediction

다변량 센서 데이터를 활용하여 **항공기 엔진의 잔여 수명(Remaining Useful Life, RUL)**을 예측하고  
이를 기반으로 **고장 확률 분석 및 최적 유지보수 의사결정**을 수행하는 머신러닝 프로젝트입니다.

Predictive Maintenance(예지정비) 관점에서 **데이터 기반 유지보수 전략**을 제안하는 것을 목표로 합니다.

---

# Project Overview

항공기 엔진과 같은 고가 장비는 고장이 발생할 경우  
운용 중단 비용과 유지보수 비용이 크게 증가합니다.

따라서 센서 데이터를 활용하여 장비 상태를 분석하고  
**고장 이전에 유지보수를 수행하는 Predictive Maintenance 기술**이 중요합니다.

이 프로젝트에서는 다음을 수행합니다.

- 엔진 센서 데이터를 이용한 **잔여 수명(RUL) 예측 모델 구축**
- **Failure Probability (고장 확률) 추정**
- **정비 시점 결정 정책(Decision Policy)** 설계

---

# Dataset

NASA에서 공개한 **C-MAPSS Turbofan Engine Degradation Simulation Dataset (FD001)**을 사용했습니다.

Dataset 특징

- Engine units : **100**
- Operational settings : **3**
- Sensor measurements : **21**
- Total records : **23,010**

각 엔진은 시간에 따라 열화(degradation)가 진행되며  
모델의 목표는 **고장까지 남은 사이클 수(RUL)**을 예측하는 것입니다.

### Data Structure

각 행은 한 cycle의 엔진 상태를 나타냅니다.

| Column | Description |
|------|-------------|
| unit_number | engine id |
| cycle | operating cycle |
| setting1~3 | operational settings |
| sensor1~21 | sensor measurements |

---

# Target Variable

Remaining Useful Life (RUL)
RUL = max_cycle - current_cycle


각 엔진이 **고장까지 남은 운용 사이클 수**를 의미합니다.

---

# Project Workflow


EDA
↓
Feature Engineering
↓
RUL Prediction Model
↓
Failure Probability Estimation
↓
Maintenance Decision Policy


---

# Exploratory Data Analysis (EDA)

### Engine Life Distribution

- 평균 수명: **206 cycles**
- 최소: **128 cycles**
- 최대: **362 cycles**

엔진 수명은 다양한 범위를 가지며  
**조기 고장과 장기 수명이 혼재된 데이터 특성**을 보입니다.

---

### Sensor Pattern Analysis

센서 패턴을 크게 세 가지로 분류했습니다.

#### 1️⃣ Monotonic Trend Sensors

cycle 증가에 따라 일정한 증가 또는 감소 패턴을 보이는 센서


sensor2
sensor3
sensor4
sensor7
sensor8
sensor11
sensor12
sensor13
sensor15
sensor17
sensor20
sensor21


→ RUL 예측에 중요한 변수

---

#### 2️⃣ Constant Sensors (Removed)

값이 변하지 않는 센서


sensor1
sensor5
sensor10
sensor16
sensor18
sensor19


→ 정보량이 없어 모델에서 제거

---

#### 3️⃣ Irregular Sensors

불규칙한 변동성을 가진 센서


sensor6
sensor9
sensor14


→ 이동 평균 및 통계적 특징으로 활용

---

# Feature Engineering

센서 데이터의 노이즈 제거와 시간 의존성을 반영하기 위해 다음 Feature를 생성했습니다.

- Moving Average (MA)
- Rolling Standard Deviation
- Lag Features
- Difference Features

예시


sensor4_ma5
sensor11_ma5
sensor9_lag1
sensor14_lag5


---

# RUL Prediction Model

사용 모델

- Random Forest
- XGBoost

모델 학습 목표


Input : sensor features
Output : Remaining Useful Life (RUL)


모델 평가 지표

- RMSE

---

# Failure Probability

RUL 예측 결과를 기반으로 **고장 확률 곡선(Failure Probability)**을 계산합니다.

목적

- 단순 RUL 예측 → **확률 기반 유지보수 의사결정**

예


P(Failure | time)


---

# Decision Policy

Failure Probability를 활용하여 **최적 정비 시점**을 결정합니다.

전략 예시


If Failure Probability > Threshold
→ Preventive Maintenance


목표

- 다운타임 최소화
- 유지보수 비용 최적화

---
# Tech Stack

- Python
- Pandas
- NumPy
- Scikit-learn
- XGBoost
- Matplotlib
- Seaborn

---

# Reference

NASA Prognostics Data Repository  

Damage Propagation Modeling for Aircraft Engine Run-to-Failure Simulation  
PHM 2008 Conference


---
상세 분석 내용은 분석보고서를 참고해주세요.

[📄분석보고서](./분석보고서ppt.pdf)
