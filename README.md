# ssd-failure
   

### 2023-07-31
  - MB1_LSTM.ipynb
    - LSTM
    - Conv 1d + LSTM
    - LSTM + Attention
    - Conv 1d + LSTM + Attention
      - window_size = 30 ,90 에 대해서 각각 실험 진행
  - MB1_clustering.ipynb
    - failure 발생 날짜 포함 이전 10일에 대해서, time series clsutreing (Kmeans based on DTW distance)
    - 각 클러스터별 특징 분석 진행 중
    -      

### 2023-06-13 회의 이후 기록
- 저번에 회의를 할때 window방법, 4일을 input으로 넣으면 5일을 label값으로 한다는 것을 토론하였는데
- 그 이전에 좀 더 원론적인 토론을 하였다. 

- 우리가 읽었던 그 어떠한 논문에도 4일을 input으로 넣으면 label을 5일로 한다는 말이 없었고 fail이 일어난 시점부터 30일전을 다 label을 1로 잡았다. 
- 이말은 실제로 fail이 일어난 시점과 fail이 일어날거같은 시점을 둘다 label을 1로 잡은 것이다. 
- 그래서 그냥 classification을 해서 만약 label이 1이 된다면 그냥 곧있으면 fail을 할 disk라고 판단을 내릴수 있는 것이다. 

- 하나의 디스크 모델(MA1)에 대해서 fail이 일어난 시점의 행과 fail이 일어나기전 30일의 행을 label이 1인 값이라고 두고 그 외는 그냥 다 healthy하다고 하고 그냥 classification을 하는 것이다. 
- 그래서 MA1을 기준으로 우리가 토의한대로 fail이 일어난 시점과 일어나기전 30일을 다 라벨링을 한 뒤에 그냥 분류모델을 autoML 방법을 통해서 돌려보기로 하고 그것에 따른 실험 결과를 보기로 하였다. 

- 다음은 파이썬 파일에 대한 설명이다
  - 00-making_combined_df.ipynb
    - 이 파일은 우리가 가진 일별 데이터를 월별로 묶어서 병합된 엑셀을 만드는 작업을 하는 것이다.
    - 6개월치에 대한 엑셀을 생성하여서 6개의 엑셀이 생성되었다.
    - 하나의 엑셀에 약 천이백만개의 행이 있다.
  - 01-making_preporcessed_data.ipynb
    - 이 파일은 월별 묶어서 만든 엑셀에서 디스크 모델별로 구분을 시켜놓은것이다.
    - 우리가 6개월치의 엑셀에 각가 디스크 모델 6개가 있으니 36개의 구분된 엑셀파일을 만들었다.
    - 이 작업이 꽤나 귀찮아도 나중에 모델별로 불러올때 시간이 너무 걸려서 엑셀을 다시 만드는 이 작업을 하는것이다.
  - 02-making_combined_by_diskmodel_data.ipynb
    - 그리고 디스크 모델별로 다시 묶었다. 6개의 엑셀 파일이 생성되었다.
    - 예를들어서 MA1에 대한 6개월데이터를 생성하였다.
  - 03-making_final_data_MA1_after_deleted.ipynb
    - 이상하게 fail이 된 이후의 디스크아이디가 남아있었다. 그래서 fail된 뒤의 disk행들을 다 제거했다.
    - fail이 일어난 날짜를 기준으로 한달 전을 다 라벨링을 1로 변환하였다.
      - 이때 연속적이지 않은 데이터들이 많았는데 한달을 기준으로 라벨링해서 만약 한달 전의 행이 25개라면 25개만 1로 labeling되는것이다.
    - fail된 디스크개수(275개)만큼 healthy한 디스크(275개)를 가져와서 병합하는 작업을 거쳤다.
    - 그리고 그렇게 병합한 dataframe을 final_data_MA1_after_deleted.csv에 저장한다. 
  - MA1_by_automl.ipynb
    - 만들어진 final_data_MA1_after_deleted.csv로 automl을 돌렸다.


### 2023-06-07
- 0 인거 지우고
- 하나의 값으로 이루어진거 지우고
=> 58개 feature로 줄였다!

- labeling 코드 완성
  
### 2023-06-06
일로 되어있는 데이터를 월로 묶었다.
1월 2월에 대해서 변수를 확보했다.

앞으로 해야할일
- y labeling
  - failure 1 , healthy 0 
- classification
- eda?




