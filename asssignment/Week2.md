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

**XML**(eXtensible Markup Language): 시작 태그, 종료 태그로 계층 구조 표현. 구조적이지 못하여 API에서는 적절하지 않음.

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

`fromstring(문자열)`: XML 문자열 → `Element` 객체(부모/루트 반환) 

`element.findtext('태그명')`: 해당 태그의 텍스트 1개 반환. 순서와 무관하게 안전하게 탐색 

`element.findall('태그명')`: 동일 이름의 자식 엘리먼트를 모두 리스트로 반환 (for문과 함께 사용) 

> 판다스 1.3.0 이상: `pandas.read_xml(xml_str)`로 바로 데이터프레임 변환 가능

---

## URL 파라미터 (쿼리 스트링)

API를 호출할 때, URL 뒤에 원하는 조건(파라미터)을 붙여서 전달할 수 있음.


- `?` : 호출 주소와 파라미터를 구분하는 시작 표시
- `파라미터=값` : 조건 하나 (예: `startDt=2021-04-01` → 검색 시작일을 지정)
- `&` : 파라미터가 여러 개일 때 이어붙이는 구분자
- 이런 방식으로 파라미터를 URL에 붙여 요청하는 것을 **HTTP GET 방식**이라 부름
- URL 길이 제한이 있어(보통 2,000자 이내 안전), 더 긴 데이터는 **POST 방식**(URL이 아닌 HTTP 내부 공간에 데이터 전송) 사용

### 예시: 도서관 정보나루 API 파라미터

| 파라미터 | 의미 |
|---|---|
| `format` | 응답 형식 지정 (미지정 시 XML, `json` 지정 시 JSON) |
| `startDt` | 검색 시작 일자 |
| `endDt` | 검색 종료 일자 |
| `age` | 연령대 (예: `20` → 20대) |
| `authKey` | 인증키 (API 사용 자격 확인용) |

---

## requests 패키지
: 파이썬에서 웹 기반 API(URL)를 호출할 때 널리 쓰이는 패키지.
*브라우저 주소창에 URL 치고 엔터 누를 필요 없이, 파이썬 코드 한 줄로 대신함.

`requests.get(url)`: GET 방식으로 URL 호출, `Response` 객체 반환 

`Response.json()`: 응답으로 받은 JSON 문자열을 파이썬 객체로 변환 

`Response.text`: 응답 원본 텍스트 

`Response.content`: 응답 데이터(bytes) — 이미지 등 바이너리에 유용 

`Response.status_code`: HTTP 상태 코드 (200=정상, 404=파일 없음 등) 

API 응답은 보통 여러 겹으로 중첩된 딕셔너리 구조이므로, 필요한 리스트를 꺼낸 뒤 `pandas.DataFrame()`으로 변환하는 과정이 뒤따름.

---

## 핵심 요약

`json.dumps()`: 파이썬 객체 → JSON 문자열 

`json.loads()`: JSON 문자열 → 파이썬 객체 

`pandas.read_json()`: JSON 문자열 → 데이터프레임/시리즈 

`xml.etree.ElementTree.fromstring()`: XML 문자열 → `Element` 객체 

`Element.findtext()`: 지정 태그의 첫 자식 텍스트 반환 

`Element.findall()`: 지정 태그와 일치하는 모든 자식 엘리먼트 반환 

`requests.get()`: GET 방식으로 URL 호출, `Response` 객체 반환 

`Response.json()`: 응답 JSON 문자열 → 파이썬 객체 


## 02.웹 스크래핑 사용하기

**웹 스크래핑(web scraping)** = 프로그램으로 웹사이트 페이지를 옮겨 다니며 HTML에서 필요한 데이터를 직접 추출하는 방법. (= 웹 크롤링 web crawling)

- 원하는 데이터를 API로 제공하지 않을 때 사용하는 **최후의 수단**
- API처럼 정해진 구조가 없기 때문에, HTML 구조를 직접 분석해서 원하는 값의 위치를 찾아야 함
- 흐름: `검색 결과 페이지 URL 생성` → `HTML 요청(requests)` → `상세 페이지 링크 추출` → `상세 페이지 요청` → `원하는 값 추출`

---

## 데이터프레임 행·열 선택: `loc` 메서드

여러 열 이름을 매번 리스트로 나열하지 않고, 레이블 기반으로 행/열을 선택하는 방법.

```python
df.loc[[0, 1], ['bookname', 'authors']]     # 행 인덱스 0,1 + 지정 열
df.loc[0:1, 'bookname':'authors']           # 슬라이싱 (마지막 인덱스 '포함')
df.loc[:, 'no':'isbn13']                    # 전체 행 + 열 범위
df.loc[::2, 'no':'isbn13']                  # 스텝(step) 지정 가능
```

- 파이썬 슬라이싱과 달리 **`loc`의 슬라이싱은 끝 인덱스를 포함함**
- `iloc`은 **정수 위치(0, 1, 2 ...)** 기준으로 선택. (`loc`은 레이블/이름 기준)

---

## HTML에서 데이터 추출: 뷰티플수프(BeautifulSoup)
: HTML 안에서 원하는 태그를 "찾는" 역할.

