# 데이터분석 4주차 정규과제

📌데이터분석 정규과제는 매주 정해진 분량의 『*혼자 공부하는 데이터 분석 with 파이썬*』 을 읽고 학습하는 것입니다. 이번 주는 아래의 **DataAnalysis_4th_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=HNlRYQnLkek&list=PLVsNizTWUw7FGzSRCkQrPEEe-ljVXgS7k&index=8
https://www.youtube.com/watch?v=Cbk_tQtuhbM&list=PLVsNizTWUw7FGzSRCkQrPEEe-ljVXgS7k&index=9
-->


## DataAnalysis_4th_TIL

### 4장 데이터 요약하기
#### 01. 통계로 요약하기
#### 02. 분포 요약하기


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~81    | ✅         |
| 2주차 | p.84~151   | ✅         |
| 3주차 | p.154~219  | ✅         |
| 4주차 | p.222~279 | ✅         |
| 5주차 | p.282~325 | 🍽️         |
| 6주차 | p.328~379 | 🍽️         |
| 7주차 | p.382~430 | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->


# 1️⃣ 개념 정리 

## 01. 통계로 요약하기

## 핵심 키워드
`평균` `중앙값` `분위수` `분산` `표준편차` `최빈값`

---

## 기술통계란?

**기술통계(descriptive statistics)**는 자료의 내용을 압축하여 설명하는 방법. 다른 말로는 **요약 통계(summary statistics)**라고도 함.

- 정량적인 수치로 전체 데이터의 특징을 요약하거나, 이해하기 쉬운 간단한 그래프를 사용함
- 대표적인 통계량에는 평균, 표준편차 등이 있음
- 데이터 시각화를 아우르는 이러한 데이터 분석 방법을 **탐색적 데이터 분석(exploratory data analysis)**이라고 함

---

## describe() 메서드로 기술통계 구하기

```python
import gdown
gdown.download('https://bit.ly/3736JWl', 'ns_book6.csv', quiet=False)

import pandas as pd
ns_book6 = pd.read_csv('ns_book6.csv', low_memory=False)
ns_book6.describe()
```

`describe()`는 기본적으로 **수치형 열**에 대한 요약 통계를 보여줌.

| 통계량 | 설명 |
|---|---|
| count | 누락된 값을 제외한 데이터 개수 |
| mean | 평균 |
| std | 표준편차 |
| min | 최솟값 |
| 25%, 50%, 75% | 분위수 (50%가 중앙값) |
| max | 최댓값 |

### 도서권수가 0인 데이터 제외하기

```python
sum(ns_book6['도서권수']==0)  # 3206
ns_book7 = ns_book6[ns_book6['도서권수']>0]
```

### percentiles 매개변수로 원하는 위치 값 보기

```python
ns_book7.describe(percentiles=[0.3, 0.6, 0.9])
```

### include 매개변수로 문자형 열 통계 보기

```python
ns_book7.describe(include='object')
```

- `count`: 누락값 제외 데이터 개수
- `unique`: 고유한 값의 개수
- `top`: 가장 많이 등장하는 값
- `freq`: top 값의 등장 빈도수

---

## 평균 구하기

$$평균 = \frac{x_1 + x_2 + x_3}{3} = \frac{\sum_{i=1}^{3} x_i}{3}$$

- Σ(시그마) 기호는 반복적인 덧셈을 간단히 표현하는 수학 기호
- 아래에는 시작 인덱스, 위에는 종료 인덱스를 지정함

```python
ns_book7['대출건수'].mean()
# 11.593438968070707
```

---

## 중앙값 구하기

**중앙값(median)**은 전체 데이터를 순서대로 늘어놓았을 때 중앙에 위치한 값.

```python
ns_book7['대출건수'].median()
# 6.0
```

- 데이터 개수가 **홀수**면 정확히 가운데 값
- 데이터 개수가 **짝수**면 가운데 두 값의 평균

```python
temp_df = pd.DataFrame([1,2,3,4])
temp_df.median()
# 2.5
```

### 중복값 제거하고 중앙값 구하기

```python
ns_book7['대출건수'].drop_duplicates().median()
# 183.0
```

중복된 값을 제거하니 중앙값이 크게 높아짐 → 작은 대출건수에 중복된 행이 많다는 의미로 해석 가능.

---

## 최솟값, 최댓값 구하기

```python
ns_book7['대출건수'].min()  # 0
ns_book7['대출건수'].max()  # 1765
```

---

## 분위수 구하기

**분위수(quantile)**는 데이터를 순서대로 늘어놓았을 때 균등한 간격으로 나누는 기준점.

