# 데이터분석 6주차 정규과제

📌데이터분석 정규과제는 매주 정해진 분량의 『*혼자 공부하는 데이터 분석 with 파이썬*』 을 읽고 학습하는 것입니다. 이번 주는 아래의 **DataAnalysis_6th_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=XD65UhBMOiI&list=PLVsNizTWUw7FGzSRCkQrPEEe-ljVXgS7k&index=12
https://www.youtube.com/watch?v=NTQ5NXelOfw&list=PLVsNizTWUw7FGzSRCkQrPEEe-ljVXgS7k&index=13
-->


## DataAnalysis_6th_TIL

### 6장 복잡한 데이터 표현하기
#### 01. 객체지향 API로 그래프 꾸미기
#### 02. 맷플롯립의 고급 기능 배우기


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~81    | ✅         |
| 2주차 | p.84~151   | ✅         |
| 3주차 | p.154~219  | ✅         |
| 4주차 | p.222~279 | ✅         |
| 5주차 | p.282~325 | ✅         |
| 6주차 | p.328~379 | ✅         |
| 7주차 | p.382~430 | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->


# 1️⃣ 개념 정리 

## 01. 객체지향 API로 그래프 꾸미기

---

## pyplot 방식 vs 객체지향 API 방식

맷플롯립으로 그래프를 그리는 방법 두 가지.

- **pyplot 방식**: `matplotlib.pyplot`에 있는 함수를 사용. 함수들이 **하나의 피겨 객체에 대한 상태를 공유**함
- **객체지향 API 방식**: 명시적으로 피겨 객체와 서브플롯(Axes) 객체를 만들고, 이 객체의 메서드를 사용

```python
# pyplot 방식
plt.plot([1, 4, 9, 16])
plt.title('simple line graph')
plt.show()

# 객체지향 API 방식 (동일한 결과)
fig, ax = plt.subplots()   # 하나의 Axes 객체를 가지는 피겨 생성
ax.plot([1, 4, 9, 16])
ax.set_title('simple line graph')
fig.show()
```

> 📝 `plot()`에 리스트를 **하나만** 전달하면, 맷플롯립은 그 리스트를 y축 값으로 인식하고 리스트의 인덱스를 x축 값으로 사용함. 예: `[1, 4, 9, 16]` → y값, x값은 `[0, 1, 2, 3]`.

두 방식 모두 결과는 동일하지만, **복잡한 그래프**(특히 하나의 피겨에 여러 서브플롯을 추가하는 경우)에는 **객체지향 방식**이 좋음. 이 책에서는 간단한 경우 pyplot 방식, 피겨 설정을 바꾸거나 서브플롯을 추가할 때는 객체지향 API를 혼용함.


### 폰트 지정하기 (1) — `font.family` 속성

```python
plt.rcParams['font.family']       # 기본값: ['sans-serif']
plt.rcParams['font.family'] = 'NanumGothic'   # 나눔고딕 폰트로 변경
```

### 폰트 지정하기 (2) — `rc()` 함수

`rc()` 함수의 첫 번째 매개변수에는 **설정할 그룹**을, 두 번째 매개변수부터는 그룹의 하위 속성을 **키워드 매개변수**로 지정.

```python
plt.rc('font', family='NanumBarunGothic')             # 폰트만 설정
plt.rc('font', family='NanumBarunGothic', size=11)     # 폰트 패밀리와 크기를 동시에 설정
```

> 💡 `rc()` 함수는 한 그룹 내 여러 설정을 동시에 지정할 수 있음. 설정값은 `plt.rcParams['font.family']`로 다시 확인 가능.

> 📝 `findSystemFonts()` 함수로 시스템에 설치된 전체 폰트 목록 확인 가능.

---

### 고유한 출판사 목록 만들기

```python
top30_pubs = ns_book7['출판사'].value_counts()[:30]   # 카운트 내림차순, 상위 30개 (슬라이스는 범위 미포함이라 30으로 지정)
```

**불리언 인덱스 만들기 — `isin()`**

```python
top30_pubs_idx = ns_book7['출판사'].isin(top30_pubs.index)
```
→ 상위 30개에 해당하는 출판사는 True, 아니면 False로 반환됨 (원본 데이터프레임과 길이가 같은 불리언 배열).

```python
top30_pubs_idx.sum()   # True 개수 세기 (불리언 True=1, False=0으로 처리됨) → 51886
```

### 표본 추출 — `sample()`

