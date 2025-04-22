![image.png](attachment:f285ffc8-15a4-4d30-aee4-20cfba5da806:image.png)

# 미션 목표 및 환경 세팅

---

<aside>
💡

데이터를 통해 고객이 정기 예금을 가입할 가능성을 예측하고,  이를 통해 마케팅 켐페인의 효율성을 높이는 것! 마케팅 담당자로서 정기 예금과 관련이 있는 요소들을 파악해보고, 고객의 행동을 이해해보고, 어떤 상황에서  어떤 고객들이 정기 예금을 가입하는지 분석해보자.

이번 미션의 최종 목표는 가장 정확한 분류 모델을 개발하여 고객이 정기 예금을 가입할지 여부를 예측하고, 그 모델을 통해 도출한 인사이트를 바탕으로 비즈니스 전략을 제시하는 것!

</aside>

위의 미션을 수행하기 위해 물리 컴퓨터에서 작업하는 것보다, 클라우드 컴퓨터 성능이 적합할 것이라고 판단하였기에, Google Colab에서 작업을 진행하였다.

# 데이터 설명

---

| 컬럼명 | 설명 |
| --- | --- |
| age | 나이 (숫자) |
| job | 직업 (범주형) |
| marital | 결혼 여부 (범주형) |
| education | 교육 수준 (범주형) |
| default | 신용 불량 여부 (범주형) |
| housing | 주택 대출 여부 (범주형) |
| loan | 개인 대출 여부 (범주형) |
| contact | 연락 유형 (범주형) |
| month | 마지막 연락 월 (범주형) |
| day_of_week | 마지막 연락 요일 (범주형) |
| duration | 마지막 연락 지속 시간, 초 단위 (숫자) |
| campaign | 캠페인 동안 연락 횟수 (숫자) |
| pdays | 이전 캠페인 후 지난 일수 (숫자) |
| previous | 이전 캠페인 동안 연락 횟수 (숫자) |
| poutcome | 이전 캠페인의 결과 (범주형) |
| emp.var.rate | 고용 변동률 (숫자) |
| cons.price.idx | 소비자 물가지수 (숫자) |
| cons.conf.idx | 소비자 신뢰지수 (숫자) |
| euribor3m | 3개월 유리보 금리 (숫자) |
| nr.employed | 고용자 수 (숫자) |
| y | 정기 예금 가입 여부 (이진: yes=1, no=0) |

**< 코드잇 Comment >**

<aside>
💡