- **이분위수**: 데이터를 2구간으로 나눔 → 중앙값과 동일
- **사분위수(quartile)**: 데이터를 4구간(25%, 50%, 75%)으로 나눔
- **백분위수(percentile)**: 데이터를 100구간으로 나눔

```python
ns_book7['대출건수'].quantile(0.25)
# 2.0

ns_book7['대출건수'].quantile([0.25, 0.5, 0.75])
# 0.25     2.0
# 0.50     6.0
# 0.75    14.0
```

### interpolation 매개변수 (보간)

```python
pd.Series([1,2,3,4,5]).quantile(0.9)
# 4.6  (기본값 'linear': 양쪽 분위수에 비례하여 결정)

pd.Series([1,2,3,4,5]).quantile(0.9, interpolation='midpoint')
# 4.5  (두 수 사이의 중앙값)

pd.Series([1,2,3,4,5]).quantile(0.9, interpolation='nearest')
# 5  (더 가까운 값 선택)
```

### 백분위 구하기 (반대로 계산)

```python
borrow_10_flag = ns_book7['대출건수'] < 10
borrow_10_flag.mean()
# 0.6402712530190833

ns_book7['대출건수'].quantile(0.65)
# 10.0
```

불리언 배열은 산술 연산 시 True=1, False=0으로 취급되므로, `mean()`을 호출하면 조건을 만족하는 비율을 구할 수 있음.

---

## 분산 구하기

**분산(variance)**은 평균으로부터 데이터가 얼마나 퍼져있는지를 나타내는 통계량.

$$s^2 = \frac{\sum_{i=1}^{n}(x_i - \bar{x})^2}{n}$$

- 각 값에서 평균을 뺀 후 제곱하여 평균처럼 개수로 나눔
- 제곱하는 이유: 음수가 되는 것을 막아 평균을 중심으로 좌우 값이 상쇄되지 않도록 하기 위함
- $\bar{x}$: 평균 (인덱스에 따라 변하지 않는 상수)

```python
ns_book7['대출건수'].var()
# 371.69563042906674
```

---

## 표준편차 구하기

**표준편차(standard deviation)**는 분산에 제곱근을 취한 것.

$$s = \sqrt{\frac{\sum_{i=1}^{n}(x_i - \bar{x})^2}{n}}$$

```python
ns_book7['대출건수'].std()
# 19.279409493785508
```

표준편차는 평균을 중심으로 데이터가 대략 얼만큼 떨어져 분포해 있는지를 표현하는 값.

### 수식을 코드로 직접 구현해보기

```python
import numpy as np
diff = ns_book7['대출건수'] - ns_book7['대출건수'].mean()
np.sqrt(np.sum(diff**2)/(len(ns_book7)-1))
# 19.279409493785508
```

> ⚠️ 판다스의 `std()`를 수식으로 재현하려면 분모를 **n이 아니라 n-1**로 나눠야 함. (자유도, degree of freedom)

---

## 최빈값 구하기

**최빈값(mode)**은 데이터에서 가장 많이 등장하는 값.

```python
ns_book7['도서명'].mode()
# 0    승정원일기

ns_book7['발행년도'].mode()
# 0    2012.0
```

---

## 데이터프레임에서 기술통계 구하기

수치형 열만 연산 가능하므로 `numeric_only=True`를 지정해야 함.

```python
ns_book7.mean(numeric_only=True)
# 번호          202977.476649
# 발행년도         2008.460076
# 도서권수            1.145540
# 대출건수           11.593439
```

```python
ns_book7.loc[:, '도서명':].mode()
```

> mode() 메서드 출력 결과물 사이에는 서로 연관이 없다는 것에 주의. (예: '승정원일기'가 '문학동네' 출판사라는 뜻이 아님)

### CSV로 저장

```python
ns_book7.to_csv('ns_book7.csv', index=False)
```

---

## 좀 더 알아보기 - 넘파이의 기술통계 함수

### 평균

```python
import numpy as np
np.mean(ns_book7['대출건수'])
# 11.593438968070707
```

**가중 평균 (weighted average)**

$$가중평균 = \frac{x_1 \times w_1 + x_2 \times w_2}{w_1 + w_2} = \frac{\sum_{i=1}^{2} x_i \times w_i}{\sum_{i=1}^{2} w_i}$$

```python
np.average(ns_book7['대출건수'], weights=1/ns_book7['도서권수'])
# 10.543612175385386
```

