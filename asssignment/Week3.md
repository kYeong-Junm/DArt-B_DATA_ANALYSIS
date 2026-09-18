# 데이터분석 3주차 정규과제

📌데이터분석 정규과제는 매주 정해진 분량의 『*혼자 공부하는 데이터 분석 with 파이썬*』 을 읽고 학습하는 것입니다. 이번 주는 아래의 **DataAnalysis_3rd_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=CE3_InvbmLY&list=PLVsNizTWUw7FGzSRCkQrPEEe-ljVXgS7k&index=6
https://www.youtube.com/watch?v=hhbzUEQWdTg&list=PLVsNizTWUw7FGzSRCkQrPEEe-ljVXgS7k&index=7
-->


## DataAnalysis_3rd_TIL

### 3장 데이터 정제하기
#### 01. 불필요한 데이터 삭제하기
#### 02. 잘못된 데이터 수정하기


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~81    | ✅         |
| 2주차 | p.84~151   | ✅         |
| 3주차 | p.154~219  | ✅         |
| 4주차 | p.222~279 | 🍽️         |
| 5주차 | p.282~325 | 🍽️         |
| 6주차 | p.328~379 | 🍽️         |
| 7주차 | p.382~430 | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->


# 1️⃣ 개념 정리 

## 01. 불필요한 데이터 삭제하기

핵심 키워드: `데이터 정제` `데이터 랭글링` `데이터 먼징` `원소별 비교` `불리언 배열` `넘파이`