해당 데이터는 UC Irvine Machine Learning Repository에서 제공하는 **Bank Marketing([링크](https://archive.ics.uci.edu/dataset/222/bank+marketing))** 데이터 입니다. 여러 데이터 중 2014년 아래 논문에서 사용된 데이터를 선택하였습니다. 데이터에 대해서 더 자세히 알고 싶은 분들은 아래 데이터 설명 txt파일과 논문을 확인해보시는 걸 추천드립니다.

</aside>

- [데이터 설명서 다운로드](https://bakey-api.codeit.kr/api/files/resource?root=static&seqId=10725&version=2&directory=/bank-additional-names.txt&name=bank-additional-names.txt)
- 데이터 출처
    
    [Moro, S., Cortez, P., & Rita, P. (2014). A Data-Driven Approach to Predict the Success of Bank Telemarketing. Decision Support Systems, In press,](http://dx.doi.org/10.1016/j.dss.2014.03.001)
    

**< 데이터 설명서의 번역본 >**

[데이터 설명서 번역본](https://www.notion.so/1c9e008ecc3a80cba2aac17af441c056?pvs=21)

# 데이터 EDA

---

데이터를 불러오기 전, 데이터 설명에 다음과 같은 내용이 적혀 있다.

<aside>
💡

11 - duration: last contact duration, in seconds (numeric). Important note:  this attribute highly affects the output target (e.g., if duration=0 then y="no"). Yet, the duration is not known before a call is performed. Also, after the end of the call y is obviously known. Thus, this input should only be included for benchmark purposes and should be discarded if the intention is to have a realistic predictive model.

11 - duration: 마지막 연락 지속 시간(초) (숫자). 중요 참고 사항: 이 속성은 출력 목표에 큰 영향을 미칩니다(예: duration=0이면 y="no"). 그러나 통화가 수행되기 전에 지속 시간을 알 수 없습니다. 또한 통화가 종료된 후 y를 명확하게 알 수 있습니다. 따라서 이 입력은 벤치마크 목적으로만 포함해야 하며 현실적인 예측 모델을 만들려는 경우 삭제해야 합니다.

</aside>

따라서 `duration` 컬럼은 불러올 때 바로 drop처리 하였다.

## 데이터의 info확인

---

- 데이터의 info 확인 토글
    
    ```
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 41188 entries, 0 to 41187
    Data columns (total 20 columns):
     #   Column          Non-Null Count  Dtype
    ---  ------          --------------  -----
     0   age             41188 non-null  int64
     1   job             41188 non-null  object
     2   marital         41188 non-null  object
     3   education       41188 non-null  object
     4   default         41188 non-null  object
     5   housing         41188 non-null  object
     6   loan            41188 non-null  object
     7   contact         41188 non-null  object
     8   month           41188 non-null  object
     9   day_of_week     41188 non-null  object
     10  campaign        41188 non-null  int64
     11  pdays           41188 non-null  int64
     12  previous        41188 non-null  int64
     13  poutcome        41188 non-null  object
     14  emp.var.rate    41188 non-null  float64
     15  cons.price.idx  41188 non-null  float64
     16  cons.conf.idx   41188 non-null  float64
     17  euribor3m       41188 non-null  float64
     18  nr.employed     41188 non-null  float64
     19  y               41188 non-null  object
    dtypes: float64(5), int64(4), object(11)
    memory usage: 6.3+ MB
    ```
    

## 데이터의 크기

---

- `(41188, 20)`

train과 test로 구분되지 않는 Raw 데이터로 확인 되었다.

## 데이터의 전체 기간

## 데이터 결측치 확인

---

모델링에 있어서 데이터의 결측치는 삭제하는 것이 가장 안전한 전처리 방법이다. 따라서 결측치가 존재하는지 확인해보았다.

```
age               0
job               0
marital           0
education         0
default           0
housing           0
loan              0
contact           0
month             0
day_of_week       0
campaign          0
pdays             0
previous          0
poutcome          0
emp.var.rate      0
cons.price.idx    0
cons.conf.idx     0
euribor3m         0
nr.employed       0
y                 0
dtype: int64
```

현재 데이터 내의 결측치가 존재하지 않는다고 나온다. 하지만 데이터 설명서에 다음과 같은 내용이 적혀 있다.

<aside>
💡

There are several missing values in some categorical attributes, all coded with the "unknown" label. These missing values can be treated as a possible class label or using deletion or imputation techniques. 

일부 범주형 속성에 여러 누락된 값이 있으며 모두 "unknown" 레이블로 코딩되었습니다. 이러한 누락된 값은 가능한 클래스 레이블로 처리하거나 삭제 또는 대체 기술을 사용하여 처리할 수 있습니다.

</aside>

따라서, 데이터프레임 내 `unknown` 데이터가 존재하지 않는 데이터만 따로 추출하였다.

수정된 데이터의 크기는 다음과 같다.

- `(30488, 20)`

 그 결과, `unknown` 데이터를 가지는 row는 총 11,000건 존재했다.

## 각 컬럼별 unique, nunique 확인

---

각 컬럼들의 내용들을 알고 있지만, unique 데이터가 어떤 값이 들어가 있고, 총 몇개의 unique 데이터가 있는지 확인하여, 판단하기 위해 각 컬럼별 unique와  nunique값을 확인하여 살펴보았다.

- unique, nunique값 확인 토글
    
    ```
    column name : age
    age's unique value : [56 37 40 59 24 25 29 57 35 50 30 55 41 54 34 52 32 38 45 39 60 53 51 48
     44 31 49 33 42 36 43 46 28 58 27 47 26 22 23 20 21 18 70 61 66 76 67 73
     88 95 19 68 75 63 62 65 72 64 71 69 78 85 80 79 77 83 81 74 82 17 87 91
     94 86 84 89]
    age's nunique value : 76
    
    column name : job
    job's unique value : ['housemaid' 'services' 'admin.' 'technician' 'blue-collar' 'unemployed'
     'retired' 'entrepreneur' 'management' 'student' 'self-employed']
    job's nunique value : 11
    
    column name : marital
    marital's unique value : ['married' 'single' 'divorced']
    marital's nunique value : 3
    
    column name : education
    education's unique value : ['basic.4y' 'high.school' 'basic.6y' 'professional.course' 'basic.9y'
     'university.degree' 'illiterate']
    education's nunique value : 7
    
    column name : default
    default's unique value : ['no' 'yes']
    default's nunique value : 2
    
    column name : housing
    housing's unique value : ['no' 'yes']
    housing's nunique value : 2
    
    column name : loan
    loan's unique value : ['no' 'yes']
    loan's nunique value : 2
    
    column name : contact
    contact's unique value : ['telephone' 'cellular']
    contact's nunique value : 2
    
    column name : month
    month's unique value : ['may' 'jun' 'jul' 'aug' 'oct' 'nov' 'dec' 'mar' 'apr' 'sep']
    month's nunique value : 10
    
    column name : day_of_week
    day_of_week's unique value : ['mon' 'tue' 'wed' 'thu' 'fri']
    day_of_week's nunique value : 5
    
    column name : campaign
    campaign's unique value : [ 1  2  3  4  5  6  7  8  9 10 12 11 18 23 14 22 25 17 15 20 19 39 13 42
     16 28 26 32 21 24 29 31 30 35 41 37 27 40 43 34 33]
    campaign's nunique value : 41
    
    column name : pdays
    pdays's unique value : [999   6   4   5   1   0   3  10   7   9  11   8   2  12  13  14  15  16
      21  17  18  22  25  26  19  27]
    pdays's nunique value : 26
    
    column name : previous
    previous's unique value : [0 1 2 3 4 5 6 7]
    previous's nunique value : 8
    
    column name : poutcome
    poutcome's unique value : ['nonexistent' 'failure' 'success']
    poutcome's nunique value : 3
    
    column name : emp.var.rate
    emp.var.rate's unique value : [ 1.1  1.4 -0.1 -0.2 -1.8 -2.9 -3.4 -3.  -1.7 -1.1]
    emp.var.rate's nunique value : 10
    
    column name : cons.price.idx
    cons.price.idx's unique value : [93.994 94.465 93.918 93.444 93.798 93.2   92.756 92.843 93.075 92.893
     92.963 92.469 92.201 92.379 92.431 92.649 92.713 93.369 93.749 93.876
     94.055 94.215 94.027 94.199 94.601 94.767]
    cons.price.idx's nunique value : 26
    
    column name : cons.conf.idx
    cons.conf.idx's unique value : [-36.4 -41.8 -42.7 -36.1 -40.4 -42.  -45.9 -50.  -47.1 -46.2 -40.8 -33.6
     -31.4 -29.8 -26.9 -30.1 -33.  -34.8 -34.6 -40.  -39.8 -40.3 -38.3 -37.5
     -49.5 -50.8]
    cons.conf.idx's nunique value : 26
    
    column name : euribor3m
    euribor3m's unique value : [4.857 4.856 4.855 4.859 4.86  4.858 4.864 4.865 4.866 4.967 4.961 4.959
     4.958 4.96  4.962 4.955 4.947 4.956 4.966 4.963 4.957 4.968 4.97  4.965
     4.964 5.045 5.    4.936 4.921 4.918 4.912 4.827 4.794 4.76  4.733 4.7
     4.663 4.592 4.474 4.406 4.343 4.286 4.245 4.223 4.191 4.153 4.12  4.076
     4.021 3.901 3.879 3.853 3.816 3.743 3.669 3.563 3.488 3.428 3.329 3.282
     3.053 1.811 1.799 1.778 1.757 1.726 1.703 1.687 1.663 1.65  1.64  1.629
     1.614 1.602 1.584 1.56  1.556 1.548 1.538 1.531 1.52  1.51  1.498 1.483
     1.479 1.466 1.453 1.445 1.435 1.423 1.415 1.41  1.405 1.406 1.4   1.392
     1.384 1.372 1.365 1.354 1.344 1.334 1.327 1.313 1.299 1.291 1.281 1.266
     1.25  1.244 1.259 1.264 1.27  1.262 1.26  1.268 1.286 1.252 1.235 1.224
     1.215 1.206 1.099 1.085 1.072 1.059 1.048 1.044 1.029 1.018 1.007 0.979
     0.969 0.944 0.937 0.933 0.927 0.921 0.914 0.908 0.903 0.899 0.884 0.883
     0.881 0.879 0.873 0.869 0.861 0.859 0.854 0.851 0.849 0.843 0.838 0.834
     0.829 0.825 0.821 0.819 0.813 0.809 0.803 0.797 0.788 0.781 0.778 0.773
     0.771 0.77  0.768 0.766 0.762 0.755 0.749 0.743 0.741 0.739 0.75  0.753
     0.754 0.752 0.744 0.74  0.742 0.737 0.735 0.733 0.73  0.731 0.728 0.724
     0.722 0.72  0.719 0.716 0.715 0.714 0.718 0.721 0.717 0.712 0.71  0.709
     0.708 0.706 0.707 0.7   0.655 0.654 0.653 0.652 0.651 0.65  0.649 0.646
     0.644 0.643 0.639 0.637 0.635 0.636 0.634 0.638 0.64  0.642 0.645 0.659
     0.663 0.668 0.672 0.677 0.682 0.683 0.684 0.685 0.688 0.69  0.692 0.695
     0.697 0.699 0.701 0.702 0.704 0.711 0.713 0.723 0.727 0.729 0.732 0.748
     0.761 0.767 0.782 0.79  0.793 0.802 0.81  0.822 0.827 0.835 0.84  0.846
     0.87  0.876 0.885 0.889 0.893 0.896 0.898 0.9   0.904 0.905 0.895 0.894
     0.891 0.89  0.888 0.886 0.882 0.88  0.878 0.877 0.942 0.953 0.956 0.959
     0.965 0.972 0.977 0.982 0.985 0.987 0.993 1.    1.008 1.016 1.025 1.032
     1.037 1.043 1.045 1.047 1.05  1.049 1.046 1.041 1.04  1.039 1.035 1.03
     1.031 1.028]
    euribor3m's nunique value : 314
    
    column name : nr.employed
    nr.employed's unique value : [5191.  5228.1 5195.8 5176.3 5099.1 5076.2 5017.5 5023.5 5008.7 4991.6
     4963.6]
    nr.employed's nunique value : 11
    
    column name : y
    y's unique value : ['no' 'yes']
    y's nunique value : 2
    ```
    

## Object 데이터의 분포 확인

---

barplot을 통해 Object 데이터의 분포가 어떻게 나타나고 있는지 확인하였다.

- 각 Object 컬럼별 barplot
    
    
    ### Job
    
    ![download.png](attachment:ae00ece0-00c0-4096-a2de-caf16cd0d428:download.png)
    
    ### Marital
    
    ![download.png](attachment:a82e8897-2ec7-49bb-8b9a-26519e0a9ced:download.png)
    
    ### Education
    
    ![download.png](attachment:508325d2-437a-498e-916b-fc6fb7733e53:download.png)
    
    ### Default
    
    ![download.png](attachment:f8220b3a-ce05-4a4c-adde-6e71c0fc8c2d:download.png)
    
    ### Housing
    
    ![download.png](attachment:33a325f9-6662-4228-b671-34b7a7a2e714:download.png)
    
    ### Loan
    
    ![download.png](attachment:a7a4b4f9-9cb5-4ee1-9392-2902be996138:download.png)
    
    ### Contact
    
    ![download.png](attachment:4201395b-0643-4ef4-897a-0ed145c19b76:download.png)
    
    ### Month
    
    ![download.png](attachment:899a3171-1e91-4eb8-8c74-fcbc7b0e87b7:download.png)
    
    ### Day Of Week
    
    ![download.png](attachment:9b33cd36-13fc-4113-90b7-a4a85af84432:download.png)
    
    ### Poutcome
    
    ![download.png](attachment:e9de7494-97d8-4c04-903b-4507c1ea38f8:download.png)
    

## Numeric 데이터 분포 및 이상치 확인

---

- 각 Numeric 컬럼별 boxplot, kdeplot
    
    ### Age
    
    ![download.png](attachment:ae98f3f7-f223-4ec9-bee9-274681991465:download.png)
    
    ### Campaign
    
    ![download.png](attachment:54e2a042-a5d6-4951-becd-cd0382f99ccf:download.png)
    
    ### Pdays
    
    ![download.png](attachment:e40653f9-e0ef-4280-9d7a-0d110c6fb05a:download.png)
    
    ### Previous
    
    ![download.png](attachment:f1d454ad-00f1-481a-9af7-0d3ff28256f8:download.png)
    
    ### emp.var.rate
    
    ![download.png](attachment:49ecaf3a-5f04-4ecf-b113-ffa78b5fe904:download.png)
    
    ### cons.price.idx
    
    ![download.png](attachment:98c687fc-8527-4d0c-8cb1-b71675bcf0e0:download.png)
    
    ### cons.conf.idx
    
    ![download.png](attachment:51cd2f0b-970a-4440-86ff-6c62ce5ddb30:download.png)
    
    ### euribor3m
    
    ![download.png](attachment:bffb431e-d1f6-4a02-9592-1688559cc824:download.png)
    
    ### nr.employed
    
    ![download.png](attachment:0e06b9b0-f617-4b39-82cc-e4abf4dc39ce:download.png)
    

## 소비자 신뢰지수 관련 데이터 시각화

---

가장 먼저 소비자 신뢰지수에 대해서 알아보았다.

### **소비자 신뢰지수란?**

- 소비자들이 경제 상황에 대해 얼마나 긍정적이거나 부정적인지를 나타내는 경제 지표
- 소비자들의 현재 및 미래 경제 전망, 개인 재정 상태, 고용 기회 등에 대한 설문조사를 기반으로 산출
- 포르투갈의 소비자 신뢰지수는 0을 기준으로 한다.
    - 0 초과: 소비자들이 경제를 긍정적으로 평가하며, 소비 지출이 증가할 가능성이 큼.
    - 0 미만: 소비자들이경제를 부정적으로 평가하며, 소비를 줄일 가능성이 큼.

### **소비자 신뢰지수를 알면 좋은 점**

1. **경기 전망 예측**
    - 소비자가 경제를 낙관적으로 보면 소비가 늘어나 경제 성장에 긍정적 영향을 미침.
    - 반대로 신뢰지수가 낮으면 소비가 줄어들고 경제 침체 가능성이 높아짐.
2. **기업의 경영 전략 수립**
    - 소비자 신뢰가 높으면 기업은 적극적인 투자와 마케팅을 진행할 수 있음.
    - 신뢰지수가 낮으면 기업은 비용 절감 및 보수적인 경영 전략을 취할 가능성이 큼.
3. **금융 시장 예측**
    - 소비 심리가 강하면 주식 시장에 긍정적인 영향을 주고, 반대의 경우 하락 요인이 될 수 있음.
    - 중앙은행이 금리 정책을 결정할 때 참고할 수 있음.
4. **정책 결정 참고 자료**
    - 정부가 경기 부약책(예: 금리 인하, 소비 촉진 정책)을 결정할 때 활용 가능.
5. **산업별 영향 예측**
    - 소비재, 여행, 자동차 등 소비자의 지출과 직결된 산업은 CCI 변화에 민감함.
    - 신뢰지수가 높으면 사치품 소비가 늘어나고, 낮으면 필수 소비재 중심으로 지출이 이동할 가능성이 큼.

이하 교육 수준별, 직업별, 나이대별 소비자 신뢰지수를 살펴 보았다.

### 교육 수준별 소비자 신뢰지수

![download.png](attachment:f6b59507-94c3-4182-a149-60a6d715c7ce:download.png)

### 직업별 소비자 신뢰지수

![download.png](attachment:24b4cf03-37f8-42c5-86c0-b9ce0978a388:download.png)

### 나이대별 소비자 신뢰지수

![download.png](attachment:399d4e11-3939-44a2-9b4e-549e6108731d:download.png)

### Insight

<aside>
💡

- 교육 수준별 상관없이 전체적으로 소비자 신뢰지수가 -40대에 웃돌고 있음.
- 직업별 소비자 신뢰지수는 직장을 가지지 않은 인원의 소비자 신뢰지수와 직장이 있는 인원의 소비자 신뢰지수는 차이가 있는 걸 확인할 수 있다.
- 나이대별 소비자 신뢰지수는 소득이 있고, 소비 활동이 왕성한 20대와 30대가 최저점에 위치하고 있다. 이는 소비 활동에 대해 비관적인 시각을 가지고 있다고 이야기 할 수 있다.
</aside>

## 3개월 유리보 금리 관련 데이터 시각화

---

여기서 유리보란 무엇이고, 3개월 유리보는 어떤 의미인지 찾아보았다.

### **Euribor(유리보)란?**

- Euribor(Euro Interbank Offered Rate): 유럽의 주요 은행들이 서로 단기 대출을 할 떄 적용하는 금리
- 유로존에서 단기 금리의 기준이 되며, 유럽 중앙은행(ECB)의 정책에 따라 변동
- 다양한 기간(1주, 1개월, 3개월, 6개월, 12개월 등) 단위로 발표됨

### **3개월 유리보 금리의 특징**

- 3개월 만기의 대출에 적용되는 Euribor 금리
- 일반적으로 단기 대출, 대출 이자율, 금융 상품(예: 변동금리 대출, 모기지)의 기준 금리로 사용
- 경제 상황(인플레이션, ECB 기준금리 변화)에 따라 변동

### **어디에 활용되는가?**

- 은행 대출 및 모기지(주택담보대출)
- 기업 대출
- 파생상품 및 금융시장

### **3개월 유리보 금리가 중요한 이유**

- 유럽 경제의 건강 상태를 반영
- 유동성 및 신용위험을 평가하는 지표
- ECB의 통화정책이 어떻게 반영되는지 알 수 있음

앞선 내용과 마찬가지로 교육 수준별, 직업별, 나이대별로 3개월 유리보 금리를 살펴보았다.

### 교육 수준별

![download.png](attachment:06f78502-c213-463f-a0cd-48bcbc921d1d:download.png)

### 직업별

![download.png](attachment:9f64a48d-3767-4389-94aa-e6eec422fbf9:download.png)

### 나이대별

![download.png](attachment:20fe012b-b2f6-4b3b-a54c-716810c652cf:download.png)

### Insight

<aside>
💡

3개월 유리보 금리는 경제 상황과 금리 수준에 따라 다르지만, 일반적으로 다음과 같이 판단한다.

- 2% 이하 → 낮은 금리
- 2% ~ 3% → 중간 수준
- 3% ~ 4% → 높은 금리
- 4% 이상 → 매우 높은 금리

시각화를 통해 알 수 있는 점은 다음과 같다.

- 교육 수준별 금리는 관계없이 대체적으로 3%에 분포하고 있다. 즉, 전체적으로 높은 금리를 가지고 있음.
- 직업별 소비자 신뢰지수는 직장을 가지지 않은 인원과 직장이 있는 인원의 금리는 비교적 큰 차이가 있다.
- 나이대별 금리는 대다수 직장을 가지고 있는 소득이 있고, 소비활동이 많은 나이대인 20대 ~ 50대 사이에서 많은 금리를 보유하고 있다.
</aside>

## 정기 예금 가입 여부 관련  데이터 시각화

---

### 교육 수준별

![download.png](attachment:2cb7b5a6-467c-4b8f-9d9a-3b810e96c9eb:download.png)

### 직업별

![download.png](attachment:58e6cac1-1ca0-4ba7-97ff-6e125f6ac094:download.png)

### 나이대별

![download.png](attachment:6eacdbc6-2dd4-490a-8c3d-dced8931981e:download.png)

<aside>
💡

전체적으로 많은 사람들이 정기 결제에 참여하지 않았다. 이는 데이터의 존재 기간이 2008년부터 2010년 까지의 포르투갈의 은행 데이터인데, 이 때는 **서브프라임-모기지 사태**라는 세계경제에 큰 충격을 주었던 시기였기 때문이라고 추측할 수 있다.

또한, 종속변수의 클래스 불균형이 많이 심하다는 것을 알 수 있다.

</aside>

## 범주형 변수 간 카이제곱을 통한 상관관계 시각화

---

| Variable 1 | Variable 2 | Chi2 Statistic | p-value |
| --- | --- | --- | --- |
| job | marital | 2632.731334 | 0.000000e+00 |
| job | education | 27170.190881 | 0.000000e+00 |
| job | loan | 23.832837 | 8.056670e-03 |
| job | contact | 355.931551 | 2.194807e-70 |
| job | month | 3439.844830 | 0.000000e+00 |
| job | day_of_week | 63.235141 | 1.104738e-02 |
| job | poutcome | 576.098426 | 3.097345e-109 |
| job | y | 730.223863 | 2.033137e-150 |
| marital | education | 1130.228326 | 1.817146e-234 |
| marital | contact | 145.864105 | 2.118420e-32 |
| marital | month | 254.316376 | 1.079232e-43 |
| marital | day_of_week | 31.768993 | 1.024445e-04 |
| marital | poutcome | 40.262516 | 3.819717e-08 |
| marital | y | 54.393629 | 1.543738e-12 |
| education | housing | 19.558380 | 3.317537e-03 |
| education | contact | 316.117576 | 2.871065e-65 |
| education | month | 1828.552741 | 0.000000e+00 |
| education | day_of_week | 63.858089 | 1.787281e-05 |
| education | poutcome | 86.215927 | 2.655741e-13 |
| education | y | 115.492701 | 1.439250e-22 |
| default | day_of_week | 12.360410 | 1.486305e-02 |
| housing | loan | 67.071433 | 2.618453e-16 |
| housing | contact | 197.557189 | 7.127281e-45 |
| housing | month | 155.083694 | 7.789781e-29 |
| housing | day_of_week | 12.662798 | 1.304654e-02 |
| housing | poutcome | 19.312743 | 6.401638e-05 |
| loan | month | 16.991328 | 4.885195e-02 |
| contact | month | 9774.542912 | 0.000000e+00 |
| contact | day_of_week | 97.628039 | 3.145518e-20 |
| contact | poutcome | 1627.980641 | 0.000000e+00 |
| contact | y | 630.038863 | 4.901951e-139 |
| month | day_of_week | 573.899927 | 4.337835e-98 |
| month | poutcome | 3247.237974 | 0.000000e+00 |
| month | y | 2358.789362 | 0.000000e+00 |
| day_of_week | poutcome | 16.762229 | 3.268243e-02 |
| day_of_week | y | 25.669313 | 3.689334e-05 |
| poutcome | y | 3181.092423 | 0.000000e+00 |

<aside>
💡

대부분의 범주형 변수가 담긴 컬럼들이 연관성을 크게 가지고 있다.

</aside>

## 연속형 변수 간의 피어슨 상관계수를 통한 상관관계 시각화

---

![download.png](attachment:29199666-0549-4f39-b831-8223f21a29fb:download.png)

피어슨 상관계수가 높다고 평가하는 기준은 그 절댓값이 0.3 이상인 값을 일반적으로 많이 지정한다.

다음은 상관계수의 절댓값이 0.3 이상인 값들만 따로 출력한 결과이다.

![download.png](attachment:a492afca-c111-4268-a24d-b4612601275f:download.png)

<aside>
💡

캠페인, 고용, 경졔관련 지수 내용들이 상관성을 크게 가지는 것으로 나타났다.

</aside>

# 모델링

---

## 모델링하기 전 유의 사항

- 데이터의 이상치가 많이 분포하지만, 각 컬럼들의 특성상 삭제할 수 없는 데이터들이 다수 존재하여 이상치 처리는 하지 않았다.
- 선형 분류 분석시, 이상치에 민감하게 반응하여 분석 모델링에 큰 차질을 줄 것으로 예상하여, 결정 트리 모델로 진행하는 것으로 결정하였다.

## 모델링 전 데이터 전처리

- 각 범주형 데이터들을 라벨링 처리하여 수치화 하였다.
- 순차적인 데이터를 표현하기 위해 `pdays` 컬럼을 수정하였다.

## 각 모델링 별 F1-Score와 Feature Importance

### RandomForest의 F1-Score와 Feature Importance

```
Best Parameters: {'class_weight': 'balanced', 'max_depth': 10, 'min_samples_leaf': 4, 'min_samples_split': 2, 'n_estimators': 300}
Best Score: 0.516245171205836
              precision    recall  f1-score   support

           0       0.94      0.88      0.91      7974
           1       0.44      0.62      0.52      1173

    accuracy                           0.85      9147
   macro avg       0.69      0.75      0.71      9147
weighted avg       0.88      0.85      0.86      9147
```

![download.png](attachment:b36829c7-9cb0-4e58-91fa-aef5e7456a2f:download.png)

### AdaBoost의 F1-Score와 Feature Importance

```
Best Parameters: {'learning_rate': 0.1, 'n_estimators': 300}
Best Score: 0.29778356255103994
              precision    recall  f1-score   support

           0       0.89      0.99      0.94      7974
           1       0.70      0.17      0.28      1173

    accuracy                           0.88      9147
   macro avg       0.80      0.58      0.61      9147
weighted avg       0.87      0.88      0.85      9147
```

![download.png](attachment:97ac15af-291b-44ef-8e53-e5a26278b638:download.png)

### GradientBoosting의 F1-Score와 Feature Importance

```
Best Parameters: {'learning_rate': 0.1, 'max_depth': 5, 'n_estimators': 300, 'subsample': 0.8}
Best Score: 0.41257530177396856
              precision    recall  f1-score   support

           0       0.90      0.97      0.94      7974
           1       0.59      0.31      0.40      1173

    accuracy                           0.88      9147
   macro avg       0.75      0.64      0.67      9147
weighted avg       0.86      0.88      0.87      9147
```

![download.png](attachment:b2b6b073-d2cc-448e-8fbc-c7aeac6768f3:download.png)

### XGBoost의 F1-Score와 Feature Importance

```
Best Parameters: {'colsample_bytree': 0.9, 'learning_rate': 0.01, 'max_depth': 7, 'n_estimators': 100, 'scale_pos_weight': np.float64(6.945271779597915)}
Best Score: 0.5069448117593149
              precision    recall  f1-score   support

           0       0.94      0.88      0.91      7974
           1       0.43      0.61      0.51      1173

    accuracy                           0.85      9147
   macro avg       0.69      0.75      0.71      9147
weighted avg       0.87      0.85      0.86      9147
```

![download.png](attachment:b499e3af-e9c1-4ad0-8bf6-fda0d460aff3:download.png)

### LightGBM의 F1-Score와 Feature Importance

```
Best Parameters: {'class_weight': 'balanced', 'learning_rate': 0.01, 'n_estimators': 100, 'num_leaves': 40, 'subsample': 0.8}
Best Score: 0.5054146447844372
              precision    recall  f1-score   support

           0       0.94      0.87      0.91      7974
           1       0.42      0.63      0.51      1173

    accuracy                           0.84      9147
   macro avg       0.68      0.75      0.71      9147
weighted avg       0.87      0.84      0.85      9147
```

![download.png](attachment:1dc48da8-ece7-462c-b183-ad7d626241d9:download.png)

## 모델링 인사이트

<aside>
💡

- RandomForest의 F1 스코어가 잘 나왔다는 것을 알 수 있다.
- 연속형 변수 중, 상관계수가 많이 높게 나왔던 컬럼인 `euribor3m` , `nr.employed`, `emp.var.rate`, `cons.corf.idx`, `cons.price.idx`이 변수 중요도에서 가장 높게 나타나있다는 것을 알 수 있다.
- 범주형 변수는 `age` 컬럼이 가장 높게 나타난 것을 확인할 수 있었다.
</aside>

# 분석 결론

---

EDA를 통해 나온 분석 인사이트와 모델링 인사이트를 통해 나온 분석 인사이트를 통해 다음과 같은 결론을 낼 수 있었다.

<aside>
💡

## 전략의 핵심 방향

- 현재 소비자 신뢰지수가 낮고, 고금리 상황임.
- 따라서, 고객이 예금 상품을 선택할 수 있도록 신뢰를 높이고 맞춤형 혜택을 제공해야 함.
- 특히, 20~30대 직장인을 타겟팅하는 전략이 필요.
</aside>

## 소비자 신뢰 회복을 위한 마케팅 전략

- “안정적인 금융 환경 속에서 당신의 돈을 안전하게 불릴 수 있습니다.” 라는 메시지 전달
- 예금 이자를 실질적인 가치로 환산하여 보여주기
- 고객 성공 사례 홍보
- 금융 전문가 인터뷰 및 경제 전망 자료를 활용한 콘텐츠 제공

## 20~30대 맞춤형 예금 상품 개발

- 소액 예금 가능 상품 출시
    - 자유 입출금 예금과 정기 예금을 결합한 하이브리드 상품 제공
- 보너스 혜택 제공
- 모바일 최적화
    - 예금 가입을 모바일에서 1분 내 가능하도록 UX/UI 최적화

## 고용 상태에 따른 맞춤 금융 솔루션

- 직장인 대상 고금리 예금 상품 홍보
- 프리랜서 및 자영업자 대상 유동성 예금 상품 개발
    - 일정 기간 후 부분 인출 가능한 예금 상품 출시
- 고객 대상 금융 컨설팅 제공