같은 데이터라도 평균을 구하는 방식(단순 평균, 가중 평균, 비율로 나눈 후 평균, 총합으로 나누기 등)에 따라 값이 달라질 수 있음.

```python
np.mean(ns_book7['대출건수']/ns_book7['도서권수'])
# 9.873029861445774

ns_book7['대출건수'].sum()/ns_book7['도서권수'].sum()
# 10.120503701300958
```

> 💡 어떤 방식으로 계산되었는지 정확한 정보를 제공하지 않으면 요약된 통계량은 오해를 일으키기 쉬움.

### 중앙값

```python
np.median(ns_book7['대출건수'])
# 6.0
```

### 최솟값, 최댓값

```python
np.min(ns_book7['대출건수'])  # 0
np.max(ns_book7['대출건수'])  # 1765
```

### 분위수

```python
np.quantile(ns_book7['대출건수'], [0.25, 0.5, 0.75])
# array([ 2., 6., 14.])
```

### 분산 - n vs n-1 자유도 차이 ⭐

```python
np.var(ns_book7['대출건수'])
# 371.6946438971496   (판다스와 다름)

ns_book7['대출건수'].var()
# 371.69563042906674
```

**핵심 차이점**

| 구분 | 분모 | ddof 기본값 | 용도 |
|---|---|---|---|
| 판다스 `var()` | n-1 | 1 | 표본집단으로 모집단 특징 추정 |
| 넘파이 `var()` | n | 0 | 단순히 일련의 데이터 분산 계산 |

- **표본집단(sample)**: 전체 데이터 중 수집한 일부 데이터
- **모집단(population)**: 전체 데이터
- **자유도(degree of freedom)**: 평균과 n-1개의 샘플 값을 알면 마지막 값은 자동으로 알 수 있음 → n-1

`ddof` 매개변수로 자유도 차감값을 지정할 수 있음.

```python
ns_book7['대출건수'].var(ddof=0)   # 판다스, 371.6946438971496
np.var(ns_book7['대출건수'], ddof=1)  # 넘파이, 371.69563042906674
```

> 데이터 개수가 충분히 많으면 n과 n-1의 차이가 크지 않아 실무적으로는 자유도를 크게 고려할 필요 없음.

### 표준편차

```python
np.std(ns_book7['대출건수'])
# 19.27938390865096
```

### 최빈값 (넘파이는 직접 제공 X → unique() 활용)

```python
values, counts = np.unique(ns_book7['도서명'], return_counts=True)
max_idx = np.argmax(counts)
values[max_idx]
# '승정원일기'
```

---

## 핵심 함수와 메서드

| 함수/메서드 | 기능 |
|---|---|
| `DataFrame.describe()` | 데이터프레임의 기술통계량 출력 |
| `Series.mean()` / `numpy.mean()` | 평균 계산 |
| `Series.median()` / `numpy.median()` | 중앙값 계산 |
| `Series.quantile()` / `numpy.quantile()` | 분위수 계산 |
| `Series.var()` / `numpy.var()` | 분산 계산 |
| `Series.std()` / `numpy.std()` | 표준편차 계산 |
| `Series.mode()` | 최빈값 계산 |


## 02. 분포 요약하기

## 핵심 키워드
`맷플롯립` `산점도` `히스토그램` `도수` `로그 스케일` `상자 수염 그림`

---

## 왜 그래프로 요약하는가?

- 평균이나 중앙값은 이해하기 쉽지만, 분위수·분산·표준편차 등은 숫자만으로 직관적으로 와닿지 않음
- 전체 데이터를 한눈에 파악하려면 그래프가 가장 좋은 방법
- 이번 절에서 다루는 세 가지 그래프: **산점도(scatter plot)**, **히스토그램(histogram)**, **상자 수염 그림(box-and-whisker plot)**

---

## 산점도 그리기

**산점도**는 데이터를 화면에 뿌리듯 그리는 그래프. 두 **변수(variable)** 혹은 **특성(feature)** 값을 직교 좌표계에 점으로 나타냄.

> 변수/특성: 프로그래밍의 변수와는 다른 개념. 데이터 분석에서는 한 레코드를 구성하는 개별 측정값을 의미함. 통계학에서는 '변수', 이 책에서는 주로 '특성'이라는 용어를 사용.

- 수평축 = x축, 수직축 = y축
- 그래프는 **맷플롯립(matplotlib)** 패키지로 그림 (`matplotlib.pyplot`을 보통 `plt`로 줄여서 사용)

```python
import matplotlib.pyplot as plt
```

### scatter() 함수

```python
plt.scatter([1,2,3,4], [1,2,3,4])
plt.show()
```

