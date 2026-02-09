# NLP Learning Project - Phonetic Translation Error Analysis

발음 유사 오류가 포함된 한국어 문장의 영어 번역에서 교정 단계의 효과를 검증하는 실험 프로젝트

## 📋 프로젝트 개요

음성 인식(STT) 시스템에서 발생하는 발음 유사 오류(예: "결제"→"결재", "매일"→"메일")가 번역 품질에 미치는 영향을 분석하고, 다양한 교정 전략의 효과를 비교 평가합니다.

### 연구 질문
> 발음 유사 오류가 포함된 문장을 번역할 때, 교정 단계를 추가하는 것이 번역 품질을 향상시키는가?


## 🏗️ 프로젝트 구조

```
Project/
├── data/                          # 실험 데이터
│   ├── clean_sentences.txt        # 원본 정확한 문장
│   ├── noisy_sentences.txt        # 발음 오류 포함 문장
│   └── answer_sentences.txt       # 정답 영어 번역
├── prompts/                       # 실험용 프롬프트
│   ├── baseline.txt               # 단순 번역
│   ├── phonetic_aware.txt         # 오류 인지 번역 (One-Stage)
│   └── correction.txt             # 교정 전용 (Two-Stage 1단계)
├── results/                       # 실험 결과 및 시각화
├── Project.ipynb                  # 메인 실험 노트북
└── README.md
```

## 🔬 실험 방법론

### 3가지 번역 접근법

1. **Baseline**: 오류 교정 없이 직접 번역
   - 프롬프트: "한국어를 영어로 번역하세요"
   - 특징: 단순하지만 LLM의 문맥 이해 능력 활용

2. **One-Stage**: 오류 교정과 번역을 동시 수행
   - 프롬프트: "발음 유사 오류를 고려하여 번역하세요"
   - 특징: 1회 LLM 호출, 효율적이나 복잡도 증가

3. **Two-Stage**: 교정 후 번역 (2단계 파이프라인)
   - 1단계: 발음 오류 교정 (한국어→한국어)
   - 2단계: 교정된 문장 번역 (한국어→영어)
   - 특징: 명확한 역할 분리, but 오류 누적 가능성

### 평가 지표
- **BLEU Score**: n-gram 정밀도 기반 번역 품질
- **METEOR Score**: 동의어/어간 고려한 의미적 유사도
- **Exact Match**: 정답과의 완전 일치율
- **Case-by-case Analysis**: 케이스별 성능 변화 추적

## 🛠️ 기술 스택

- **LLM Engine**: Ollama (로컬 실행)
- **Model**: llama3.1:8b
- **Framework**: LangChain
- **Evaluation**: sacrebleu, NLTK (METEOR)
- **Visualization**: Matplotlib, Pandas

## 🚀 실행 방법

### 1. 환경 설정
```bash
# 가상환경 생성 및 활성화
python -m venv .venv
.venv\Scripts\activate  # Windows
source .venv/bin/activate  # Linux/Mac

# 패키지 설치
pip install langchain langchain-ollama langchain-core sacrebleu nltk matplotlib pandas
```

### 2. Ollama 설치 및 모델 다운로드
```bash
# Ollama 설치 (https://ollama.com/)
ollama serve  # 서버 시작

# 모델 다운로드
ollama pull llama3.1:8b
```

### 3. 실험 실행
`Project.ipynb`를 열고 순차적으로 셀을 실행합니다.

### 향후 연구 방향
1. 더 강력한 모델(GPT-4, Claude 등)에서의 재실험
2. 단일 문장이 아닌 연속되는 문장들의 문맥을 고려하여 재실험
3. STT의 출력에서 Confidence와 같은 출력을 고려하여 재실험

**Note**: 이 프로젝트는 교육 및 연구 목적으로 제작되었습니다.
