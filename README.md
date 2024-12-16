# 복지분야 재현데이터 생성기 개발

<br>

## 진행기간
> **2023/09~12**
<br>


## 사용 데이터
> **한국보건사회연구원에서 진행되는 한국복지패널조사 설문 데이터**
>
> www.koweps.re.kr
<br>

## 개발 환경
> **R, Python**
<br>

### 역할
> **자료 조사, EDA, 프로젝트 코드 리뷰, Modeling**
<br>

## 분석 주제
> **복지 분야 재현 데이터 생성기 개발**
<br>

## 분석 목적
- 데이터 3법 이후 데이터의 적극 활용이 매우 중요해짐
- 데이터 활용을 위해 개인정보보호 과정이 필수
- 현재 마스킹 기법과 차등적 보호기법이 널리 사용되나 데이터 효용의 손실이 발생하는 어려움
<br>

- **재현 데이터 생성 기법을 활용해보자**
    - 실제로 관측된 데이터를 생성하는 모형 혹은 모집단이 존재한다고 가정하고, 통계적 방법이나 기계학습 방법 등을 이용해 추정된 모형에서 새롭게 생성한 모의 데이터
    - 예를 들어 특정 회사 고객의 거래 내역과 정보를 재현 데이터로 만들고자 할 때 실제 인구 통계적 특성과 거래 내역의 특성을 유지하며 각 개인의 신원 정보는 보호되는 방식으로 데이터를 생성
    - 익명데이터로 분류되어 개인정보 관련 규제로부터 자유로움
    - 원 데이터와 비슷한 수준의 효용
<br>

## 분석 요약

1. 데이터 수집
2. Data Preprocessing
3. Modeling
    1. Synthpop, CTGAN, CTABGAN, CTABGAN+
4. Verification
    1. 노출위험도와 유용성 평가(TCAP, pMSE)
    2. 시각적 비교분석
    3. 통계적 유사성(ks-test, Columns Shapes, Columns Pair Trends)
    4. 머신러닝 유효성 평가
<br>

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/75d82d16-f489-4a37-82a2-ff41b5068dfd" width="70%">
    </td>
  </tr>
</table>
<br>

## 분석 과정

### *데이터 수집&Data Preprocessing*
<br>

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/93a2523e-463a-4f87-ba8a-fa80e0f32514" width="90%">
    </td>
  </tr>
</table>
<br>

- 국민의 복지 실태 및 복지욕구를 정태적, 동태적으로 보여주는 조사(2006년부터 매년 시행)
- 우리나라의 복지 발전을 위해 필요한 지적 인프라 제공
- **특히 저소득층의 문제 및 빈곤의 역동성 분석에 초점**
- 인구 사회학적 배경 및 개인사와 관련된 문항과 복지 서비스를 중심으로 문항 구성
<br>

- 저소득층여부, 나이, 교육 수준 등 개인적인 정보라고 판단되는 변수 선정 후 전처리 진행
- 결측치나 논리적  이상치는 존재하지 않아 따로 크게 전처리는 범주형 재코딩만 진행
<br>

### *Modeling*

<br>

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/ae97270d-962e-4435-abb8-0f0e3eb39e7a" width="80%">
    </td>
  </tr>
</table>
<br>

- 재현데이터 생성 기법에는 대표적으로 통계적 기반 모델과 딥러닝 기반 모델로 나뉘어 연구되고 있음
- 이에 Synthpop 패키지를 활용하여 `비모수적 다중대체 모형을 이용한 생성 방법론`과 GAN 기반 모델인 `CTGAN, CTABGAN, CTABGAN+`를 이용하여 재현데이터를 생성 후 복지데이터에 적합한 모델을 선정
<br>

<table width="100%">
  <tr>
    <td align="left" width="50%">
      <img src="https://github.com/user-attachments/assets/9451a2e8-7d93-42e3-af84-4d4f45783b13" width="95%">
    </td>
    <td align="right" width="50%">
      <img src="https://github.com/user-attachments/assets/807cc5d6-bf74-4be6-814b-9970cc0fa279" width="95%">
    </td>
  </tr>
</table>

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/f002df03-a8fe-4aac-94cb-8d9476e2fbd4" width="80%">
    </td>
  </tr>
</table>
<br>

