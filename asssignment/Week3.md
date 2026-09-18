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

# 3장-1. 불필요한 데이터 삭제하기

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


## 02. 잘못된 데이터 수정하기

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->


# 2️⃣ 수행 인증

<!-- 교재에서 안내된 과정을 직접 실행해본 뒤, 진행 결과가 보이도록 4~6장의 스크린샷을 캡처하여 아래에 첨부해주세요.-->
<!-- 이번 주차에는 API를 발급받는 과정도 포함하여 첨부해주세요.-->


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
