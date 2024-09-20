---
layout: single
title:  "Project_No.2_서울시 아파트 실거래가 예측"
categories: Project
tag: [Project]
toc: true
author_profile: false
---

<head>
  <style>
    table.dataframe {
      white-space: normal;
      width: 100%;
      height: 240px;
      display: block;
      overflow: auto;
      font-family: Arial, sans-serif;
      font-size: 0.9rem;
      line-height: 20px;
      text-align: center;
      border: 0px !important;
    }

    table.dataframe th {
      text-align: center;
      font-weight: bold;
      padding: 8px;
    }

    table.dataframe td {
      text-align: center;
      padding: 8px;
    }

    table.dataframe tr:hover {
      background: #b8d1f3; 
    }

    .output_prompt {
      overflow: auto;
      font-size: 0.9rem;
      line-height: 1.45;
      border-radius: 0.3rem;
      -webkit-overflow-scrolling: touch;
      padding: 0.8rem;
      margin-top: 0;
      margin-bottom: 15px;
      font: 1rem Consolas, "Liberation Mono", Menlo, Courier, monospace;
      color: $code-text-color;
      border: solid 1px $border-color;
      border-radius: 0.3rem;
      word-break: normal;
      white-space: pre;
    }

  .dataframe tbody tr th:only-of-type {
      vertical-align: middle;
  }

  .dataframe tbody tr th {
      vertical-align: top;
  }

  .dataframe thead th {
      text-align: center !important;
      padding: 8px;
  }

  .page__content p {
      margin: 0 0 0px !important;
  }

  .page__content p > strong {
    font-size: 0.8rem !important;
  }

  </style>
</head>
# House Price Prediction | 아파트 실거래가 예측 Report
### Fastcampus Upstage AI Lab 경진대회(Upstage Competition)

> 팀명 : 6조

![image](https://github.com/user-attachments/assets/6b149119-c4a4-4651-90b2-0cbe393cddfe) **프로젝트 요약**
+ **기간** : 2024.09.03 ~ 2024.09.13
+ **목표** : 서울시 아파트 실거래가 예측
+ **결과** : 서울시 아파트 정보에 관련된 자료를 전처리한 후 XGBoost 모델에 학습한 결과 RMSE가 13648.0988로 Public 1등 그리고 최종 Private 1등 (RMSE : 10624.0663)을 하였음
+ **역할 및 기여** 
    + 데이터 전처리
    + 파생변수 생성
    + 선행 연구 조사
    + 모델 학습 및 선정
    + 발표자료 구성 및 제작

# 1. 프로젝트 개요
## A. 개요
House Price Prediction 경진대회는 주어진 데이터를 활용하여 서울의 아파트 실거래가를 효과적으로 예측하는 모델을 개발하는 대회입니다. 

부동산은 의식주에서의 주로 중요한 요소 중 하나입니다. 이러한 부동산은 아파트 자체의 가치도 중요하고, 주변 요소 (강, 공원, 백화점 등)에 의해서도 영향을 받아 시간에 따라 가격이 많이 변동합니다. 개인에 입장에서는 더 싼 가격에 좋은 집을 찾고 싶고, 판매자의 입장에서는 적절한 가격에 집을 판매하기를 원합니다. 부동산 실거래가의 예측은 이러한 시세를 예측하여 적정한 가격에 구매와 판매를 도와주게 합니다. 그리고, 정부의 입장에서는 비정상적으로 시세가 이상한 부분을 체크하여 이상 신호를 파악하거나, 업거래 다운거래 등 부정한 거래를 하는 사람들을 잡아낼 수도 있습니다. 

저희는 이러한 목적 하에서 다양한 부동산 관련 의사결정을 돕고자 하는 부동산 실거래가를 예측하는 모델을 개발하는 것입니다. 특히, 가장 중요한 서울시로 한정해서 서울시의 아파트 가격을 예측하려고합니다.

**제공된 데이터셋**
> 1. 아파트 관련 정보 (아파트 주소, 번지, 건축면적, x좌표, y좌표 등)
> 2. 지하철역 데이터
> 3. 버스정류장 데이터

주어진 데이터는 다음과 같았으나 외부 데이터에 대한 제한 사항이 규칙에 없어 다양한 외부 데이터를 사용하여 대회에 응용시킬 수 있었다.

## B. 환경
+ 컴퓨팅 환경
> 인당 3090 GPU 서버를 VSCode와 SSH로 연결하여 사용
+ 요구사항
> VPN, SSH key, VSCode
+ 협업
> Notion, Github

# 2. 프로젝트 팀 구성 및 역할
+ 팀 구성
프로젝트 팀원 인원은 4명으로 프로젝트를 진행하였다.
+ 역할
> 데이터 확인 및 수집(Data Verification and Collection)
> 데이터 전처리 및 EDA(Data Preprocessing and Exploratory Data Analysis)
> 모델링 및 파라미터 튜닝(Modeling and Parameter Tuning)
> 결과 정리 및 보고서 작성(Summarization of Results and Report Writing)

# 3. 프로젝트 수행 절차 및 방법
## A. 목표 설정
> 1. 프로젝트 basic 온라인 강의 듣고 코드 및 데이터 패턴 파악하기
> 2. 파생변수 생성, 외부 데이터 수집, 데이터 탐색 및 파악 (전처리 및 EDA)
> 3. 아파트 실거래가 예측 관련 선행연구 조사
> 4. 선행연구에서 사용된 모델 활용

# 4. 프로젝트 결과
Public 1등 (RMSE : 13648.0988), Private 1등 (RMSE : 10624.0663)

![image](https://github.com/user-attachments/assets/0742dab5-37b0-4561-9ad8-462fbbfa5d1e)


# 5. 자체 평가 의견
## A. 긍정적 
+ 팀원 중 프로젝트와 관련된 전공자분이 계셔서 깊은 도메인 지식으로 인한 데이터 처리 및 수집 시간이 매우 단축되었다.
+ 각자 시도한 방향을 실시간으로 공유하여 함께 프로젝트를 진행하여 너무 만족스러웠다.

## B. 부정적
+ 커널 충돌로 인한 하이퍼파라미터 튜닝 이슈
+ 서버 속도 이슈로 모델링에 많은 시간이 소요