- **통계적 기반 재현 데이터 생성 기법으로는 Synthpop 패키지를 활용**
    - 순차적 조건부 확률분포 학습 기반의 여러 모형들을 구현하여 제공하며, 저희는 비모수적 다중대체 모형을 이용한 방법으로 CART 알고리즘을 지정하여 진행
    - 보통 CART 알고리즘은 예측을 위해 개별 말단 노드에서 관측된 반응 변수의 평균을 사용하나 데이터 재현을 위해 Synthpop에서는 베이지안 붓스트랩을 사용
- **딥러닝 기반 재현 데이터 생성 기법으로는 GAN 기반의 모델을 활용**
    - GAN 모형은 이미지 데이터에 최적화 되어 있는 모형으로 테이블 형식의 데이터에 활용하기 위해 TableGAN이 제안
        - 테이블의 각각의 행을 정방행렬의 형태로 변환하여 2차원 이미지 형태의 자료로 고려한 것처럼 학습. 하지만 기울기 소실 문제나 혼합형 데이터에 대해 불균형이 존재할 경우 소수의 범주가 누락될 수 있다는 단점
        - 이러한 문제점을 해결하기 위해 CTGAN이 고안되었으며, 이후 CTABGAN, CTABGAN+ 등 또한 개발

<br>

### *Verification*

<br>

<table width="100%">
  <tr>
    <td align="left" width="50%">
      <img src="https://github.com/user-attachments/assets/6bdea922-bd20-4a7d-84b0-1396d00145f3" width="95%">
    </td>
    <td align="right" width="50%">
      <img src="https://github.com/user-attachments/assets/9532f8b9-be7e-4424-ac2f-ff2ee9d627dc" width="95%">
    </td>
  </tr>
</table>

<table width="100%">
  <tr>
    <td align="left" width="50%">
      <img src="https://github.com/user-attachments/assets/3d275871-ebba-4a2d-b833-3ac4a48add3c" width="95%">
    </td>
    <td align="right" width="50%">
      <img src="https://github.com/user-attachments/assets/84476f85-e8e8-48ef-8eb2-315ab4ad5a6e" width="95%">
    </td>
  </tr>
</table>

<br>

- 재현데이터는 익명데이터로 분류되어 개인정보 관련 규제로부터 자유롭다고 하나 참고한 논문에서 범주형 변수만 포함된 데이터에 대해서는 동일한 범주로 구성된 원 데이터와 동일한 레코드가 나타나게 되므로 재현 데이터에서도 노출 위험을 측정해야한다는 의견이 제안
- `TCAP`
    - 외부 공격자가 재현 데이터와 원 데이터의 식별자 변수 정보를 통하여 원 데이터의 목표 변수의 정보를 얻는 노출 위험을 측정하는 측도
    - 원 데이터의 변수들을 식별자 변수로 정의하고 재현 데이터와 원 데이터의 변수 정보를 활용하여 알아 내고자 하는 정보를 지닌 변수를 목표변수로 정의
    - 0부터 1의 값을 가지며 `높은 값을 가질 수록 노출 위험도가 낮다는 의미`
- `pMSE`
    - 재현데이터의 효용성을 측정하는 측도로 원 데이터와 재현데이터의 특정 통계량의 유사도를 계산하는 것이 아닌 전체적인 관점에서 유사성을 판단하기 위해 원 데이터와 재현 데이터의 성향 점수를 기준으로 유용성을 측정
    - 원 데이터와 재현 데이터를 하나로 통합한 뒤 원 데이터와 재현데이터를 0과 1로 구분하는 변수를 추가
    - 통합된 데이터의 변수를 설명 변수로 고려하고 원 데이터와 재현데이터를 구분하는 변수를 예측 변수로 두고 적절한 이진분류 모형을 적용
    - 각각의 레코드마다 재현데이터라고 판단할 확률을  $\hat{p_i}$ 로 설정하고 pMSE를 계산
    - 재현데이터가 원데이터와 비슷하다면 확률이 0.5와 가깝게 도출되므로, pMSE가 `낮은 값을 가질 수록 데이터 효용성이 우수`하다고 판단
- 우측 하단 그래프에서 세로축은 TCAP을, 가로축은 pMSE를 나타냄
    - 보라색 선은 원본 데이터를 비율에 맞추어 sampling 하고 원본 데이터 100%와 비교하여 각 지표를 계산한 결과
    - 원본데이터 100%를 사용했을때 노출위험도가 가장 높지만 데이터 효용성 또한 가장 높은 값을 나타내는 것을 알 수 있음
    - 그와 반대로 원본데이터의 2.5%를 sampling 하여 사용했을 때는 두 지표 모두 가장 낮은 값을 가짐
    - 모델 학습의 결과 Synthpop은 노출위험도가 가장 높은 것으로 나타났고 `CTGAN과 CTABGAN+의 경우 노출 위험도가 원본데이터를 거의 사용하지 않은 데이터보다 낮으며 데이터 유용성은 매우 높게 나타남`. 이에 두 모델을 선정하여 보조 지표를 활용해 검증 후 최종 모델을 선정

