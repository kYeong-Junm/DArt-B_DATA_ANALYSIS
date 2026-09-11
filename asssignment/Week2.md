# 데이터분석 2주차 정규과제

📌데이터분석 정규과제는 매주 정해진 분량의 『*혼자 공부하는 데이터 분석 with 파이썬*』 을 읽고 학습하는 것입니다. 이번 주는 아래의 **DataAnalysis_2nd_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=s_-VvTLb3gs&list=PLVsNizTWUw7FGzSRCkQrPEEe-ljVXgS7k&index=4
https://www.youtube.com/watch?v=Il6L8OtNFpc&list=PLVsNizTWUw7FGzSRCkQrPEEe-ljVXgS7k&index=5
-->


## DataAnalysis_2nd_TIL

### 2장 데이터 수집하기
#### 01. API 사용하기
#### 02. 웹 스크래핑 사용하기


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~81    | ✅         |
| 2주차 | p.84~151   | ✅         |
| 3주차 | p.154~219  | 🍽️         |
| 4주차 | p.222~279 | 🍽️         |
| 5주차 | p.282~325 | 🍽️         |
| 6주차 | p.328~379 | 🍽️         |
| 7주차 | p.382~430 | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->


# 1️⃣ 개념 정리 

## 01. API 사용하기

# 2장-1. API 사용하기

핵심 키워드: `API` `HTTP` `JSON` `XML`

## API

**API(Application Programming Interface)**: 두 프로그램이 서로 대화하기 위한 규칙.

```
프로그램 A  ──── 데이터 요청 ────▶  프로그램 B
프로그램 A  ◀─── 데이터 전송 ────  프로그램 B
```

- 데이터베이스 직접 접근이 어렵거나, 반복적으로 최신 데이터를 받아야 할 때 사용

---

## HTTP와 웹 기반 API

- 웹 서버 ↔ 웹 브라우저는 **HTTP**로 통신하며, 이때 주고받는 문서가 **HTML**
- HTML: 마크업 언어라고 부르며, `<div>` 같은 태그로 구성. 다만, 구조가 복잡해 프로그램 간 데이터 교환에는 부적합

**웹 기반 API**는 같은 HTTP 프로토콜을 쓰되, HTML 대신 **CSV / JSON / XML**로 데이터를 주고받음:

---

**JSON**(JavaScript Object Notation): 파이썬 딕셔너리·리스트와 구조가 거의 동일한 텍스트 포맷.

```json
{"name": "혼자 공부하는 데이터 분석"}
```
- 키-값을 콜론(`:`)으로 연결
- 문자열은 큰따옴표(`"`) 필수
- 대괄호([])로 배열 표현 가능 → 여러 값 또는 여러 객체를 나열

### 변환 함수

`json.dumps(obj, ensure_ascii=False)`: 파이썬 객체 → JSON 문자열. *한글 보존: `ensure_ascii=False`

`json.loads(str)`: JSON 문자열 → 파이썬 객체 

*HTTP는 텍스트 기반 프로토콜이라 객체를 그대로 못 보내고 문자열로 변환해서 전송해야 함.

### JSON ↔ 데이터프레임

`pandas.read_json(json_str)`: JSON 문자열 → 데이터프레임 

`pandas.DataFrame(list_of_dict)`: 이미 변환된 파이썬 객체(리스트) → 데이터프레임 

---

**XML**(eXtensible Markup Language): 시작 태그·종료 태그로 계층 구조를 표현. JSON보다 장황하지만 구조적.

```xml
<book>                                <!-- 부모(루트) 엘리먼트 -->
  <name>혼자 공부하는 데이터 분석</name>   <!-- 자식 엘리먼트 -->
  <author>박해선</author>
  <year>2022</year>
</book>
```

- 부모(=루트) 엘리먼트가 자식 엘리먼트들을 포함
- 태그 이름 규칙: 특수문자·공백 불가, 숫자로 시작 불가, 동일 자식이 여럿이면 복수형 권장(`<books>`)
- JSON과 달리 배열 구조가 없어, 여러 항목은 상위 태그로 감싸서 표현 (`<books><book>...</book><book>...</book></books>`)

### 핵심 함수 / 메서드 (`xml.etree.ElementTree`)

| 코드 | 기능 |
|---|---|
| `fromstring(문자열)` | XML 문자열 → `Element` 객체(부모/루트 반환) |
| `element.findtext('태그명')` | 해당 태그의 텍스트 1개 반환. 순서와 무관하게 안전하게 탐색 |
| `element.findall('태그명')` | 동일 이름의 자식 엘리먼트를 **모두** 리스트로 반환 (for문과 함께 사용) |

> 판다스 1.3.0 이상: `pandas.read_xml(xml_str)`로 바로 데이터프레임 변환 가능

---

## requests 패키지

파이썬에서 웹 기반 API(URL)를 호출할 때 널리 쓰이는 패키지.

| 코드 | 기능 |
|---|---|
| `requests.get(url)` | GET 방식으로 URL 호출, `Response` 객체 반환 |
| `Response.json()` | 응답으로 받은 JSON 문자열을 파이썬 객체로 변환 |
| `Response.text` | 응답 원본 텍스트 |
| `Response.content` | 응답 데이터(bytes) — 이미지 등 바이너리에 유용 |
| `Response.status_code` | HTTP 상태 코드 (200=정상, 404=파일 없음 등) |

API 응답은 보통 여러 겹으로 중첩된 딕셔너리 구조이므로, 필요한 리스트를 꺼낸 뒤 `pandas.DataFrame()`으로 변환하는 과정이 뒤따름.

---

## 핵심 함수·메서드 요약

| 함수/메서드 | 기능 |
|---|---|
| `json.dumps()` | 파이썬 객체 → JSON 문자열 |
| `json.loads()` | JSON 문자열 → 파이썬 객체 |
| `pandas.read_json()` | JSON 문자열 → 데이터프레임/시리즈 |
| `xml.etree.ElementTree.fromstring()` | XML 문자열 → `Element` 객체 |
| `Element.findtext()` | 지정 태그의 첫 자식 텍스트 반환 |
| `Element.findall()` | 지정 태그와 일치하는 모든 자식 엘리먼트 반환 |
| `requests.get()` | GET 방식으로 URL 호출, `Response` 객체 반환 |
| `Response.json()` | 응답 JSON 문자열 → 파이썬 객체 |

## 02.웹 스크래핑 사용하기

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->


# 2️⃣ 수행 인증

<!-- 교재에서 안내된 과정을 직접 실행해본 뒤, 진행 결과가 보이도록 4~6장의 스크린샷을 캡처하여 아래에 첨부해주세요.-->



<br>
<br>

# 3️⃣ 확인 문제

## 문제 1.

> **🧚Q. 다음 중 BeautifulSoup 외에 웹 스크래핑에 사용할 수 있는 파이썬 패키지로 가장 적절한 것은 무엇인가요?**

```
1️⃣ NumPy  
2️⃣ Scrapy  
3️⃣ Matplotlib  
4️⃣ Scikit-learn  
```

```
여기에 선택한 답과 그 이유를 간단히 서술해주세요!
```



### 🎉 수고하셨습니다.