### 기본 사용법

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(r.text, 'html.parser')   # HTML 파싱(파서로 구조화)
```
- **파싱(parsing)**: 텍스트를 분석해 프로그램이 다룰 수 있는 구조로 만드는 것
- `html.parser`: 파이썬 내장 파서 (lxml보다 느리지만 표준에 엄격하지 않아 실패가 적음)

### 태그 찾기 — 크롬 개발자 도구 활용

- 원하는 데이터가 HTML 어디 있는지는 브라우저 **개발자 도구**(마우스 오른쪽 클릭 → 검사)로 확인
- 태그 이름, `class`/`id` 속성을 확인해 두면 뷰티플수프로 정확히 찾을 수 있음

### 핵심 메서드

| 메서드 | 기능 |
|---|---|
| `soup.find('태그명', attrs={'속성':'값'})` | 조건에 맞는 **첫 번째** 태그 반환. 없으면 `None` |
| `soup.find_all('태그명')` | 조건에 맞는 **모든** 태그를 리스트로 반환. 없으면 빈 리스트 |
| `태그객체['속성명']` | 태그의 속성값 추출 (예: `['href']` → 링크 주소) |
| `태그객체.get_text()` | 태그 안에 담긴 텍스트만 추출 |

```python
prd_link = soup.find('a', attrs={'class': 'gd_name'})
prd_link['href']              # 링크 주소 추출

prd_tr_list = prd_detail.find_all('tr')
for tr in prd_tr_list:
    if tr.find('th').get_text() == '쪽수, 무게, 크기':
        page_td = tr.find('td').get_text()
```

> `find()`는 하나, `find_all()`은 여러 개 — XML의 `findtext()` / `findall()`과 개념이 같음

---

## 함수로 재사용 가능하게 만들기 + 데이터프레임에 일괄 적용

여러 건의 데이터를 반복 처리할 때는 **함수로 감싸고**, 데이터프레임 전체에 **일괄 적용**하는 방식을 사용.

### `apply()` 메서드

```python
def get_page_cnt2(row):
    return get_page_cnt(row['isbn13'])

page_count = books.apply(get_page_cnt2, axis=1)
```
- 데이터프레임의 각 행(또는 열)에 함수를 자동으로 적용
- `axis=1` → 각 **행**에 적용, `axis=0`(기본값) → 각 **열**에 적용
- for문으로 직접 반복하는 것보다 판다스 데이터프레임에 최적화된 방식
- **람다(lambda) 함수**로 더 간결하게 쓸 수도 있음: `df.apply(lambda row: 함수(row['열']), axis=1)`

### `merge()` 함수 — 데이터프레임/시리즈 합치기

```python
pd.merge(df1, df2, left_index=True, right_index=True)   # 인덱스 기준 합치기
pd.merge(df1, df2, on='공통열이름')                        # 특정 열 기준 합치기
```

| 매개변수 | 의미 |
|---|---|
| `on` | 합칠 기준이 되는 (양쪽에 공통으로 있는) 열 이름 |
| `how` | 합치는 방식: `inner`(기본, 공통 값만) / `left` / `right` / `outer`(전체 유지, 없는 값은 `NaN`) |
| `left_on` / `right_on` | 기준 열 이름이 서로 다를 때 각각 지정 |
| `left_index` / `right_index` | 열이 아니라 **인덱스**를 기준으로 합칠 때 `True` 지정 |

---

## 웹 스크래핑할 때 주의할 점

1. **`robots.txt` 확인하기**: 사이트마다 스크래핑을 허용/금지하는 경로를 명시한 `robots.txt` 파일이 있음 (예: `사이트주소/robots.txt`). 허용되지 않은 페이지를 스크래핑하면 접속이 차단될 수 있음
2. **HTML 태그를 특정할 수 있는지 확인**: 필요한 값의 위치를 태그 이름/속성으로 안정적으로 찾을 수 있어야 함. 일부 사이트는 자바스크립트로 데이터를 동적으로 채워서 단순 스크래핑으로는 못 가져올 수 있음 (이 경우 셀레니움 등 별도 도구 필요)
3. **웹 스크래핑은 최후의 수단**: 웹 페이지는 언제든 구조가 바뀔 수 있어 유지보수가 어려움. 가능하면 공개 API를 먼저 찾아보는 것이 좋음

---

## 핵심 함수·메서드 요약

| 함수/메서드 | 기능 |
|---|---|
| `DataFrame.loc[]` | 레이블(이름) 또는 불리언 배열로 행/열 선택 |
| `BeautifulSoup(html, 'html.parser')` | HTML 문자열을 파싱해 뷰티플수프 객체 생성 |
| `BeautifulSoup객체.find()` | 조건에 맞는 첫 번째 태그 반환 (없으면 `None`) |
| `BeautifulSoup객체.find_all()` | 조건에 맞는 모든 태그를 리스트로 반환 (없으면 빈 리스트) |
| `Tag.get_text()` | 태그 안의 텍스트 반환 |
| `DataFrame.apply()` | 데이터프레임의 행 또는 열에 지정한 함수를 일괄 적용 |
| `pandas.merge()` | 데이터프레임이나 시리즈 객체를 합침 |


# 2️⃣ 수행 인증

<img width="1117" height="906" alt="image" src="https://github.com/user-attachments/assets/ccb79595-09d4-47f4-9334-763531ba5488" />
<img width="832" height="492" alt="image" src="https://github.com/user-attachments/assets/07c80476-c5a1-4551-82e9-975eea2e73df" />
<img width="977" height="907" alt="image" src="https://github.com/user-attachments/assets/fe8f2301-d782-4fe8-8ca7-cb37bbacde8c" />
<img width="911" height="637" alt="image" src="https://github.com/user-attachments/assets/f699cb1d-f3ac-41f1-8aaf-f682f1adac43" 
/>
<img width="862" height="622" alt="image" src="https://github.com/user-attachments/assets/924233c2-612a-4e8f-acf0-1b1a9aea193b" />
<img width="737" height="902" alt="image" src="https://github.com/user-attachments/assets/67f81977-e7d9-4d8f-bbe3-1d56c29e7fc2" />








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
