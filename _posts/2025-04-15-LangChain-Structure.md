---
title: "[AI TIL] 3일차 – LangChain 구성과 체인 구조 정리"
date: 2025-04-15 09:00:00 +0900
categories: [AI, RAG, LangChain]
tags: [LangChain, Chain, Embedding, GPT, Retriever, 생성형AI]
---

# ✅ AI 서비스 설계 스터디 – 3일차 정리

## 🎯 오늘의 목표

> LangChain의 구성요소와 체인 구조를 이해하고,  
> 다양한 사용 사례에 맞는 체인을 직접 선택·구성할 수 있도록 한다.

---

## 1️⃣ LangChain의 역할

LangChain은 다양한 AI 컴포넌트(GPT, VectorStore, Embedding 등)를  
**코드 체인 형태로 조립해서 하나의 응답 흐름을 구성**해주는 프레임워크이다.

---

## 2️⃣ LangChain 핵심 구성요소

| 구성 요소        | 역할               | 예시                |
| ---------------- | ------------------ | ------------------- |
| `DocumentLoader` | 문서 불러오기      | PDF, txt, 웹        |
| `TextSplitter`   | 문서 분할          | 문단, 토큰 기준     |
| `Embeddings`     | 벡터화             | OpenAI, HuggingFace |
| `VectorStore`    | 벡터 저장 및 검색  | FAISS, Chroma       |
| `Retriever`      | 유사 문서 검색     | similarity_search() |
| `PromptTemplate` | GPT 입력 포맷 구성 | 질문 + 문서         |
| `LLM`            | 생성 모델 호출     | GPT-3.5, GPT-4      |
| `Chain`          | 전체 흐름 조립     | RetrievalQA 등      |

🖼️ **[그림 1 삽입 예정: LangChain 파이프라인 구성 흐름도]**

---

## 3️⃣ 대표 체인 구조 비교

```
[RetrievalQA]
질문 → 문서 검색 → 응답 생성

[ConversationalRetrievalQA]
(대화 맥락 포함) 질문 → 문서 검색 → 응답 생성

[MapReduceDocumentsChain]
문서 분할 요약(Map) → 전체 요약(Reduce)
```

- RetrievalQA: 가장 기본적인 체인
- Conversational: 챗봇 인터페이스와 대화 유지
- MapReduce: 대량 문서 요약에 적합

🖼️ **[그림 2 삽입 예정: 체인 구조 비교 요약도]**

---

## ✅ 요약

LangChain은 GPT를 직접 호출하는 것보다  
**검색, 문서 처리, 응답 생성의 전체 흐름을 자동화**할 수 있게 해주는 프레임워크이다.  
각 상황에 따라 체인을 적절히 선택하여 구성할 수 있다.

---

## 📅 다음 예고

- 4~5일차: MCP 구조와 AI Researcher 구조 비교
- 6~8일차: 실습 기반 RAG 시스템 직접 구현해보기
