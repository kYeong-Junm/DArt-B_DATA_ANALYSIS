# 데이터분석 5주차 정규과제

📌데이터분석 정규과제는 매주 정해진 분량의 『*혼자 공부하는 데이터 분석 with 파이썬*』 을 읽고 학습하는 것입니다. 이번 주는 아래의 **DataAnalysis_5th_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=ho0LZ6GWhtc&list=PLVsNizTWUw7FGzSRCkQrPEEe-ljVXgS7k&index=10
https://www.youtube.com/watch?v=deYY4xHsI0o&list=PLVsNizTWUw7FGzSRCkQrPEEe-ljVXgS7k&index=11
-->


## DataAnalysis_5th_TIL

### 5장 데이터 시각화하기
#### 01. 맷플롯립 기본 요소 알아보기
#### 02. 선 그래프와 막대 그래프 그리기


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~81    | ✅         |
| 2주차 | p.84~151   | ✅         |
| 3주차 | p.154~219  | ✅         |
| 4주차 | p.222~279 | ✅         |
| 5주차 | p.282~325 | ✅         |
| 6주차 | p.328~379 | 🍽️         |
| 7주차 | p.382~430 | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->


# 1️⃣ 개념 정리 

## 01. 맷플롯립 기본 요소 알아보기

## 핵심 키워드
`피겨` `rcParams` `축` `마커` `서브플롯`

---

## Figure(피겨) 객체
모든 그래프 구성 요소를 담고 있는 **최상위 객체**

- `scatter()` 등을 호출하면 자동으로 피겨 객체가 생성됨
- `figure()` 함수로 **명시적으로** 피겨 객체를 만들면 다양한 옵션(크기 등)을 조절할 수 있음
- `plt.show()`가 호출되면 `figure()`로 만든 피겨 객체는 **자동으로 소멸**됨

### 그래프 크기 바꾸기 — figsize

- `figsize`는 **튜플**로 지정 (리스트와 비슷하지만 소괄호, 한 번 생성하면 수정 불가)
- 단위는 **인치(inch)**, 기본 그래프 크기는 (6, 4)

### 그래프 실제 크기와 DPI

화면에 그려지는 실제 픽셀 크기 = `figsize(인치)` × `DPI`

- **DPI(dot per inch)**: 1인치를 몇 개의 점(픽셀)으로 표현하는지 나타내는 값
- 맷플롯립 기본 DPI는 **72** (모니터 실제 DPI보다 작아서, figsize를 지정해도 화면에 작게 그려짐)
- 원하는 픽셀 크기를 얻으려면: `figsize = (원하는 픽셀값 / DPI, 원하는 픽셀값 / DPI)`

```python
# 900×600 픽셀 크기로 그리고 싶을 때 (DPI 기본값 72 기준)
plt.figure(figsize=(900/72, 600/72))
```

> 📝 코랩은 그래프 출력 시 **타이트(tight) 레이아웃**을 적용해 여백을 최소화하므로, 지정한 figsize보다 실제 이미지가 작게 나올 수 있음. 타이트 레이아웃을 끄려면 `bbox_inches` 옵션을 `None`으로 지정:
> ```python
> %config InlineBackend.print_figure_kwargs = {'bbox_inches': None}
> ```

### 그래프 크기 바꾸기 — dpi 매개변수

`figsize`는 그대로 두고 **DPI 값 자체**를 높이는 방법도 있음.

```python
plt.figure(dpi=144)   # 기본 72의 2배 → 그래프와 내부 모든 구성 요소(글자, 마커 등)가 함께 커짐
```

> 💡 figsize는 그래프를 그리는 **캔버스 크기**, DPI는 그 캔버스를 **확대해서 보는 돋보기**라고 생각하면 이해하기 쉬움.

---

## rcParams 객체

**rcParams**는 맷플롯립 그래프의 **기본값을 관리**하는 객체.

- 값을 출력할 수도, **새로운 값으로 바꿀 수도** 있음 → 바꾸면 **이후에 그려지는 모든 그래프**에 적용됨

```python
# DPI 기본값을 100으로 변경
plt.rcParams['figure.dpi'] = 100
```

### 산점도 마커 모양 바꾸기

```python
plt.rcParams['scatter.marker']      # 기본값 'o' (동그라미)
plt.rcParams['scatter.marker'] = '*'  # 별 모양으로 변경 → 이후 모든 산점도에 적용
```

- 특정 그래프 **하나만** 마커를 바꾸고 싶다면 rcParams 대신 `scatter()` 함수의 **marker 매개변수**를 사용 (rcParams 설정을 무시하고 그 그래프에만 적용됨)

```python
plt.scatter(ns_book7['도서권수'], ns_book7['대출건수'], alpha=0.1, marker='+')
```

> 💡 **전역 기본값**을 바꾸려면 rcParams, **그래프 하나만** 바꾸려면 함수의 매개변수(marker 등) 사용.

---

## 서브플롯(subplot)