## 목차
- [데이터 정제란](#데이터-정제란)
- [열 삭제하기](#열-삭제하기)
- [loc 메서드와 불리언 배열](#loc-메서드와-불리언-배열)
- [drop() 메서드](#drop-메서드)
- [dropna() 메서드](#dropna-메서드)
- [행 삭제하기](#행-삭제하기)
- [중복된 행 찾기](#중복된-행-찾기)
- [그룹별로 모으기: groupby()](#그룹별로-모으기-groupby)
- [원본 데이터 업데이트하기: update()](#원본-데이터-업데이트하기-update)
- [일괄 처리 함수 만들기](#일괄-처리-함수-만들기)
- [핵심 함수·메서드 요약](#핵심-함수메서드-요약)

---

## 데이터 정제란

API, 웹 스크래핑, 데이터베이스 등으로 수집한 데이터는 때때로 불완전함 (값이 잘못 들어가거나 불필요한 문자가 섞여 있음). 분석에 필요하지 않은 행이나 열은 제거해야 함.

- **데이터 정제(data cleaning)**: 데이터에서 손상되거나 부정확한 부분을 수정하고, 불필요한 데이터를 삭제하거나 불완전한 값을 교체하는 작업
- 데이터 정제는 **데이터를 분석 목적에 맞게 변환하는 데이터 랭글링(data wrangling) 또는 데이터 먼징(data munging)의 일부**로 수행될 수 있음
- 판다스에서 `NaN`은 **누락된 값, 비어 있는 값**을 의미

---

## 열 삭제하기

### 배경: 불필요한 열이 생기는 이유

CSV 파일 각 라인의 끝에 콤마(`,`)가 있으면, 판다스가 자동으로 `Unnamed: 13` 같은 불필요한 열을 추가함. 이런 열은 분석에 쓸모없으므로 삭제 대상.

### 방법 1: `loc` 메서드 + 슬라이싱

```python
ns_book = ns_df.loc[:, '번호':'등록일자']   # '번호' 열부터 '등록일자' 열까지 전체 행 선택
```

- 이 방법은 편리하지만, **중간에 있는 열만 제외**하려면 슬라이싱으로는 어려움 → 이럴 때 **불리언 배열**을 사용

---

## loc 메서드와 불리언 배열

### `columns` 속성

```python
print(ns_df.columns)
# Index(['번호', '도서명', ..., 'Unnamed: 13'], dtype='object')

print(ns_df.columns[0])   # '번호' — 숫자 인덱스로 참조 가능
```
- `columns` 속성은 판다스의 **Index 클래스** 객체

### 원소별 비교 (element-wise comparison)

Index 클래스를 비롯한 판다스 배열 성격의 객체는 어떤 값과 비교할 때 **자동으로 배열에 있는 모든 원소와 하나씩 비교**해줌. 이를 **원소별 비교**라고 함.

```
["혼공", "분석", "파이썬"] == "분석"
→ [False, True, False]
```

### 불리언 배열로 원하는 열 선택하기

```python
selected_columns = ns_df.columns != 'Unnamed: 13'   # !=는 비교 연산자
ns_book = ns_df.loc[:, selected_columns]             # True인 열의 모든 행을 선택합니다.
```

- `!=` 결과로 반환되는 것은 **넘파이 배열(Numpy array)**. **넘파이는 효율적으로 배열을 다룰 수 있는 파이썬의 대표 패키지**
- 지금은 넘파이 배열을 파이썬 리스트와 비슷한 것으로 생각해도 괜찮음
- 중간에 있는 열(예: `'부가기호'`)을 제외할 때도 동일한 방식 사용 가능:
```python
selected_columns = ns_df.columns != '부가기호'
ns_book = ns_df.loc[:, selected_columns]
```

---

## drop() 메서드

판다스에서는 **데이터프레임의 행이나 열을 삭제하는 `drop()` 메서드**를 제공.

```python
ns_book = ns_df.drop('Unnamed: 13', axis=1)
```
- 첫 번째 매개변수: 삭제하려는 열 이름
- **`axis` 매개변수에는 삭제할 축을 지정**할 수 있음. **`axis` 매개변수의 기본값인 0은 행을 삭제**합니다. **1로 지정하면 열을 삭제**합니다.

### 여러 열 한 번에 삭제

```python
ns_book = ns_df.drop(['부가기호', 'Unnamed: 13'], axis=1)  # 리스트 형식으로 여러 개 지정
```

### `inplace` 매개변수

```python
ns_book.drop('주제분류번호', axis=1, inplace=True)   # 선택한 데이터프레임에 덮어씁니다.
```
- `inplace=True`로 지정하면 **현재 선택한 데이터프레임을 바로 수정**할 수 있음
- **참고**: 사실 `inplace=True`일 때 원래 변수에 연결된 객체가 수정되는 것은 아님. 판다스는 내부적으로 수정된 새로운 객체를 만든 후 그 변수에 연결함. 따라서 `inplace` 매개변수를 사용하더라도 **성능상의 이득은 없음**

---

## dropna() 메서드

`drop()`과 비슷한 **`dropna()` 메서드는 기본적으로 NaN이 하나 이상 포함된 행이나 열을 삭제**함.

```python
ns_book = ns_df.dropna(axis=1)   # NaN이 하나라도 있으면 그 열 삭제
```

### 모든 값이 NaN인 열만 삭제하려면

```python
ns_book = ns_df.dropna(axis=1, how='all')   # how='all' → 모든 값이 NaN인 열만 삭제
```

> `dropna()` 메서드도 `inplace=True`를 지정하여 데이터프레임을 새로 생성·반환하지 않고 현재 데이터프레임을 수정할 수 있음

---

## 행 삭제하기

### drop() 메서드로 행 삭제

```python
ns_book2 = ns_book.drop([0, 1])   # 인덱스 0부터 1까지 2개 행을 선택합니다.
```
- 행 삭제 시 `axis=0`(기본값)이라 생략 가능. 다만 숫자로 된 행 인덱스를 직접 지정하는 일은 흔치 않음

### `[ ]` 연산자와 슬라이싱

`[ ]` 연산자에 **열 이름/리스트**를 전달하면 열을 선택하지만, **슬라이싱이나 불리언 배열을 전달하면 행을 선택**함.

```python
ns_book2 = ns_book[2:]     # 인덱스 0, 1을 제외한 나머지 행
ns_book2 = ns_book[0:2]    # 인덱스 0과 1 선택, 2는 포함하지 않음
```

> `loc` 메서드에 슬라이싱을 사용하면 **마지막 인덱스를 포함**하지만, `[ ]` 연산자에 슬라이싱을 사용하면 **마지막 인덱스를 포함하지 않음** (파이썬 슬라이싱과 동일한 규칙)

### `[ ]` 연산자와 불리언 배열

슬라이싱 외에도 **불리언 배열을 사용해서 행을 선택**할 수 있음 — **행을 선택할 때 가장 즐겨 사용하는 방법**.

```python
selected_rows = ns_df['출판사'] == '한빛미디어'
ns_book2 = ns_book[selected_rows]

# 조건을 변수 없이 [] 연산자에 바로 넣는 방식이 더 일반적
ns_book2 = ns_book[ns_book['대출건수'] > 1000]
```

> `loc` 메서드에도 불리언 배열을 사용해 행을 선택할 수 있음 (`ns_book.loc[selected_rows]`)

---

## 중복된 행 찾기

**판다스 데이터프레임의 중복된 행은 `duplicated()` 메서드를 사용하여 검사**할 수 있음. **중복된 행 중에서 처음 행을 제외한 나머지 행은 True로, 그 외에 중복되지 않은 나머지 모든 행은 False로 표시**한 불리언 배열을 반환함.

```python
sum(ns_book.duplicated())   # sum()으로 True 개수(=중복 행 개수) 셀 수 있음
```

- `duplicated()` 메서드는 **기본적으로 데이터프레임에 있는 모든 열을 기준으로 중복된 행을 찾음**
- **일부 열만 기준으로** 찾으려면 `subset` 매개변수에 기준 열을 나열:

```python
sum(ns_book.duplicated(subset=['도서명', '저자', 'ISBN']))
```

- **`keep` 매개변수를 False로 지정하여 중복된 모든 행을 True로 표시**할 수도 있음(처음 등장한 행까지 포함해서 전부 표시):

```python
dup_rows = ns_book.duplicated(subset=['도서명', '저자', 'ISBN'], keep=False)
ns_book3 = ns_book[dup_rows]
```

> 중복된 행을 합치지 않고 그냥 **삭제**하려면 `drop_duplicates()` 메서드 사용 (subset, keep, inplace 매개변수 동일하게 제공)

---

## 그룹별로 모으기: groupby()

**같은 도서의 대출건수는 하나로 합치는 것이 좋음** → 이때 **`groupby()` 메서드**를 사용. `by` 매개변수에는 **행을 합칠 때 기준이 되는 열**을 지정.

```python
count_df = ns_book[['도서명', '저자', 'ISBN', '권', '대출건수']]

group_df = count_df.groupby(by=['도서명', '저자', 'ISBN', '권'], dropna=False)
loan_count = group_df.sum()

# 또는 메서드를 연이어 호출 (판다스 사용자들이 선호하는 방식 — 어떤 작업을 하는지 명확히 드러남)
loan_count = count_df.groupby(by=['도서명', '저자', 'ISBN', '권'], dropna=False).sum()
```

- 정수 타입인 '대출건수' 열을 합칠 때는 **`sum()` 메서드** 사용
- **`groupby()` 메서드는 기본적으로 `by` 매개변수에 지정된 열에 NaN이 포함되어 있으면 해당 행을 삭제**함 → 계산에 NaN 있는 행도 포함하려면 **`dropna` 매개변수를 False로 지정**
- `head()`로 출력 시 그룹 기준이 된 **인덱스 열은 굵게 표시**됨

---

## 원본 데이터 업데이트하기: update()

**다른 데이터프레임을 사용해 원본 데이터프레임의 값을 업데이트할 때는 `update()` 메서드**를 사용. 원본에 중복 데이터가 있는 경우 아래 과정을 거쳐야 함:

1. **`duplicated()` 메서드로 중복된 행을 True로 표시한 불리언 배열**을 만듭니다.
2. 1번에서 구한 **불리언 배열을 반전시켜서 중복되지 않은 고유한 행을 True로 표시**합니다.
3. 2번에서 구한 불리언 배열을 사용해 **원본 배열에서 고유한 행만 선택**합니다.

```python
dup_rows = ns_book.duplicated(subset=['도서명', '저자', 'ISBN', '권'])
unique_rows = ~dup_rows                          # ~ 연산자로 반전
ns_book3 = ns_book[unique_rows].copy()           # copy()로 복사본 생성
```

> **`copy()` 메서드는 왜 사용하나요?** — `copy()`는 데이터프레임의 복사본을 만듦. 판다스는 `copy()`를 사용하지 않으면 일부 행/열을 선택해 만든 데이터프레임이 별도의 메모리 공간에 저장되는지 보장하지 않음. 따라서 명시적으로 복사하지 않고 값을 업데이트하면 원본 데이터가 바뀔 수도 있음. **일부 행이나 열을 선택하여 데이터를 업데이트할 때는 항상 복사하는 것이 좋음**

### 인덱스 맞추기 → 업데이트 → 인덱스 재설정

```python
ns_book3.set_index(['도서명', '저자', 'ISBN', '권'], inplace=True)  # 인덱스로 설정
ns_book3.update(loan_count)                                          # 값 업데이트
ns_book4 = ns_book3.reset_index()                                    # 인덱스 재설정(해제)
```
- `set_index()`: 지정한 열을 인덱스로 설정
- `reset_index()`: 데이터프레임 인덱스를 재설정(다시 기본 정수 인덱스로)
- 여러 열을 인덱스로 설정했다가 해제하면 **열 순서가 바뀌므로**, 원래 순서로 되돌리려면:

```python
ns_book4 = ns_book4[ns_book.columns]   # 원본 데이터프레임의 열 이름 순서를 전달합니다.
```

---

## 일괄 처리 함수 만들기

반복 작업을 새로운 데이터에 쉽게 적용하도록 **하나의 함수로 감싸서** 재사용.

```python
def data_cleaning(filename):
    """남산 도서관 장서 CSV 데이터 전처리 함수"""
    ns_df = pd.read_csv(filename, low_memory=False)
    ns_book = ns_df.dropna(axis=1, how='all')

    count_df = ns_book[['도서명', '저자', 'ISBN', '권', '대출건수']]
    loan_count = count_df.groupby(by=['도서명', '저자', 'ISBN', '권'], dropna=False).sum()

    dup_rows = ns_book.duplicated(subset=['도서명', '저자', 'ISBN', '권'])
    unique_rows = ~dup_rows
    ns_book3 = ns_book[unique_rows].copy()
    ns_book3.set_index(['도서명', '저자', 'ISBN', '권'], inplace=True)
    ns_book3.update(loan_count)

    ns_book4 = ns_book3.reset_index()
    ns_book4 = ns_book4[ns_book.columns]
    return ns_book4
```

- 서로 다른 데이터프레임이 동일한지 비교할 때는 **`equals()` 메서드** 사용:
```python
new_ns_book4 = data_cleaning('ns_202104.csv')
ns_book4.equals(new_ns_book4)   # True
```

---

## 핵심 함수·메서드 요약

| 함수/메서드 | 기능 |
|---|---|
| `DataFrame.drop()` | 데이터프레임의 행이나 열을 삭제 |
| `DataFrame.dropna()` | 누락된 값(NaN)이 포함된 행이나 열을 삭제 |
| `DataFrame.duplicated()` | 중복된 행을 찾아 불리언 값으로 표시한 배열을 반환 |
| `DataFrame.groupby()` | 데이터프레임의 행을 그룹으로 모음 |
| `DataFrame.sum()` | 행 또는 열을 기준으로 합계를 계산 |
| `DataFrame.set_index()` | 지정한 열을 인덱스로 설정 |
| `DataFrame.reset_index()` | 데이터프레임의 인덱스를 재설정 |
| `DataFrame.update()` | 다른 데이터프레임을 사용해 원본 데이터프레임의 값을 업데이트 (다른 데이터프레임의 NaN은 업데이트에서 제외) |
| `DataFrame.equals()` | 다른 데이터프레임과 동일한 원소를 가졌는지 비교 (동일하면 True) |

<br>

---

<br>

## 02. 잘못된 데이터 수정하기

핵심 키워드: `NaN` `정규 표현식`

## 목차
- [데이터프레임 정보 요약 확인하기: info()](#데이터프레임-정보-요약-확인하기-info)
- [누락된 값 개수 확인하기: isna()](#누락된-값-개수-확인하기-isna)
- [누락된 값으로 표시하기: None과 np.nan](#누락된-값으로-표시하기-none과-npnan)
- [누락된 값 바꾸기(1): loc, fillna()](#누락된-값-바꾸기1-loc-fillna)
- [누락된 값 바꾸기(2): replace()](#누락된-값-바꾸기2-replace)
- [정규 표현식](#정규-표현식)
- [잘못된 값 바꾸기](#잘못된-값-바꾸기)
- [누락된 정보 채우기](#누락된-정보-채우기)
- [일괄 처리 함수 만들기](#일괄-처리-함수-만들기)
- [핵심 함수·메서드 요약](#핵심-함수메서드-요약)

---

## 데이터프레임 정보 요약 확인하기: info()

데이터에서 잘못된 값을 파악하려면 **데이터가 의미하는 바를 이해하고 시간을 들여 직접 데이터를 꼼꼼하게 살펴보아야** 함. 판다스는 **누락된 값을 기본적으로 NaN으로 표시**함.

```python
ns_book4.info()
```
- **`info()` 메서드는 데이터프레임 정보를 요약해서 출력**해줌
- 출력 내용: 전체 행 개수 → 열 개수, 열 이름, **각 열마다 누락된 값이 없는 행 개수**, 열 데이터 타입 → 마지막에 사용하는 데이터 타입과 메모리 사용량
- 각 열의 (non-null count)과 전체 행 개수를 비교하면 **몇 개가 누락됐는지** 계산 가능
- `float64`는 실수형, `int64`는 정수형, `object`는 문자열 또는 혼합형 데이터 타입

### 메모리 사용량 정확히 확인하기

```python
ns_book4.info(memory_usage='deep')   # 정확한 메모리 사용량 표시
```
- `info()`는 기본적으로 원소 개수와 데이터 타입을 기반으로 메모리 사용량을 **추정**함. 정확한 값을 얻으려면 `memory_usage` 매개변수에 `'deep'` 옵션 지정

---

## 누락된 값 개수 확인하기: isna()

`info()`로도 누락된 개수를 헤아릴 수 있지만 번거로움. **NaN을 직접 카운트할 수 있는 `isna()` 메서드를 사용하면 훨씬 편리**함.

```python
ns_book4.isna().sum()
```
- **`isna()` 메서드는 각 행이 비어 있는지를 나타내는 불리언 배열을 반환**함
- **`sum()` 메서드를 이어서 호출하면 불리언 배열의 True 개수로 비어 있는 행 개수를 얻을 수 있음**
- 반대로 **누락되지 않은 값을 확인할 때는 `notna()` 메서드**를 사용 (사용법 동일)

---

## 누락된 값으로 표시하기: None과 np.nan

판다스 데이터프레임에서는 **정수를 저장하는 열에 파이썬의 `None`을 입력하면 누락된 값으로 인식**함.

```python
ns_book4.loc[0, '도서권수'] = None
```
- 이때 원래 정수(1)였던 값이 `1.0`처럼 실수로 바뀜 — **판다스가 NaN을 특별한 실수 값으로 저장하기 때문**. 그래서 원래 데이터 타입이 int64였던 열이 NaN을 표시하기 위해 **float64로 자동으로 바뀜**

### astype() 메서드로 데이터 타입 되돌리기

```python
ns_book4 = ns_book4.astype({'도서권수': 'int32', '대출건수': 'int32'})
```
- 데이터 타입을 지정할 때는 **`astype()` 메서드**를 사용. 매개변수를 `{열 이름: 데이터 타입}` 형식의 **딕셔너리**로 전달
- **`astype()` 메서드는 열과 바꾸려는 데이터 타입을 딕셔너리로 전달합니다. 또한 기본적으로 새로운 데이터프레임을 반환**함

### 문자열 열에서 None vs np.nan

정수형이 아니라 문자열을 저장할 수 있는 열(`object` 타입)에 `None`을 입력하면 NaN으로 표시되지 않고 **문자열 그대로 None으로 표시**됨.

- **사실 판다스는 NaN이라는 값을 따로 가지고 있지 않음. 대신 넘파이 패키지에 있는 `np.nan`을 사용**해야 함

```python
import numpy as np
ns_book4.loc[0, '부가기호'] = np.nan   # 숫자/문자열 타입 상관없이 NaN으로 표시하려면 np.nan 사용
```

---

## 누락된 값 바꾸기(1): loc, fillna()

### loc + isna()로 직접 바꾸기

**`loc` 메서드를 사용하면 누락된 값을 원하는 값으로 바꿀 수 있음**. 그러려면 누락된 값을 가리키는 **불리언 배열**을 만들어야 하는데, `isna()` 메서드로 간단하게 만들 수 있음.

```python
set_isbn_na_rows = ns_book4['세트 ISBN'].isna()      # 누락된 값을 찾아 불리언 배열로 반환
ns_book4.loc[set_isbn_na_rows, '세트 ISBN'] = ''      # 누락된 값을 빈 문자열로 바꿉니다.
```

### fillna() 메서드 — 더 편리한 방법

```python
ns_book4.fillna('없음')             # 모든 NaN을 '없음' 문자열로
ns_book4['부가기호'].fillna('없음')  # 특정 열만 선택해서 NaN을 바꾸기
```
- **`fillna()` 메서드에 원하는 값을 전달하면 NaN을 대체**할 수 있음
- `fillna()`는 기본적으로 **새로운 데이터프레임을 반환**하므로 `isna()`를 이어서 호출하면 개수 확인 가능
- 특정 열을 선택한 후 `fillna()`를 적용하면 열 이름 없이 개수만 있는 **판다스 시리즈 객체**로 반환됨. 전체 데이터프레임을 반환하려면 **열 이름과 바꾸려는 값으로 이루어진 딕셔너리**를 전달:
```python
ns_book4.fillna({'부가기호': '없음'})
```

---

## 누락된 값 바꾸기(2): replace()

**`replace()` 메서드는 NaN은 물론 어떤 값도 바꿀 수 있는 편리한 메서드**. 사용법이 다양함.

### 첫째, 바꾸려는 값이 하나일 때

```python
replace(원래 값, 새로운 값)
ns_book4.replace(np.nan, '없음')
```

### 둘째, 바꾸려는 값이 여러 개일 때 (리스트)

```python
replace([원래 값1, 원래 값2], [새로운 값1, 새로운 값2])
ns_book4.replace([np.nan, '2021'], ['없음', '21'])
```
또는 딕셔너리 형식으로도 전달 가능: `replace({np.nan: '없음', '2021': '21'})`

### 셋째, 열마다 다른 값으로 바꿀 때

```python
replace({열 이름: 원래 값}, 새로운 값)
ns_book4.replace({'부가기호': np.nan}, '없음')   # '부가기호' 열의 NaN만 '없음'으로 바뀝니다.
```
또는 열 이름과 변경 전후 값을 `{열 이름: {원래 값1: 새로운 값1}}`처럼 **중첩된 딕셔너리**로 전달 가능:
```python
ns_book4.replace({'부가기호': {np.nan: '없음'}, '발행년도': {'2021': '21'}})
```

> `replace()` 메서드도 판다스 데이터프레임의 다른 메서드와 마찬가지로 **기본적으로 새로운 데이터프레임을 반환**하므로 여러 메서드나 `[]` 인덱싱을 연이어 쓸 수 있음

---

## 정규 표현식

**정규 표현식(regular expression, 정규식)은 문자열 패턴을 찾아서 대체하기 위한 규칙의 모음**임. 연도가 다양한 형태(예: '2018', '2021')로 있을 때, 특정 값만 일일이 지정해서 바꾸는 것은 번거로움 → 정규 표현식을 사용하면 훨씬 간편하게 처리 가능.

### 숫자 찾기: `\d`

- 정규 표현식에서 **숫자를 나타내는 기호는 `\d`**. 네 자리 연도는 `\d\d\d\d`
- **표현식을 그룹으로 묶을 때는 괄호를 사용**. 뒤 두 자리만 그룹으로 묶으려면 `\d\d(\d\d)`처럼 씀
- 패턴에 맞는 문자열을 찾은 후 **그룹에 해당하는 번호로 참조**할 때는 `\1`, `\2`처럼 사용 (그룹 번호는 패턴 안에 등장하는 순서대로 매겨짐)

```python
ns_book4.replace({'발행년도': {r'\d\d(\d\d)': r'\1'}}, regex=True)[100:102]
```
- `replace()`에 정규 표현식을 사용한다는 의미로 **`regex` 매개변수 옵션을 True로 지정**
- 정규 표현식 앞에 붙인 **`r` 문자는 파이썬에서 정규 표현식을 다른 문자열과 구분하기 위해 접두사처럼 붙이는 것**

### 반복 개수 지정: 중괄호 `{ }`

정규 표현식이 반복될 때는 일일이 쓰는 대신 **중괄호를 사용하여 개수를 지정**할 수 있음. 예를 들어 `\d{2}`는 `\d\d`와 동일하게 연속된 숫자 두 개를 의미.

```python
ns_book4.replace({'발행년도': {r'\d{2}(\d{2})': r'\1'}}, regex=True)[100:102]
```

### 문자 찾기: 마침표(`.`)와 `*`

- **어떤 문자에도 대응하는 정규 표현식 문자는 마침표(`.`)**
- 이름처럼 몇 글자가 될지 알 수 없어 반복 개수를 지정하기 어려울 때는 **`*` 문자를 사용하여 0개 이상 반복된다고 표시**할 수 있음 → `.*`
- 찾으려는 패턴 자체에 괄호(`(`, `)`)가 있는 경우, 그룹이 아니라 일반 문자로 인식하게 하려면 **역슬래시(`\`)를 앞에 붙임**
- 공백 문자를 나타내는 정규 표현식은 `\s`

```python
# 저자 열에서 '(지은이)', '(옮긴이)' 같은 구분 문자열을 삭제하는 예
ns_book4.replace(
    {'저자': {r'(.*)\s\(지은이\)(.*)\s\(옮긴이\)': r'\1\2'},
     '발행년도': {r'\d{2}(\d{2})': r'\1'}},
    regex=True
)[100:102]
```

---

## 잘못된 값 바꾸기

### astype() 시도 시 오류 확인

```python
ns_book4.astype({'발행년도': 'int32'})
# ValueError: invalid literal for int() with base 10: '1988.'
```
- 숫자가 아닌 다른 문자(마침표, 대괄호, 한글 등)가 섞인 값이 있어서 발생

### 문자열 패턴 검사: str.contains()

**판다스 시리즈 객체는 `str` 속성 아래 다양한 문자열 처리 함수를 제공**함. **`contains()` 메서드는 시리즈나 인덱스에서 문자열 패턴을 포함하고 있는지 검사**함.

```python
ns_book4['발행년도'].str.contains('1988').sum()
```
> `ns_book4['발행년도'] == '1988'`처럼 쓰면 정확히 '1988'인 것만 찾아서 '1988.'같은 값은 제외됨. `contains()`는 **주어진 문자열이 포함된 모든 행**을 찾음

- **`contains()` 메서드는 기본적으로 정규 표현식을 인식**함. 숫자에 대응하는 정규 표현식이 `\d`라면, **숫자가 아닌 다른 모든 문자에 대응하는 표현은 `\D`** (정규 표현식은 대소문자를 반대 용도로 사용)

```python
invalid_number = ns_book4['발행년도'].str.contains('\D', na=True)
```
- **`na` 매개변수를 True로 지정**하여 값이 누락된 행도 True로 표시 (contains()는 기본적으로 누락된 값을 np.nan으로 채워서 인덱싱에 사용할 수 없기 때문)

### 정규 표현식으로 연도만 추출

```python
ns_book5 = ns_book4.replace({'발행년도': r'.*(\d{4}).*'}, r'\1', regex=True)
```
- 연도 앞뒤에 어떤 문자가 있어도 매칭하도록 `.*`를 사용하고, 네 자리 숫자를 그룹으로 묶어 그 부분만 남김

### 임의 값(-1)으로 처리하고 정수형 변환

```python
ns_book5.loc[unkown_year, '발행년도'] = '-1'
ns_book5 = ns_book5.astype({'발행년도': 'int32'})
```
- 변환할 수 없는 값(NaN이거나 네 자리 숫자가 아닌 값)은 **임의로 -1로 바꾼 후** 정수형으로 변환

### 비교 메서드로 이상값 찾기: gt(), lt() 등

**`gt()` 메서드는 전달된 값보다 큰 값을 찾음**.

```python
ns_book5['발행년도'].gt(4000).sum()   # 4000보다 큰 값의 개수를 셉니다.
```

| 메서드 | 부등호 | 내용 |
|---|---|---|
| `gt()` | `>` | 지정된 값보다 큰 값을 검사 |
| `ge()` | `>=` | 지정된 값보다 크거나 같은 값을 검사 |
| `lt()` | `<` | 지정된 값보다 작은 값을 검사 |
| `le()` | `<=` | 지정된 값보다 작거나 같은 값을 검사 |
| `eq()` | `==` | 지정된 값과 같은 값을 검사 |
| `ne()` | `!=` | 지정된 값과 같지 않은 값을 검사 |

> `gt()` 메서드 대신 `(ns_book5['발행년도'] > 4000).sum()`처럼 부등호 기호를 직접 사용해도 동일하게 동작함

### 단군기원 연도 보정, 비정상적으로 오래된 연도 처리

```python
dangun_yy_rows = ns_book5['발행년도'].gt(4000)
ns_book5.loc[dangun_yy_rows, '발행년도'] = ns_book5.loc[dangun_yy_rows, '발행년도'] - 2333   # 단군기원 → 서기로 변환

dangun_year = ns_book5['발행년도'].gt(4000)   # 보정 후에도 여전히 4000 넘는 값
ns_book5.loc[dangun_year, '발행년도'] = -1

old_books = ns_book5['발행년도'].gt(0) & ns_book5['발행년도'].lt(1900)   # 0보다 크고 1900 미만
ns_book5.loc[old_books, '발행년도'] = -1
```
- 여러 조건을 조합할 때는 `&`(AND) 연산자로 두 불리언 배열을 결합

---

## 누락된 정보 채우기

**도서명, 저자, 출판사, 발행년도는 분석에 중요하므로 누락된 값이 있으면 안 됨.**

```python
na_rows = (ns_book5['도서명'].isna() | ns_book5['저자'].isna()
           | ns_book5['출판사'].isna() | ns_book5['발행년도'].eq(-1))
```
- 여러 조건을 `|`(OR)로 연결하고, 코드 끝에 `\`를 붙이면 다음 줄과 이어진다는 의미

### 웹 스크래핑으로 누락된 정보 채우기

Yes24에서 ISBN으로 검색해 크롬 개발자 도구로 확인한 태그 정보를 활용해, 도서명·저자·출판사·발행연도를 추출하는 함수를 만듦.

```python
def get_book_info(row):
    title = row['도서명']
    author = row['저자']
    pub = row['출판사']
    year = row['발행년도']

    url = 'http://www.yes24.com/Product/Search?domain=BOOK&query={}'
    r = requests.get(url.format(row['ISBN']))
    soup = BeautifulSoup(r.text, 'html.parser')

    try:
        if pd.isna(title):
            title = soup.find('a', attrs={'class': 'gd_name'}).get_text()
    except AttributeError:
        pass

    try:
        if pd.isna(author):
            authors = soup.find('span', attrs={'class': 'info_auth'}).find_all('a')
            author_list = [auth.get_text() for auth in authors]
            author = ','.join(author_list)
    except AttributeError:
        pass

    try:
        if pd.isna(pub):
            pub = soup.find('span', attrs={'class': 'info_pub'}).find('a').get_text()
    except AttributeError:
        pass

    try:
        if year == -1:
            year_str = soup.find('span', attrs={'class': 'info_date'}).get_text()
            year = re.findall(r'\d{4}', year_str)[0]   # 정규 표현식으로 찾은 값 중 첫 번째만 사용
    except AttributeError:
        pass

    return title, author, pub, year
```

- 도서명과 달리 **저자는 두 명 이상일 수 있어 `find_all()` 메서드**를 사용해 여러 `<a>` 태그를 모두 추출 → **리스트 내포**로 텍스트를 리스트에 모은 뒤 **`join()` 메서드로 하나의 문자열로 합침**
- 발행 연도는 '2020년 12월'처럼 쓰여 있으므로 파이썬 **`re` 모듈의 `findall()` 함수**로 정규식에 매칭되는 모든 문자열을 리스트로 반환받아 처리
- 이 함수는 **누락된 값에만** 뷰티플수프로 추출한 값을 저장함. Yes24에 정보가 없거나 HTML 요소가 누락된 경우 오류가 발생할 수 있으므로 **`try ~ except` 문으로 예외 처리**를 해서 실행이 중단되지 않게 함

### apply()로 여러 값을 각기 다른 열로 반환

```python
updated_sample = ns_book5[na_rows].head(2).apply(get_book_info, axis=1, result_type='expand')
```
- 함수가 여러 개의 값을 반환하는 경우 `apply()`는 기본적으로 반환된 값을 **하나의 튜플**로 만듦. **`result_type` 매개변수를 `'expand'`로 지정**하면 반환된 값을 각기 다른 열로 만들 수 있음

### 그래도 남는 값은 삭제

```python
ns_book6 = ns_book5.dropna(subset=['도서명', '저자', '출판사'])
ns_book6 = ns_book6[ns_book6['발행년도'] != -1]
```
- 웹 스크래핑으로도 채우지 못한 나머지는 **분석 대상에서 제외(삭제)**

> 데이터를 정제하는 자세한 방법은 해결하려는 문제에 따라 달라짐. 데이터가 의미하는 바를 잘 이해하지 못하면 올바르게 정제할 수 없으므로, 필요하면 데이터를 제공한 사람이나 분야 전문가에게 도움을 요청하는 것이 좋음.

---

## 일괄 처리 함수 만들기

지금까지 수행한 작업(누락값 처리 → 발행년도 보정 → 정보 채우기 → 삭제)을 하나의 함수 `data_fixing()`으로 정리하여 다른 데이터에도 재사용 가능하게 만듦.

---

## 핵심 함수·메서드 요약

| 함수/메서드 | 기능 |
|---|---|
| `DataFrame.info()` | 데이터프레임의 요약 정보를 출력 |
| `DataFrame.isna()` | 누락된 값을 감지하는 메서드로 셀의 값이 None이나 NaN일 경우 True를 반환 |
| `DataFrame.astype()` | 데이터 타입을 지정 |
| `DataFrame.fillna()` | 데이터프레임에서 누락된 원소의 값을 채움 |
| `DataFrame.replace()` | 데이터프레임의 값을 다른 값으로 바꿈 |
| `Series.str.contains()` | 시리즈나 인덱스에서 문자열 패턴을 포함하고 있는지 검사 |
| `DataFrame.gt()` | 데이터프레임의 원소보다 큰 값을 검사 |



# 2️⃣ 수행 인증

<!-- 교재에서 안내된 과정을 직접 실행해본 뒤, 진행 결과가 보이도록 4~6장의 스크린샷을 캡처하여 아래에 첨부해주세요.-->
<!-- 이번 주차에는 API를 발급받는 과정도 포함하여 첨부해주세요.-->

<img width="1427" height="872" alt="image" src="https://github.com/user-attachments/assets/e3528bde-d10e-45a3-9ed8-216b6dbd2e98" />
<img width="1352" height="861" alt="image" src="https://github.com/user-attachments/assets/50ade548-1a23-4c5f-b4e9-607ac9b5db4f" />
<img width="1392" height="870" alt="image" src="https://github.com/user-attachments/assets/896433f0-61e2-4ab2-a594-052c2c4ca5c6" />


<br>
<br>

# 3️⃣ 확인 문제

## 문제 1.

> **🧚Q. 다음 두 데이터프레임 df1, df2를 합쳐서 데이터프레임 df3를 만들려고 합니다.**  
> 적절한 판다스 명령을 선택해주세요.

<table>
<tr>

<td>

### df1

| index | col1 | col2 |
|-------|------|------|
| 0     | x    | 5    |
| 1     | y    | 6    |
| 2     | z    | 7    |

</td>

<td>

### df2

| index | col3 | col4 |
|-------|------|------|
| 0     | x    | 50   |
| 1     | y    | 60   |
| 2     | w    | 70   |

</td>

<td align="center" valign="middle">

<h2> ➜ </h2>

</td>

<td>

### df3 (결과)

| index | col1 | col2 | col3 | col4 |
|-------|------|------|------|------|
| 0     | x    | 5.0  | x    | 50.0 |
| 1     | y    | 6.0  | y    | 60.0 |
| 2     | z    | 7.0  | NaN  | NaN  |
| 3     | NaN  | NaN  | w    | 70.0 |

</td>

</tr>
</table>

```
1️⃣ pd.merge(df1, df2)
2️⃣ pd.merge(df1, df2, how='left')
3️⃣ pd.merge(df1, df2, left_on='col1', right_on='col3', how='outer')
4️⃣ pd.merge(df1, df2, left_on='col1', right_on='col3', how='inner')
```

```
여기에 선택한 답과 그 이유를 간단히 서술해주세요!
```



### 🎉 수고하셨습니다.