산점도로 그리기에도 여전히 많아 1,000개만 무작위로 선택.

```python
ns_book8 = ns_book7[top30_pubs_idx].sample(1000, random_state=42)
```
- `sample()`의 첫 번째 매개변수: 선택할 행 개수
- `random_state`: 넘파이 `seed()`와 비슷한 역할. 동일 값 전달 시 항상 같은 결과

### 산점도 그리기

```python
fig, ax = plt.subplots(figsize=(10, 8))
ax.scatter(ns_book8['발행년도'], ns_book8['출판사'])
ax.set_title('출판사별 발행 도서')
fig.show()
```

---

## 값에 따라 마커 크기를 다르게 나타내기 — `s` 매개변수

`scatter()`의 **s 매개변수**로 마커 크기 지정 가능. 선 그래프·산점도의 기본 마커 크기는 `rcParams['lines.markersize']`(기본값 6)이며, `s`의 기본값은 이 값의 **제곱**.

- `s`에 **하나의 실수**를 전달 → 모든 마커 크기가 동일하게 바뀜
- `s`에 **데이터와 동일한 길이의 배열**을 전달 → 데이터마다 마커 크기가 다른 산점도

```python
ax.scatter(ns_book8['발행년도'], ns_book8['출판사'], s=ns_book8['대출건수'])
```
→ 대출건수가 많은 도서가 상대적으로 크게 그려짐.

## 마커 꾸미기

| 매개변수 | 설명 |
|---|---|
| `alpha` | 마커의 투명도. 마커가 많이 겹치면 진하게 나타남 |
| `edgecolors` | 마커 테두리 색 (기본값은 마커 색을 뜻하는 'face'). 'k'(검은색) 등 지정 시 여러 마커가 겹칠 때 경계 구분에 유용 |
| `linewidths` | 마커 테두리 선 두께 (기본값 1.5) |
| `c` | 산점도의 색. `s`처럼 데이터 개수와 동일한 길이의 배열을 전달하면 값에 따라 다른 색으로 표현 |

```python
fig, ax = plt.subplots(figsize=(10, 8))
ax.scatter(ns_book8['발행년도'], ns_book8['출판사'],
           linewidths=0.5, edgecolors='k', alpha=0.3,
           s=ns_book8['대출건수']*2, c=ns_book8['대출건수'])
ax.set_title('출판사별 발행 도서')
fig.show()
```

> ⚠️ 마커 크기를 이용해 데이터를 교묘하게 왜곡시킬 수 있으므로 주의. 마커 크기로 데이터를 표현한다면 **어떤 방식으로 그렸는지 정보를 제공**하는 것이 좋음.

---

## 값에 따라 색상 표현하기: 컬러맵

**컬러맵(color map)**은 값에 따라 색상을 다르게 표현하기 위해 사전에 정의한 색상 리스트.

- 기본 컬러맵: **viridis** (`scatter()`의 기본값)
- 자주 쓰는 컬러맵: **jet** — 낮은 값일수록 짙은 파란색, 높은 값으로 갈수록 노란색 → 붉은색

컬러맵은 `cmap` 매개변수로 지정.

```python
fig, ax = plt.subplots(figsize=(10, 8))
sc = ax.scatter(ns_book8['발행년도'], ns_book8['출판사'],
                 linewidths=0.5, edgecolors='k', alpha=0.3,
                 s=ns_book8['대출건수']**1.3, c=ns_book8['대출건수'], cmap='jet')
ax.set_title('출판사별 발행 도서')
fig.colorbar(sc)   # 컬러 막대 추가
fig.show()
```

**컬러 막대(color bar)**는 컬러맵의 색깔이 어떤 값에 대응하는지 참조 정보를 제공하는 막대. `scatter()`가 반환하는 객체(sc)를 `colorbar()` 메서드에 전달하면 됨.

---

## 02. 맷플롯립의 고급 기능 배우기

---

## 하나의 피겨에 여러 개의 선 그래프 그리기

여러 선 그래프를 그리는 방법은 **`plot()` 함수를 여러 번 호출**하는 것.


### 선 그래프 2개 그리기

```python
line1 = ns_book9[ns_book9['출판사'] == '황금가지']
line2 = ns_book9[ns_book9['출판사'] == '비룡소']

fig, ax = plt.subplots(figsize=(8, 6))
ax.plot(line1['발행년도'], line1['대출건수'])
ax.plot(line2['발행년도'], line2['대출건수'])
ax.set_title('연도별 대출건수')
fig.show()
```

