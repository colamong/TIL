---
title: "[AI TIL] 4일차 – RAG의 한계와 보완 전략 정리"
date: 2025-04-16 08:49:00 +0900
categories: [AI, RAG, VectorSearch]
tags:
  [
    RAG,
    Prompt-Engineering,
    파인튜닝,
    하이브리드검색,
    다단계검색,
    지식그래프,
    캐싱,
    Gen AI,
  ]
---

# ✅ AI 서비스 설계 스터디 – 4일차 정리

## 🎯 오늘의 주제

> Retrieval-Augmented Generation(RAG)의 단점들을 파악하고,  
> 이를 보완하는 다양한 전략들을 학습하며 실제 서비스에 어떻게 적용할 수 있을지 분석한다.

---

## 1️⃣ RAG의 핵심 구조와 한계

- 외부 지식을 검색한 뒤, GPT에 함께 전달하여 응답 생성
- 강점: 최신성, 유연한 응답, 컨텍스트 반영
- 한계:
  - 검색 실패 (문맥-문서 불일치)
  - 문서 통합 해석 어려움
  - 프롬프트 품질 민감
  - 토큰 한계, 응답 일관성 부족

---

## 2️⃣ 보완 전략 요약

| 전략                     | 핵심 목적           | 키워드                     |
| ------------------------ | ------------------- | -------------------------- |
| 하이브리드 검색          | 검색 정확도 향상    | 키워드 + 벡터 병합         |
| 질문 분해 생성           | 복잡 질문 분할 대응 | Sub-Query, Progressive QA  |
| 다단계 검색 & Re-ranking | 검색 정밀도 향상    | Re-ranker 모델 사용        |
| 지식 그래프 기반 검색    | 개체 관계 기반 추론 | NER, RE, KG                |
| 프롬프트 vs 파인튜닝     | 생성 품질 향상      | 빠른 실험 vs 도메인 최적화 |
| 캐싱 & 재사용            | 속도 및 비용 최적화 | Redis, 히스토리 기반       |

---

## 3️⃣ RAG 보완 전략 상세 정리

---

### ✅ ① 하이브리드 검색

**설명:**  
키워드 검색(BM25)과 벡터 검색(의미 기반)을 병합하여 검색 정확도를 향상시킴.  
의미는 비슷하지만 단어가 일치하지 않거나, 날짜·숫자 등 키워드 중요성이 클 때 강력함.

**장점:**

- 유사도 + 정확한 단어 일치 → 검색 실패 확률 낮춤
- 키워드 기반 정확성 보완

**단점:**

- 두 검색기 호출로 인한 속도 저하
- 결과 병합·정렬 기준 설정 필요

**적용 사례:**

- 뉴스 큐레이션 시스템 (정확한 키워드 + 관련 내용)
- 지식형 챗봇 (고정 질문 + 유사한 표현 다루기)

---

### ✅ ② 질문 분해 & 점진적 생성

**설명:**  
복잡하거나 다단계적 질문을 Sub-question으로 나눠 단계적으로 처리하고 최종 응답 생성.  
Chain-of-Thought 기반 reasoning 또는 Plan-first Agent 구조의 기반이 되는 전략.

**장점:**

- 복잡한 질문, 비교 질문 등 처리 가능
- 논리적 추론이 필요한 시나리오에 강력함

**단점:**

- 속도 느림 (질문을 나누고 순차 처리)
- 응답 통합 품질 관리 필요

**적용 사례:**

- 복합 기획 요청 (여행 일정 설계)
- Claude/ChatGPT의 Long-context 질의 응답
- 연구 보조용 AI

---

### ✅ ③ 다단계 검색 & Re-ranking

**설명:**  
벡터 검색으로 Top-N 문서를 먼저 찾고, 별도의 Re-ranker 모델(BERT 등)로 질문-문서 적합도를 정밀 계산하여 문서를 재정렬.

**장점:**

- GPT에 전달될 문서 품질 극대화
- 정보 누락 가능성 ↓, 답변 적합도 ↑

**단점:**

- 속도 저하 (모델 추가 호출)
- 랭커 모델 별도 학습 필요

**적용 사례:**

- 논문 추천 시스템
- 금융/법률 분야의 정확 응답 필요 시스템
- AI 기반 상담 서비스

---

### ✅ ④ 지식 그래프 기반 검색

**설명:**  
문서에서 개체(Entity)와 관계(Relation)를 추출해 지식 그래프로 구성.  
질문을 그래프에서 탐색하여 정확한 관계 기반 응답 도출.

**장점:**

- 개체 간 관계 기반 응답 가능
- 시계열/구조화 정보에 탁월

**단점:**

- 구축 복잡 (NER/RE 전처리, 그래프 구축)
- 실시간 변화 대응 어려움

**적용 사례:**

- 네이버 QnA (서비스/회사 관계)
- 제약사 정보 검색 (성분-약물-효능)
- 법률 문서 개체 연결 기반 검색

---

### ✅ ⑤ 프롬프트 엔지니어링 vs 파인튜닝

**설명:**  
GPT의 출력 품질 향상을 위해 프롬프트를 최적화하거나, 모델 자체를 도메인에 맞게 학습(Fine-tune)하는 전략.

**장점:**

| 항목         | 프롬프트 | 파인튜닝 |
| ------------ | -------- | -------- |
| 접근성       | 쉬움     | 어려움   |
| 실험 속도    | 빠름     | 느림     |
| 커스터마이징 | 낮음     | 강력     |
| 비용         | 없음     | 높음     |

**단점:**

- 프롬프트: 불안정성 존재, 복잡한 작업에 한계
- 파인튜닝: 유지보수 부담, 데이터 필요

**적용 사례:**

- MVP/스타트업 → 프롬프트
- 의료/법률/전문 분야 → 파인튜닝
- 대화형 FAQ 시스템 → 프롬프트

**📽️ 추천 영상**

해당 부분에 대해 잘 정리되어 있는 영상 시청을 추천함.

[RAG vs Prompting vs Fine-tuning – AssemblyAI 영상](https://www.youtube.com/watch?v=zYGDpG-pTho)

<iframe width="840" height="473" 
  src="https://www.youtube.com/embed/zYGDpG-pTho" 
  title="RAG vs Prompting vs Fine-tuning (AssemblyAI)" 
  frameborder="0" 
  allowfullscreen></iframe>

---

### ✅ ⑥ 캐싱 & 응답 재사용

**설명:**  
자주 묻는 질문이나 동일 검색 결과에 대한 GPT 응답을 저장 후 재사용하여 비용과 응답 속도를 줄이는 전략.

**장점:**

- 속도 빠름
- 비용 절감, 서버 부하 감소

**단점:**

- 정보 신선도 ↓ (캐시 오래된 경우)
- 사용자 개인화엔 약함

**적용 사례:**

- 고객센터 챗봇 (FAQ 자동화)
- 공공 정보 안내 시스템
- 전자상거래의 기본 응답 캐싱

---

## ✅ 결론

> “RAG는 완벽하지 않다. 그러나 다양한 전략을 조합하면  
> 훨씬 더 똑똑하고 정밀한 응답이 가능한 AI 시스템으로 진화할 수 있다.”

---

## 📅 다음 예고

**5일차 – Claude 기반의 Multistep Complex Planning(MCP)**  
AI가 스스로 문제를 쪼개고 계획하여 해결하는 Agent 구조 학습