하나의 피겨 객체 안에는 여러 개의 **서브플롯**을 담을 수 있음.

- 서브플롯 = 맷플롯립의 **Axes 클래스** 객체
- 하나의 서브플롯은 두 개 이상의 **축(Axis)**을 포함 (2차원이면 x축·y축 2개, 3차원이면 3개)
- 축에는 **눈금(틱, tick)**과 **레이블(label)**이 있음

### subplots() 함수로 서브플롯 그리기

`subplots()` 함수는 **피겨 객체**와 각 서브플롯을 나타내는 **Axes 객체의 배열**을 반환함.

```python
fig, axs = plt.subplots(2)   # 세로로 2개 서브플롯 (2행 1열)

axs[0].scatter(ns_book7['도서권수'], ns_book7['대출건수'], alpha=0.1)
axs[1].hist(ns_book7['대출건수'], bins=100)
axs[1].set_yscale('log')
fig.show()
```
### 서브플롯을 가로로 나란히 — 행/열 지정

`subplots()`의 **첫 번째 매개변수 = 행 개수**, **두 번째 매개변수 = 열 개수**.

---

## 마무리 - 5가지 키워드 정리

| 키워드 | 설명 |
|---|---|
| **피겨(Figure)** | 맷플롯립 그래프 요소를 모두 담는 최상위 객체. 그래프를 그리면 자동 생성되고 `show()` 후 소멸됨. 명시적으로 만들면 다양한 옵션 제어 가능 |
| **rcParams** | 그래프 기본값을 관리하는 객체. 값을 바꾸면 이후 그려지는 모든 그래프에 적용됨 |
| **축(Axis)** | 그래프에서 데이터 좌표를 표현. 2차원은 축 2개, 3차원은 축 3개. 맷플롯립에서는 Axis 클래스로 다룸 |
| **마커** | 그래프에 데이터 포인트를 표시하는 방법. 기본 마커는 동그라미('o') |
| **서브플롯** | 피겨 안에 포함된 그래프 영역(Axes 객체). `subplots()`로 여러 개를 포함하는 피겨를 만들 수 있음 |

## 핵심 함수와 메서드

| 함수/메서드 | 기능 |
|---|---|
| `matplotlib.pyplot.figure()` | 피겨 객체를 만들어 반환 |
| `matplotlib.pyplot.subplots()` | 피겨와 서브플롯을 생성하여 반환 |
| `Axes.set_xscale()` / `set_yscale()` | 서브플롯의 x축/y축 스케일 지정 |
| `Axes.set_title()` | 서브플롯의 제목 설정 |
| `Axes.set_xlabel()` / `set_ylabel()` | 서브플롯의 x축/y축 이름 지정 |


## 02. 선 그래프와 막대 그래프 그리기

## 핵심 키워드
`선 그래프` `막대 그래프`

---

## 데이터 준비 — value_counts(), sort_index()

```python
# 연도별 도서 개수 (고유값의 등장 횟수 계산)
count_by_year = ns_book7['발행년도'].value_counts()
count_by_year = count_by_year.sort_index()   # 인덱스(연도) 기준 오름차순 정렬

# 미래 연도 등 이상치 제거 (부등호로 시리즈 필터링 가능)
count_by_year = count_by_year[count_by_year.index <= 2030]
```

- `value_counts()`: 고유한 값의 등장 횟수 계산, **기본은 값 기준 내림차순 정렬**
- 선 그래프의 x축이 시간순이면 `sort_index()`로 인덱스 기준 재정렬해야 함
- 시리즈 객체도 데이터프레임처럼 **부등호로 조건 필터링** 가능

---

## 선 그래프 그리기 — plot()

```python
plt.plot(count_by_year.index, count_by_year.values)
plt.title('Books by year')
plt.xlabel('year')
plt.ylabel('number of books')
plt.show()
```

- 첫 번째 매개변수 = x축 값, 두 번째 매개변수 = y축 값
- **시리즈 객체를 그대로** 전달하면 자동으로 **인덱스가 x축**, **값이 y축**으로 사용됨 → `plt.plot(count_by_year, ...)`도 가능

### 선 모양 · 색상 · 마커 바꾸기

| 매개변수 | 설명 | 예시 값 |
|---|---|---|
| `linestyle` | 선 모양 (기본 `'-'` 실선) | `'-'`실선, `':'`점선, `'-.'`쇄선, `'--'`파선 |
| `color` | 선 색상 | `'red'` 같은 이름, `'#ff0000'` 같은 16진수 |
| `marker` | 데이터 포인트 표시 (산점도와 동일) | `'.'`, `'*'` 등 |

```python
plt.plot(count_by_year, marker='.', linestyle=':', color='red')
```

**포맷 문자열로 한 번에 지정**: marker + linestyle + color를 문자열 하나로 축약 가능

```python
plt.plot(count_by_year, '*-g')   # 별 마커, 실선, 녹색(green)
```

### 눈금 개수 조절 — xticks() / yticks()

