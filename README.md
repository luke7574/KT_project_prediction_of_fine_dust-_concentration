# 미세먼지 농도 예측 모델 
---
## ✔️프로젝트 개요
- 주제 : 공공데이터를 활용한 미세먼지 농도 예측
- 데이터 출처

  > 미세먼지 : <https://www.airkorea.or.kr/web/pastSearch?pMENU_NO=123>

  > 날씨데이터 : <https://data.kma.go.kr/data/grnd/selectAsosRltmList.do?pgmNo=36>
- 중점사항 : 시계열 데이터 전처리, 단변량/이변량 분석/ ML 모델링
- 프로젝트 목표 : 데이터를 전처리 후 머신러닝 모델 완성해 미세먼지 농도 데이터에 대해 예측, 평가
![image](https://github.com/user-attachments/assets/ce9d8fa5-81b2-45f2-b796-e9d110b8de85)

---

## ✔️도메인 이해
![image](https://github.com/user-attachments/assets/ff0f13db-95fe-41cc-bb1d-6d3ec42ef98f)
![image](https://github.com/user-attachments/assets/2b5a41d0-e831-4e57-956a-ead7e6c1848c)

## ✔️사례 
![image](https://github.com/user-attachments/assets/21f60bb2-34cd-450e-8c14-ba0a134e1ef4)
![image](https://github.com/user-attachments/assets/e1aa7f95-9533-46c3-919c-37fddf477252)
---
## ✔️데이터 소개 
![image](https://github.com/user-attachments/assets/073a62a5-1fd3-4334-9367-c288671f0856)
![image](https://github.com/user-attachments/assets/1fe49261-280b-42e0-a1bf-dda6a03dcc3e)
---

## ✔️데이터 분석 및 전처리 
![image](https://github.com/user-attachments/assets/ce92252a-3cc1-40aa-97f7-82135c61e57e)
- QC 플래그 : 대부분의 값이 NaN값이고 결측치가 아닌 값들도 결측을 의미하는 9에 해당
-----------------------------------------
![image](https://github.com/user-attachments/assets/580cd2bf-7e02-473a-97a1-3d51a71bb1ab)
- PM10 에 결측치가 존재함
- PM10의 분포는 오른쪽으로 꼬리가 긴 형태의 분포를 가진 수치형 데이터이다.
- boxplot을 통하여 이상치가 존재하는 것을 확인하였다.
---
![image](https://github.com/user-attachments/assets/8aaf460c-0a10-4702-b86d-b911d302dc15)
- PM25 에 결측치가 존재함
- PM25의 분포는 오른쪽으로 꼬리가 긴 형태의 분포를 가진 수치형 데이터이다.
- boxplot을 통하여 이상치가 존재하는 것을 확인하였다.
---
### PM10 & (SO2, CO, NO2, O3, PM2.5) 상관관계
![image](https://github.com/user-attachments/assets/6f891869-33cc-4ba6-a63c-601304042667)

---
![image](https://github.com/user-attachments/assets/862ee852-b0d5-4af9-b642-08b8b834863c)
- PM10 과 O3는 상관계수가 0.05이고, 그래프 또한 비슷한 추세가 없으므로 컬럼 삭제.
- SO2, CO, NO2, PM25, PM10 결측치 처리는 보간법으로 진행
---
![image](https://github.com/user-attachments/assets/6a9cd26e-9ab3-47ba-b68c-63e980edf7ec)
- missingno 라이브러리를 이용한 결측치 시각화
- 시간의 변화에 따른 값으로 생각이 되어, fillna(method = ‘ffill’), fillna(method =’bfill’)을 진행
- 적설, 강수량, 일조, 일사 컬럼의 결측값은 값이 존재하지 않은 것은, 측정되지 않은 것으로 판단하여, 0으로 결측치 처리

---
## ✔️머신러닝 모델링 변수 중요도
### 랜덤포레스트 변수 중요도
![image](https://github.com/user-attachments/assets/16fbeac4-19fb-44bb-ab07-125d606f5db5)
- PM 10이 압도적으로 중요
- PM10 > 습도 > 전일_미세먼지_농도_변수

---
### 그래디언트 부스팅 변수 중요도
![image](https://github.com/user-attachments/assets/0b7689b2-b7b4-44f1-acfa-78eb2c34e1ba)
- PM 10이 압도적으로 중요
- PM10 > CO > 5cm 지중온도

- **해당 자료들을 통해 PM10이 예측시 가장 중요한 feature라는 점 확인**
---

## ✔️종합결과 
- **모델 성능 비교**
![image](https://github.com/user-attachments/assets/de4c7fbc-19e8-4013-bf36-b74df1ba6453)
- 미세먼지 농도 예측 모델의 성능으로 평균제곱오차(MSE)와 결정계수(R2 score)를 고려했을 때 Linear Regression 모델의 성능이 가장 좋았다.
- Linear Regression, Random Forest, Gradient Boosting, Light GBM 간 성능 차이가 크지 않다.
- 변수 ‘PM10’의 변수 중요도가 매우 높은 것이 원인으로 보이며 이를 제외한 추가적인 분석이 필요하며, 독립변수 선택 시 다중공선성 고려 및 결측치 처리 방식을 개선하여 모델링 성능의 개선 여지가 있다고 생각한다.