> 📝 `plot()`을 별도 색 지정 없이 여러 번 호출하면, 맷플롯립이 자동으로 선 그래프마다 다른 색을 순환 지정함. 기본은 10가지 색을 돌아가며 사용하고, 10개 이상 선을 그리면 처음 색부터 반복됨.


### for 문 + 슬라이스로 여러 선 그래프

```python
fig, ax = plt.subplots(figsize=(8, 6))
for pub in top30_pubs.index[:5]:
    line = ns_book9[ns_book9['출판사'] == pub]
    ax.plot(line['발행년도'], line['대출건수'], label=pub)
ax.set_title('연도별 대출건수')
ax.legend()
ax.set_xlim(1985, 2025)   # x축 범위 지정
fig.show()
```

- **`set_xlim()`**: x축 좌표 범위 지정 (첫 번째 매개변수 최솟값, 두 번째 매개변수 최댓값). y축은 **`set_ylim()`**
- pyplot 방식에서는 `plt.xlim()`, `plt.ylim()` 사용. `axis()` 함수로 x·y축 범위를 리스트로 한 번에 지정 가능: `plt.axis([1985, 2025, 0, 13000])` (객체지향 API에서도 동일하게 `ax.axis([...])`)

> 💡 선이 여러 개면 서로 많이 교차해 구분이 어려움 → 이런 경우 **스택 영역 그래프**가 더 좋음.

---

## 스택 영역 그래프 — `stackplot()`

**스택 영역 그래프(stacked area graph)**는 하나의 선 그래프 위에 다른 선 그래프를 차례대로 쌓는 것. 그래프 사이의 간격이 y축 값이 됨.

`stackplot()`의 첫 번째 매개변수에 x축 값, 두 번째 매개변수에 **y축 값을 2차원 배열**로 전달.

### 단계별 작업

**① `pivot_table()`로 '발행년도' 열의 값을 열로 바꾸기**

**피벗 테이블(pivot table)**은 데이터프레임 구조를 바꾸는 방법 — 엑셀의 피벗 테이블과 유사. `index` 매개변수에 행 인덱스로 사용할 열 이름, `columns` 매개변수에 열 인덱스로 사용할 열 이름을 지정하면 각 열의 고유한 값이 피벗 테이블의 인덱스와 열이 됨.

```python
ns_book10 = ns_book9.pivot_table(index='출판사', columns='발행년도')
```

> 📝 결과 열은 **다단(MultiIndex)**으로 구성됨 — 첫 번째 열은 `1947`이 아니라 `('대출건수', 1947)`.

**② '발행년도' 열을 리스트 형태로 바꾸기**

다단 열 이름에서 선택한 항목만 가져오는 **`get_level_values()`** 메서드 사용.

```python
top10_pubs = top30_pubs.index[:10]
year_cols = ns_book10.columns.get_level_values(1)
```

**③ `stackplot()`으로 스택 영역 그래프 그리기**

```python
fig, ax = plt.subplots(figsize=(8, 6))
ax.stackplot(year_cols, ns_book10.loc[top10_pubs].fillna(0), labels=top10_pubs)
ax.set_title('연도별 대출건수')
ax.legend(loc='upper left')   # 범례 위치 지정
ax.set_xlim(1985, 2025)
fig.show()
```

- `legend()`의 **loc** 매개변수로 범례 위치 지정 (기본값 'best' — 최상의 위치 자동 선택)
- 그래프 사이 영역은 자동으로 색칠됨 (역시 10개 색 반복)

> ⚠️ **`fillna()`를 쓰는 이유**: 맷플롯립은 판다스의 누락된 값을 제대로 처리하지 못해 그래프가 이상하게 그려질 수 있음. 그리기 전에 누락값을 0으로 채워야 함. 또는 `pivot_table()` 호출 시 `fill_value=0`으로 지정해도 됨.

---

## 하나의 피겨에 여러 개의 막대 그래프 그리기 — `bar()`

막대 그래프도 `bar()`를 여러 번 호출하면 됨. 그런데 막대 그래프는 **막대에 색이 채워지므로** 그냥 연이어 호출하면 먼저 그린 막대를 덮어씀.

### 막대를 나란히 — width + 위치 이동

막대 기본 너비(0.8)의 절반(0.4)으로 줄이고, x축에서 막대 하나 너비의 절반씩 떨어뜨려 그림.