```python
plt.xticks(range(1947, 2030, 10))   # 10년 단위로 x축 눈금 표시
```

- x축 눈금은 `xticks()`, y축 눈금은 `yticks()`
- 서브플롯에서는 각각 `set_xticks()`, `set_yticks()` 메서드 사용 (title()→set_title()과 같은 이름 규칙)

### 마커에 값 표시하기 — annotate()

```python
for idx, val in count_by_year[::5].items():   # 슬라이스 연산자로 5개씩 건너뛰며 선택
    plt.annotate(val, (idx, val))
```

- 첫 번째 매개변수: 표시할 문자열, 두 번째 매개변수: 텍스트가 나타날 (x, y) 좌표(튜플)
- 시리즈의 `items()` 메서드로 (인덱스, 값) 쌍을 순회

**텍스트 위치 조정 — xytext / textcoords**

```python
plt.annotate(val, (idx, val), xytext=(idx+1, val+10))
```
→ 데이터 좌표계 기준 이동. 그런데 y축 스케일이 x축보다 훨씬 크면(예: 0~17500) 거의 티가 안 남.

```python
plt.annotate(val, (idx, val), xytext=(2, 2), textcoords='offset points')
```
→ **상대 위치(포인트/픽셀 단위)**로 이동. `textcoords='offset points'`(1포인트=1/72인치) 또는 `'offset pixels'` 사용.

---

## 막대 그래프 그리기 — bar() / barh()

`bar()` 함수는 `plot()`과 매우 비슷함. x축 값과 막대 높이(y축 값)를 전달.

```python
plt.bar(count_by_subject.index, count_by_subject.values)
plt.title('Books by subject')
plt.xlabel('subject')
plt.ylabel('number of books')
for idx, val in count_by_subject.items():
    plt.annotate(val, (idx, val), xytext=(0, 2), textcoords='offset points')
plt.show()
```

### 텍스트 정렬, 막대 두께, 색상 바꾸기

| 매개변수 | 대상 | 설명 |
|---|---|---|
| `ha` | `annotate()` | 텍스트 수평 정렬. 기본 `'right'`, `'center'`(중앙), `'left'` |
| `fontsize` | `annotate()` | 텍스트가 겹칠 때 크기 축소 |
| `color` | `annotate()` | 텍스트 색상 (bar와 별개로 지정 가능) |
| `width` | `bar()` | 막대 두께. 기본값 0.8 (1로 지정하면 막대 사이 간격 사라짐) |
| `color` | `bar()` | 막대 색상 |


### 가로 막대 그래프 — barh()

`barh()`는 같은 데이터를 **가로**로 그림. 주의할 점:

- 두께 매개변수는 `width`가 아니라 **`height`**
- x축·y축 이름을 서로 바꿔 써야 함
- `annotate()` 좌표도 `(idx, val)`이 아니라 **`(val, idx)`** 순서로 바뀜
- 텍스트 정렬은 `ha`가 아니라 **`va`**(수직 정렬) 사용. 기본값 `'baseline'`, `'center'`/`'top'`/`'bottom'` 선택 가능

---

# 2️⃣ 수행 인증

<!-- 교재에서 안내된 과정을 직접 실행해본 뒤, 진행 결과가 보이도록 4~6장의 스크린샷을 캡처하여 아래에 첨부해주세요.-->

<img width="1177" height="901" alt="image" src="https://github.com/user-attachments/assets/677ed07a-8946-4384-a055-32fa891b45ef" />
<img width="1265" height="902" alt="image" src="https://github.com/user-attachments/assets/612091f4-d2c9-4ae1-8066-476dedc6bed7" />
<img width="1281" height="862" alt="image" src="https://github.com/user-attachments/assets/956fd40c-817a-48f7-b152-639bf67a39e0" />
<img width="1131" height="897" alt="image" src="https://github.com/user-attachments/assets/12a48a04-cd11-490a-9285-c679f4c0ebaf" />
<img width="1222" height="902" alt="image" src="https://github.com/user-attachments/assets/5edd544f-47db-4236-8e0e-de7aaf358540" />


<br>
<br>

# 3️⃣ 확인 문제

## 문제 1.

> **🧚Q. 다음 데이터를 이용하여 matplotlib으로 선그래프를 그리는 코드를 작성해주세요.**
- x = [1, 2, 3, 4, 5]
- y = [2, 4, 6, 8, 10]
> 조건은 아래와 같습니다.
```
1️⃣ 제목은 "Linear Trend"로 설정해주세요.
2️⃣ x축 이름은 "X values"로 설정해주세요.
3️⃣ y축 이름은 "Y values"로 설정해주세요.
4️⃣ 마커(marker)를 포함하여 선그래프를 그려주세요.
```

```
import matplotlib.pyplot as plt

x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

plt.plot(x, y, marker='*') #별 마커
plt.title('Linear Trend')
plt.xlabel('X values')
plt.ylabel('Y values')
plt.show()
```



### 🎉 수고하셨습니다.