- 첫 번째 매개변수: x축 좌표
- 두 번째 매개변수: y축 좌표

데이터프레임 열을 그대로 넣어도 됨:

```python
plt.scatter(ns_book7['번호'], ns_book7['대출건수'])
plt.show()
```

> '번호' 열은 일련번호일 뿐 의미가 없는 값이라, 대출건수가 번호에 따라 특별한 경향 없이 고르게 퍼져 있음.

### 투명도 조절하기 (alpha)

데이터가 겹쳐서 잘 안 보일 때, `alpha` 매개변수(0~1)로 투명도를 지정하면 중첩된 부분이 짙게 표시되어 밀집도를 가늠하기 좋음.

```python
plt.scatter(ns_book7['도서권수'], ns_book7['대출건수'], alpha=0.1)
plt.show()
```

- 0에 가까울수록 투명, 1에 가까울수록 불투명

### 상관관계 파악하기

- x가 증가할 때 y도 증가 → **양의 상관관계**
- x가 증가할 때 y는 감소 → **음의 상관관계**

```python
average_borrows = ns_book7['대출건수']/ns_book7['도서권수']
plt.scatter(average_borrows, ns_book7['대출건수'], alpha=0.1)
plt.show()
```

→ '도서권수 당 대출건수'가 늘어날수록 '대출건수'도 늘어나는 양의 상관관계가 뚜렷하게 보임.

> 산점도는 2차원(또는 3차원)까지만 가능해서, 표현할 수 있는 특성 개수에 한계가 있음.

---

## 히스토그램 그리기

**히스토그램**은 수치형 특성의 값을 일정한 **구간(bin, 계급)**으로 나누어, 구간 안에 포함된 데이터 개수(**도수, frequency**)를 막대로 그린 그래프.

### hist() 함수

```python
plt.hist([0,3,5,6,7,7,9,13], bins=5)
plt.show()
```

- `bins`: 구간 개수 지정 (기본값 10)
- 어느 구간에 데이터가 몰려 있는지 한눈에 보이지만, 구간 경계 수치는 그래프만으로 확인하기 어려움

### 구간 경곗값 확인하기

```python
import numpy as np
np.histogram_bin_edges([0,3,5,6,7,7,9,13], bins=5)
# array([ 0. , 2.6, 5.2, 7.8, 10.4, 13. ])
```

- 경곗값은 다음 구간에 포함됨 (예: 2.6은 두 번째 구간 소속)
- 마지막 구간은 최댓값을 포함

### 정규분포와 표준정규분포

- **정규분포(normal distribution)**: 종 모양처럼 가운데가 볼록하고 평균을 중심으로 대칭인 분포
- **표준정규분포(standard normal distribution)**: 평균 0, 표준편차 1인 정규분포

```python
np.random.seed(42)
random_samples = np.random.randn(1000)

print(np.mean(random_samples), np.std(random_samples))
# 0.0193... 0.9787...

plt.hist(random_samples)
plt.show()
```

- `randn()`: 표준정규분포를 따르는 난수 생성
- `seed()`: 유사난수(pseudorandom number, 가짜 난수) 생성 → 항상 같은 결과 재현 가능

### 로그 스케일로 구간 조정하기

한 구간의 도수가 너무 커서 다른 구간이 안 보이는 문제를 해결하기 위해, y축에 **로그 스케일(log scale)**을 적용할 수 있음. 큰 값일수록 도수 차이가 압축되어 작은 값과의 차이가 상대적으로 줄어듦.

```python
plt.hist(ns_book7['대출건수'])
plt.yscale('log')
plt.show()
```

- 맷플롯립 기본 로그 함수는 밑이 10 (10⁰, 10¹, 10²...)
- 로그 스케일 그래프를 볼 때, 실제 데이터 격차는 그래프보다 훨씬 크다는 점에 유의
- `hist()` 함수의 `log=True` 매개변수로도 동일한 효과 가능

```python
plt.hist(ns_book7['대출건수'], bins=100)
plt.yscale('log')
plt.show()
```

- `bins` 값을 키우면 데이터 분포를 더 세밀하게 관찰 가능

### x축 로그 스케일 적용

```python
title_len = ns_book7['도서명'].apply(len)
plt.hist(title_len, bins=100)
plt.xscale('log')
plt.show()
```

- `xscale('log')`: x축에도 로그 스케일 적용 가능 (한쪽으로 편중된 분포를 고르게 펼쳐볼 때 사용)

> 히스토그램은 하나의 특성 분포를 보기엔 좋지만, 여러 특성을 비교하려면 축 범위가 달라 비교가 번거로움 → 이럴 때 상자 수염 그림이 유용.