```python
fig, ax = plt.subplots(figsize=(8, 6))
ax.bar(line1['발행년도']-0.2, line1['대출건수'], width=0.4, label='황금가지')
ax.bar(line2['발행년도']+0.2, line2['대출건수'], width=0.4, label='비룡소')
ax.set_title('연도별 대출건수')
ax.legend()
fig.show()
```

> 많은 막대를 나란히 놓으려면 너비를 아주 얇게 하거나 그래프 폭을 많이 넓혀야 해서 비효율적 → **스택 막대 그래프**가 대안.

### 스택 막대 그래프 — `bar()` + `bottom`

막대를 옆으로 나란히 놓지 않고 위로 쌓는 그래프. 맷플롯립에는 `stackplot()` 같은 전용 함수가 없어, **`bar()`의 bottom 매개변수**로 수동 적용. `bottom`은 막대가 시작할 y좌표를 결정.

```python
height1 = [5, 4, 7, 9, 8]
height2 = [3, 2, 4, 1, 2]

plt.bar(range(5), height1, width=0.5)
plt.bar(range(5), height2, bottom=height1, width=0.5)   # height1 막대가 끝나는 위치에서 시작
plt.show()
```

**데이터값을 미리 누적해서 그리기** — 막대 그래프를 그릴 때마다 시작 위치를 누적 보관하는 건 번거로움. 미리 누적해 놓고(`height3 = height1 + height2`) **가장 큰 막대부터** 그리는 방식이 더 간단.

```python
height3 = [a + b for a, b in zip(height1, height2)]   # 리스트 내포 + zip()으로 양쪽 데이터를 하나씩 엮음
plt.bar(range(5), height3, width=0.5)   # 큰 막대 먼저
plt.bar(range(5), height1, width=0.5)   # 작은 막대 나중 (bottom 없이도 덮어 그려짐)
plt.show()
```

### 판다스 `cumsum()`으로 누적

```python
ns_book10.loc[top10_pubs[:5], ('대출건수',2013):('대출건수',2020)].cumsum()
```
→ `cumsum()`은 **기본적으로 행을 따라** 값을 누적함 (axis=1 지정 시 열 방향 누적도 가능).

```python
ns_book12 = ns_book10.loc[top10_pubs].cumsum()

fig, ax = plt.subplots(figsize=(8, 6))
for i in reversed(range(len(ns_book12))):   # 누적 합계가 가장 큰 마지막 출판사부터 그림
    bar = ns_book12.iloc[i]      # 행 추출
    label = ns_book12.index[i]   # 출판사 이름 추출
    ax.bar(year_cols, bar, label=label)
ax.set_title('연도별 대출건수')
ax.legend(loc='upper left')
ax.set_xlim(1985, 2025)
fig.show()
```

> ⚠️ 데이터값을 누적하여 막대 그래프를 그릴 때는 **가장 큰 막대를 먼저 그려야** 함. 그렇지 않으면 큰 막대가 이전에 그린 작은 막대를 모두 덮어씀.

---

## 원 그래프 그리기 — `pie()`

**원 그래프(파이 차트, pie chart)**는 전체 데이터에 대한 비율을 원의 부채꼴로 나타낸 그래프.

```python
data = top30_pubs[:10]          # 상위 10개 출판사의 도서 개수
labels = top30_pubs.index[:10]  # 상위 10개 출판사의 인덱스(이름)

fig, ax = plt.subplots(figsize=(8, 6))
ax.pie(data, labels=labels)
ax.set_title('출판사 도서 비율')
fig.show()
```
- 첫 번째 매개변수(`data`)를 전달하면 자동으로 전체에 대한 비율을 계산해 그림
- 부채꼴 모양 위에 표시할 이름은 `labels` 매개변수
- **3시 방향부터 반시계 방향**으로 그려짐 (data 배열 순서대로)

### 원 그래프의 단점

선·막대 그래프와 달리 시각적으로 어떤 데이터가 더 큰지 한눈에 구분하기 어려움 (특히 3차원 원 그래프). → 비율 표시와 특정 조각 강조로 보완.

### 비율 표시하고 부채꼴 강조하기

