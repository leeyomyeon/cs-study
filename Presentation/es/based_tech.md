# ES가 데이터를 삽입할 때 방식

```
삽입(Ingestion) 과정
 ├── Analysis (텍스트 분석)
 │    ├── Character Filters (문자 필터)
 │    ├── Tokenizer (토큰화)
 │    ├── Token Filters (토큰 필터)
 │    └── Term(검색 가능한 단위) 생성
 ├── Inverted Index (역색인 구축)
```

## Analysis (텍스트 분석)

입력된 문서를 처리하여 Term(검색 가능한 단어 단위)를 생성하는 과정.
Analyzer를 통해 Character Filters, Tokenizer, Token Filters 순으로 변환됨.

### Tokenizer (토큰화)

텍스트를 단어 또는 문자 단위로 나누는 과정.

예: "I love Elasticsearch" → ["I", "love", "Elasticsearch"]

### Filters (토큰 필터)

불필요한 단어 제거(Stopwords), 소문자 변환(Lowercase), 동의어 처리(Synonym), 어근 추출(Stemming) 등의 작업 수행.

### Inverted Index (역색인 구축)

생성된 Term을 기반으로 문서-단어 매핑 정보를 저장.

빠른 검색을 위해 사용됨.

## 2. BM25 (Best Matching 25)

Elasticsearch는 문서와 검색어 간 **유사도 점수**를 계산하기 위해 **BM25(Best Matching 25)** 알고리즘을 사용한다.

### 2-1. BM25의 공식

```math
BM25(D, Q) = \sum_{t \in Q} IDF(t) \cdot \frac{TF(t, D) \cdot (k_1 + 1)}{TF(t, D) + k_1 \cdot (1 - b + b \cdot \frac{|D|}{\text{avgD}})}
```

### 주요 개념

- **TF(Term Frequency)**: 단어의 등장 횟수가 많을수록 점수 증가
- **IDF(Inverse Document Frequency)**: 흔한 단어는 점수를 낮게 조정
- **문서 길이 보정**: 문서가 길어질수록 TF 영향 감소

### IDF 계산식

```math
IDF(t) = \log\left(\frac{N - DF(t) + 0.5}{DF(t) + 0.5} + 1\right)
```

- **N**: 전체 문서 수
- **DF(t)**: 특정 단어 t가 등장한 문서 수

---

## 3. Elasticsearch에서 BM25 적용 방식

Elasticsearch는 기본적으로 **BM25를 사용하여 검색 점수를 계산**한다.

### BM25 적용 예시

```json
{
  "query": {
    "match": {
      "content": "Elasticsearch tutorial"
    }
  }
}
```

검색어 "Elasticsearch tutorial"이 많이 등장하고 IDF가 높은 문서가 상위 노출됨.

---

## 결론

1. Elasticsearch는 **토큰화 → 역색인 → BM25 점수 계산** 과정으로 검색 성능을 최적화한다.
2. BM25는 **TF, IDF, 문서 길이 보정**을 고려하여 **가장 관련성 높은 문서를 검색 결과 상단에 배치**한다.
3. 이러한 원리를 활용하면 **효율적인 검색 시스템을 구축**할 수 있다
