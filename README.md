# ML-IDS: CIC-IDS-2017 네트워크 침입 탐지 실험
> **2-stage LightGBM 및 머신러닝 알고리즘을 활용한 네트워크 패킷 기반 다중 공격 분류 시스템**

본 프로젝트는 네트워크 보안 분야의 벤치마크 데이터셋인 **CIC-IDS-2017**을 활용하여, 정상 트래픽과 다양한 사이버 공격 트래픽을 분류하는 **머신러닝 기반 침입 탐지 시스템(IDS)** 연구 실험 공간입니다. 성능 최적화를 위해 시계열 분할 및 Sliding-window 기반의 윈도우 집계 피처 엔지니어링을 적용했습니다.

## 🚀 Quick Start (코드 보기)
GitHub 자체 뷰어에서 `.ipynb` 파일 렌더링 에러가 발생할 경우, 아래 링크를 통해 Google Colab에서 즉시 코드를 확인하실 수 있습니다.
- [👉 Google Colab에서 실험 결과 노트 보기](https://colab.research.google.com/github/dain77-d/ML-IDS/blob/main/cicids2017_experiment_result2.ipynb)

---

## 📊 프로젝트 아키텍처 & 주요 기능

### 1. Advanced Feature Engineering (피처 엔지니어링)
* **Base Features (77개):** 원본 PCAP 패킷에서 추출된 흐름(Flow) 기반 기본 네트워크 피처[cite: 1].
* **Window Aggregate Features (25개):** 데이터 누수(Data Leakage)를 엄격히 방지하기 위해 **과거 데이터만을 활용한 Sliding-window(10초/60초)** 기반 윈도우 집계 피처 자체 구축 (출발지-목적지 IP/포트별 엔트로피, 연결 비율 등)[cite: 1].
* **Total Features:** 총 102개의 고차원 피처 가공을 통해 탐지 성능을 극대화[cite: 1].

### 2. 엄격한 데이터 분할 알고리즘
* 네트워크 트래픽 특성을 반영하여 타임스탬프(Timestamp) 기준의 **시계열 분할(Temporal Split)** 적용[cite: 1].
* 데이터셋을 **Train(70%) / Validation(15%) / Test(15%)**로 엄격히 분할하여 미래 예측에 대한 모델의 일반화 성능 검증[cite: 1].

### 3. 데이터 불균형 해소 (Data Imbalance)
* 소수 공격 클래스(Infiltration, Heartbleed 등)의 학습을 위해 **Capped SMOTE** 및 RandomOverSampler 알고리즘 적용[cite: 1].

---

## 🤖 실험 및 비교 대상 모델 (총 5개 모델 학습)
본 실험에서는 제안하는 **2-Stage LightGBM** 모델을 포함하여 총 5가지 모델의 탐지 성능을 벤치마크 테스트했습니다[cite: 1].

1. **2-stage LightGBM (제안 모델):**[cite: 1]
   - **1단계 (Binary Stage):** 정상(Benign) 트래픽과 악성 공격 트래픽 분류[cite: 1].
   - **2단계 (Multiclass Stage):** 1단계에서 악성으로 분류된 패킷을 대상으로 구체적인 공격 유형(14개 클래스)을 다중 분류[cite: 1].
2. **1-stage LightGBM:** 정상 및 14개 공격 유형을 한 번에 분류하는 15-Class 단일 모델[cite: 1].
3. **Random Forest:** 데이터 불균형을 고려한 Balanced Subsample 가중치 기반 모델[cite: 1].
4. **Logistic Regression:** StandardScaler 전처리를 거친 전통적 선형 분류 모델[cite: 1].
5. **2-stage + capped SMOTE LightGBM:** 오버샘플링 기술을 결합한 2단계 LightGBM 모델[cite: 1].

---

## 🔬 탐지 및 분류 대상 공격 유형 (총 14종)
* **DoS/DDoS 계열:** DoS Hulk, DDoS, DoS GoldenEye, DoS slowloris, DoS Slowhttptest[cite: 1]
* **무단 침입 및 스캔:** PortScan, FTP-Patator, SSH-Patator, Infiltration, Bot[cite: 1]
* **웹 공격 및 취약점:** Web Attack (Brute Force, XSS, Sql Injection), Heartbleed[cite: 1]

---

## 🛠 기술 스택 및 라이브러리
* **Language:** Python
* **ML Frameworks:** `lightgbm`, `scikit-learn`, `imbalanced-learn` (SMOTE)[cite: 1]
* **Data Science:** `pandas`, `numpy`, `scipy`[cite: 1]
* **Visualization:** `matplotlib`, `seaborn`, `shap` (모델 설명력 검증용)[cite: 1]
* **Environment:** Virtual Environment (`.venv`), Jupyter Notebook / Google Colab[cite: 1]

---

## 📁 모델 및 데이터 가공 아티팩트
실험이 완료된 모델 가공품(Bundle) 및 스케일러는 재사용 및 프로덕션 환경 서빙이 가능하도록 `.pkl` 형태로 내보내기 및 저장을 지원합니다[cite: 1].
* `artifacts_cicids2017/cicids2017_model_bundle.pkl`[cite: 1]