| 매개변수 | 설명 |
|---|---|
| `startangle` | 원 그래프의 시작 위치 지정 (예: 90 → 12시 방향부터 시작) |
| `autopct` | 파이썬 `%` 연산자에 적용할 포맷팅 문자열 전달. 각 부채꼴 비율을 표시. 예: `'%.1f%%'` → 소수점 첫째 자리까지 실수 + % 기호 |
| `explode` | 특정 부채꼴을 원에서 떨어뜨려 강조. 떨어뜨릴 간격을 반지름 비율로 지정한 리스트 (data 배열과 길이가 같아야 함). 예: 첫 항목만 0.1, 나머지 0 |

```python
fig, ax = plt.subplots(figsize=(8, 6))
ax.pie(data, labels=labels, startangle=90,
       autopct='%.1f%%', explode=[0.1]+[0]*9)
ax.set_title('출판사 도서 비율')
fig.show()
```

---

## 여러 종류의 그래프가 있는 서브플롯 그리기

산점도, 스택 영역 그래프, 스택 막대 그래프, 원 그래프를 **하나의 피겨에 2×2 서브플롯**으로 모두 그리기.

```python
fig, axes = plt.subplots(2, 2, figsize=(20, 16))

# 산점도 (서브플롯 안 컬러 막대는 colorbar()의 ax 매개변수에 해당 서브플롯 객체 지정)
sc = axes[0, 0].scatter(ns_book8['발행년도'], ns_book8['출판사'], ...)
fig.colorbar(sc, ax=axes[0, 0])

# 스택 영역 그래프
axes[0, 1].stackplot(year_cols, ns_book10.loc[top10_pubs].fillna(0), labels=top10_pubs)

# 스택 막대 그래프
for i in reversed(range(len(ns_book12))):
    axes[1, 0].bar(year_cols, ns_book12.iloc[i], label=ns_book12.index[i])

# 원 그래프
axes[1, 1].pie(data, labels=labels, startangle=90, autopct='%.1f%%', explode=[0.1]+[0]*9)

fig.savefig('all_in_one.png')   # 이미지 파일로 저장
fig.show()
```

> 📝 `subplots()`의 첫 번째·두 번째 매개변수로 서브플롯의 행·열 개수 지정 가능 (예: `subplots(2, 2)` → 2행 2열 4개 서브플롯). 반환된 Axes 객체는 2차원 배열처럼 `axes[행, 열]`로 위치를 지정해 접근.



**스택 막대 그래프 — `plot.bar(stacked=True)`**

```python
fig, ax = plt.subplots(figsize=(8, 6))
ns_book11.loc[1985:2025, top10_pubs].plot.bar(ax=ax, title='연도별 대출건수', stacked=True, width=0.8)
ax.legend(loc='upper left')
fig.show()
```
- `stacked=True`만 지정하면 스택 막대 그래프가 그려짐 (cumsum() 호출 불필요)
- 판다스 막대 그래프 기본 두께는 0.5

> 💡 판다스는 편리한 그래프 기능을 제공하지만 맷플롯립의 모든 기능을 제공하지는 않음. 세밀한 제어가 필요하면 맷플롯립을 사용하는 것이 좋음.

---


# 2️⃣ 수행 인증

<!-- 교재에서 안내된 과정을 직접 실행해본 뒤, 진행 결과가 보이도록 4~6장의 스크린샷을 캡처하여 아래에 첨부해주세요.-->

<img width="892" height="871" alt="image" src="https://github.com/user-attachments/assets/5c4a5855-ab70-4c4b-a290-c6fb8ca4ab90" />
<img width="892" height="872" alt="image" src="https://github.com/user-attachments/assets/839ddf0c-5887-45c4-90f9-ca9169e86382" />
<img width="930" height="895" alt="image" src="https://github.com/user-attachments/assets/b0795868-7036-4334-8f45-d52a7c481264" />
<img width="890" height="877" alt="image" src="https://github.com/user-attachments/assets/6eb6d539-e3a1-4038-b0ce-2f40067b4956" />
<img width="887" height="895" alt="image" src="https://github.com/user-attachments/assets/d51e63b5-b599-4daf-9fd5-d4b23d9b32af" />




<br>
<br>

# 3️⃣ 확인 문제

## 문제 1.

> **🧚Q. 이번 주차에는 확인문제 대신 그래프 그리기 실습을 진행합니다.
4주차에서 사용했던 캐글 데이터셋을 활용하여, 다양한 요소를 포함한 복잡한 그래프를 직접 작성해주세요.**

```
[https://colab.research.google.com/drive/18yCjsgh8di4dwe6_RXWmbp8r6ua7qOpB?usp=sharing]
```



### 🎉 수고하셨습니다.
