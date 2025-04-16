---
title: "[AI TIL] LangChain과 RAG 구성 전체 흐름 정리"
date: 2025-04-14 09:00:00 +0900
categories: [AI, RAG, LangChain]
tags: [RAG, LangChain, Embedding, VectorSearch, HuggingFace, 생성형AI]
---

# ✅ AI 서비스 설계 스터디 – 2일차 통합 정리

## 🎯 오늘의 목표

> GPT 기반 RAG 구조에서 Embedding, 검색, 생성까지의 전체 흐름을 이해하고,  
> LangChain, Embedding API, OpenAI API, HuggingFace의 역할을 명확히 구분한다.

---

## 🧩 핵심 등장인물 요약

| 구성 요소         | 역할                                 | 예시                                       |
| ----------------- | ------------------------------------ | ------------------------------------------ |
| **LangChain**     | 전체 AI 파이프라인 자동화 프레임워크 | DocumentLoader → Retriever → LLMChain      |
| **Embedding API** | 문서나 질문을 벡터로 변환            | OpenAI, HuggingFace                        |
| **Vector Store**  | 벡터를 저장하고 유사한 벡터를 검색   | FAISS, Chroma                              |
| **OpenAI API**    | GPT 모델을 호출해 응답 생성          | GPT-4, GPT-3.5                             |
| **Hugging Face**  | 다양한 공개 AI 모델 허브             | `sentence-transformers`, `transformers` 등 |

---

# 🧠 전체 흐름 한 문장 요약

> **RAG는 질문을 벡터화해서 관련 문서를 검색하고, 해당 문서를 LLM에게 텍스트로 전달해 응답을 생성하는 구조다.**

---

## 🔁 전체 시스템 구성 흐름도 요약

![image.png](/_posts/img/250414img0.png)

---

## 1️⃣ 문서 임베딩 및 저장 (Embedding Phase)

![image.png](/_posts/img/250414img1.png)

- 사용자가 업로드한 문서들을 벡터화해서 벡터DB에 저장
- 의미 기반 유사도 검색을 가능하게 만드는 전처리 단계

📦 사용하는 요소

- Embedding API (OpenAI, HuggingFace)
- Vector Store (FAISS, Chroma)

---

## 2️⃣ 질문 임베딩 및 유사 문서 검색 (Retrieval Phase)

![image.png](/_posts/img/250414img2.png)

- 사용자의 질문도 벡터화
- 질문 벡터와 가장 유사한 문서 벡터를 비교해 원문 텍스트를 꺼냄

📦 사용하는 요소

- Embedding API
- Vector Store (`similarity_search`)

---

## 3️⃣ 문서 + 질문으로 응답 생성 (Generation Phase)

![image.png](/_posts/img/250414img3.png)

- 검색된 문서 텍스트와 질문을 LLM에게 함께 전달
- LLM이 문맥 기반 자연어 응답 생성

📦 사용하는 요소

- OpenAI Chat API
- LangChain의 `RetrievalQA`, `LLMChain` 등

---

## 🌍 참고: Hugging Face란?

![image.png](/_posts/img/250414img4.png)

- 다양한 오픈소스 AI 모델의 마켓플레이스이자 백과사전
- GPT 없이도 임베딩, 분류, 요약 등 가능
- LangChain과도 호환됨

---

# ✅ 마무리 정리

LangChain은 이 모든 구성요소(Embedding, 검색, 생성)를 파이썬 코드로  
**하나의 체인처럼 연결해서** AI 서비스를 쉽게 만들 수 있게 해준다.