---

## 상자 수염 그림 그리기

**상자 수염 그림(box-and-whisker plot)**은 최솟값, 세 개의 사분위수, 최댓값 등 다섯 개의 숫자로 데이터를 요약하는 그래프.

### 그리는 순서

1. 사분위수 계산 → 25%, 75% 지점을 밑면·윗면으로 하는 직사각형(상자) 그리기
2. 중간값(50%) 위치에 수평선 긋기
3. 상자 밑면·윗면에서 상자 높이의 1.5배 거리 안에서 가장 멀리 있는 샘플까지 수직선(수염) 긋기
4. 수직선 밖의 데이터는 점으로 표시 → 이를 **이상치(outlier)**라고 부름

- **IQR(interquartile range)**: 제1사분위수(25%)와 제3사분위수(75%) 사이 거리

> 상자 수염 그림에서의 이상치는 단순히 IQR의 1.5배 거리 밖 데이터를 의미할 뿐, 데이터 분석에서 반드시 제거해야 하는 대상은 아님. (머신러닝에서 말하는 이상치와는 다른 개념)

### boxplot() 함수

```python
plt.boxplot(ns_book7[['대출건수','도서권수']])
plt.show()
```

- 여러 열을 리스트로 전달하면 여러 상자를 동시에 그림
- 사분위수가 작으면 상자가 잘 안 보일 수 있음 → 이럴 때도 로그 스케일 적용

```python
plt.boxplot(ns_book7[['대출건수','도서권수']])
plt.yscale('log')
plt.show()
```

### 수평으로 그리기 (vert)

```python
plt.boxplot(ns_book7[['대출건수','도서권수']], vert=False)
plt.xscale('log')
plt.show()
```

- `vert=False`: 상자 수염 그림을 수평으로 그림 → 이때는 로그 스케일도 x축에 지정

### 수염 길이 조정하기 (whis)

```python
plt.boxplot(ns_book7[['대출건수','도서권수']], whis=10)
plt.yscale('log')
plt.show()
```

- 기본 수염 길이는 IQR의 1.5배
- `whis` 값을 키우면 더 먼 데이터까지 수염으로 포함

**백분위수로 지정하기**

```python
plt.boxplot(ns_book7[['대출건수','도서권수']], whis=(0,100))
plt.yscale('log')
plt.show()
```

- `whis=(10, 90)` → 10%, 90% 백분위수까지 수염
- `whis=(0, 100)` → 최솟값~최댓값까지 수염 (이상치 없이 전부 표시)

---

## 핵심 함수와 메서드

| 함수/메서드 | 기능 |
|---|---|
| `matplotlib.pyplot.scatter()` | 2차원 평면에 산점도를 그림 |
| `matplotlib.pyplot.hist()` | 히스토그램을 그림 |
| `matplotlib.pyplot.boxplot()` | 상자 수염 그림을 그림 |
| `matplotlib.pyplot.xscale()` | x축의 스케일 지정 |
| `matplotlib.pyplot.yscale()` | y축의 스케일 지정 |
| `numpy.random.seed()` | 임의의 정수를 입력하면 난수 발생을 동일하게 재현 |
| `numpy.random.randn()` | 표준정규분포를 따르는 난수 생성 |



# 2️⃣ 수행 인증

<img width="887" height="877" alt="image" src="https://github.com/user-attachments/assets/a269ee20-3754-4a78-9db0-a2014c68a5a4" />
<img width="900" height="885" alt="image" src="https://github.com/user-attachments/assets/3096dd3c-5b2f-405e-af54-518615d4ab85" />
<img width="876" height="856" alt="image" src="https://github.com/user-attachments/assets/a658b7c3-d08f-4b84-8f9d-462ba8665a6d" />
<img width="885" height="882" alt="image" src="https://github.com/user-attachments/assets/420ae6e7-302c-47e9-92f2-86a8565c578c" />





<br>
<br>

# 3️⃣ 확인 문제

## 문제 1.

> **🧚Q. 이번 주차에는 확인문제 대신 실습 과제를 진행합니다. 캐글에서 원하는 데이터셋을 선택하여 기술통계를 계산하고, 다양한 시각화를 수행해보세요.
작업은 코랩에서 진행한 뒤, 코랩 링크를 아래에 첨부해주세요.**

```
https://colab.research.google.com/drive/120rgPGdV_hRXvRWhhqIMO8OaUwAjgxVe?usp=sharing
```



### 🎉 수고하셨습니다.