<br>

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/406f1a8e-28a8-4267-ba86-16217273ad8c" width="90%">
    </td>
  </tr>
</table>
<br>

- 파란색은 원 데이터의 분포이고, 주황색은 재현데이터를 나타냄
- CTGAN에 비해 CTABGAN+이 특히 범주형변수에서 원 데이터와 비슷한 재현데이터를 생성했음을 확인

<br>

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/ad8814e9-2d7e-40cd-9906-1939b5b4fb5c" width="90%">
    </td>
  </tr>
</table>
<br>

- 통계적 유사성으로는 Column Shapes 지표와 Column Pair Trends 지표를 활용
    - 전자는 원본 데이터와 재현데이터의 누적 그래프가 서로 겹치는 비율을 나타내며 후자는 두 개의 데이터 추세선이 유사한 것을 나타내는 지표
    - 두 지표 모두 CTGAN보다 CTABGAN+가 더 높게 나온 것을 확인
- kstest는 누적 분포 함수 간의 차이를 이용하여 분포가 유사함을 확인하는 방법
    - 수치형 변수에서 구할 수 있으며 3가지 변수 모두 CTABGAN+가 더 높게 나온 것을 확인

<br>

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/b307bd43-447c-4f4f-a06c-1548c81f8625" width="90%">
    </td>
  </tr>
</table>
<br>

- 목적에 맞는 변수를 예측 변수로 두고 나머지 변수를 설명 변수로 설정한 후 적절한 모델을 활용하여 원본데이터와 비교해 어느 만큼의 유효성을 갖는지 확인하는 방법
- 저소득층 여부를 예측 변수로 두었고 Logistic Reggression, Random Forest, SVM 모델을 사용
- CTGAN, CTABGAN+ 모두 좋은 수치를 보이나 CTABGAN+ 가 비교적 높은 수치를 보이고 이는 원본데이터와 비교해도 거의 차이가 없음

- 특이한 점으로 실제로 총 생활비가 음수를 가진다는 것은 불가능 하며 원본데이터에서도 발견되지 않음. 하지만 CTABGAN+와 달리 CTGAN 재현 데이터에서 총 생활비가 음수를 가지는 경우를 다수 발견
<br>

***노출위험도와 데이터 효용성을 주요 지표로 살펴보았을 때는 CTGAN과 CTABGAN+ 가 선정. 이를 보조지표로 더 자세히 살펴본 결과 CTABGAN+가 더 우수한 결과를 나타냈고 최종모델로 선정***

<br>

## 시사점 및 보완할 점

- **선행 연구 조사 및 방법론 정의**
    - 재현 데이터 생성 기법 및 검증 방법론 관련 논문 리뷰
        - `생소한 분석 기법`으로 다수의 논문과 참고 자료 찾아보며 스터디 병행
        - 특히 평가 기준이 명확하지 않고 잘 알려지지 않아, `검증 방법을 직접 정의 및 코드로 구현`(TCAP, pMSE, 머신러닝 유효성 평가,…)
- **Modeling**
    - 비모수적 다중대체 모형을 비롯해 GAN 기반의 CTGAN, CTABGAN, CTABGAN+와 같은 `최신 분석 기법`을 활용
    - 재현하는 데이터의 크기가 작았기 때문에, 더 큰 데이터를 활용하여 시간 소요 및 성능 평가를 추가적으로 시행 해볼 필요가 있음
- **종합**
    - CTABGAN+ 를 활용하여 재현데이터를 생성할 경우 원 데이터와 거의 차이가 없는 성능의 학습 모델을 구축 가능할 것
    - 또한 내부 이용자 외의 연구자들도 분석할 수 있게 되어 더욱 효과적이고 적절한 정책 및 활용방안을 모색 가능
    - 기존에 수행하기 어려웠던 다양한 분석, 교육 등에 활용할 수도 있으며 복지 분야 뿐만 아닌 `금융, 의료 등 다양한 분야에서 활용할 수 있을 것으로 기대`
