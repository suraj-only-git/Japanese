

# 목차 

- 서문	

- 랩 자격 증명:	

- 데이터 흐름 템플릿 가져오기	

    데이터 흐름 템플릿을 가져오는 방법	

- 데모 요구 사항	

- 환경

    랩 환경을 구성하는 방법	

    예측용 Notebook 만드는 방법	



# 서문

이 문서에서는 다음 기능에 대한 지침을 제공합니다.

- 랩 자격 증명

- 데이터 흐름 템플릿을 가져오는 방법

- Spark 라이브러리를 로드하고 환경을 구성하는 방법

- Notebook 만드는 방법

- 데이터 과학 모델을 사용하여 예측을 생성하는 방법

- 출력을 의미 체계 모델에 저장하는 방법


**면책고지**: 제품이 매일 바뀌기 때문에 일부 스크린샷은 최신이 아닐 수 있습니다. 다음 업데이트에서 이 문제를 수정하겠습니다.


**랩 자격 증명**:

참석자가 다른 환경에서 랩을 완료하기로 선택한 경우 공유해야 할 자격 증명은 다음과 같습니다.

참석자가 Dataverse 및 SharePoint와 연결하려면 랩 계정과 연결된 사용자 이름과 암호가 필요합니다.

Snowflake 사용자 이름: **TE_SNOWFLAKE**

Snowflake 암호: **8UpfRpExVDXv2AC**

ADLS Gen2 계정 키: **Lpwn8hQASMpe5r4F+VFXAvpnzKF9x9Kjt5GMvMCFWB0xCFuM4fyVwOW6rF200bTop3LpKpsIno/T+AStx6cz6w==**
 
# 데이터 흐름 템플릿 가져오기

강사는 참석자가 데이터 흐름 템플릿을 가져올 수 있는 옵션을 갖도록 선택할 수 있습니다. 템플릿을 가져오는 단계는 다음과 같습니다.

## 데이터 흐름 템플릿을 가져오는 방법

1. 	랩 2, 작업 8에서 생성하고 이름을 FAIAD_<username>(으)로 지정한 Fabric 작업 영역으로 이동합니다. 우리는 FAIAD_demouser라는 이름을 붙였습니다.
2. 	Data Factory 홈 페이지로 이동합니다.
3. 	메뉴에서 새로 만들기 -> Dataflow Gen2를 선택합니다.
 
4. 	Power Query 창이 열립니다. 가운데 창에서  Power Query 템플릿에서 가져오기를 선택합니다.
 
5. 	바탕 화면 -> 솔루션 폴더로 이동하여 가져오려는 데이터 흐름을 선택합니다. 여기서는 df_People_SharePoint.pqt를 가져옵니다.
6. 	열기를 선택합니다.
가져온 후에는 쿼리를 확인하고 쿼리에 대한 모든 단계를 가져옵니다. 그러나 연결을 구성해야 합니다. 또한 데이터 대상을 설정해야 합니다. 이들 단계를 완료하려면 랩 지침을 따르세요.
 

데모 요구 사항
강사는 다음 단계로 진행하기 전에 랩 1~6을 완료하고 모든 데이터를 수집해야 합니다.
환경
랩 환경을 구성하는 방법
참고: 라이브러리 설치에 시간이 걸리므로 데모 전에 랩 환경을 구성하는 것이 가장 좋습니다. 참석자에게 이들 단계를 안내할 수 있습니다.

7. 	랩 2, 작업 8에서 생성하고 이름을 FAIAD_<username>(으)로 지정한 Fabric 작업 영역으로 이동합니다.
8. 	상단 메뉴에서 줄임표를 선택합니다.
9. 	작업 영역 설정을 선택합니다.
 
10. 	작업 영역 설정 대화 상자가 열립니다. 왼쪽 메뉴에서 Data Engineering/과학을 확장합니다.
11. 	Spark 설정을 선택합니다.
12. 	Spark 설정 메뉴에서 환경 탭을 선택합니다.
13. 	기본 환경 설정 슬라이더를 켜기로 변경합니다.
14. 	작업 영역 기본값 드롭다운을 선택합니다.
15. 	새 환경을 선택합니다.
 
16. 	새 환경 대화 상자가 열립니다. 이름을 FAIAD_<username>_env

로 입력합니다. 참고:작업 영역 이름은 고유해야 합니다. 이 문서의 작업 영역 이름으로 FAIAD_demouser_env를 사용하고 있습니다. 그러나 참여자의 작업 영역 이름은 이와 달라야 합니다. 이름 필드 아래에 "이 이름을 사용할 수 있습니다."라는 문구와 함께 녹색 확인 표시가 있는지 확인합니다.

17. 	만들기를 선택합니다.
 
18. 	공개 및 사용자 정의 라이브러리를 추가할 수 있는 화면으로 이동됩니다. 우리는 공개 라이브러리인 Prophet을 추가하고자 합니다. 상단 메뉴에서 공개 라이브러리 -> PyPI에서 추가를 선택합니다.
19. 	중앙 창의 라이브러리 아래 텍스트 상자에 prophet

을 입력합니다. 참고: 버전이 1.1.5인지 확인하세요.

20. 	오른쪽 상단 창에서 게시를 선택합니다. 
 
21. 	보류 중인 변경사항 대화상자가 열립니다. 모두 게시를 선택합니다.
22. 	"모든 변경사항을 게시하시겠습니까?" 대화 상자가 열립니다. 게시를 선택합니다. 업데이트를 게시하려면 몇 분 정도 걸립니다.
 
23. 	진행 상황을 확인하려면 진행 상황 보기를 선택합니다. 업데이트를 게시하려면 몇 분 정도 걸립니다.
 
24. 	설치가 완료되면 상태가 성공으로 변경됩니다.
 
25. 	이제 환경을 구성했으므로 이를 작업 영역의 기본 환경으로 저장해야 합니다. 왼쪽 패널에서 FAIAD_<username>을 선택합니다.
26. 	상단 메뉴에서 작업 영역 설정(또는 줄임표 -> 작업 영역 설정)을 선택합니다.
 
27. 	작업 영역 설정 대화 상자가 열립니다. 왼쪽 메뉴에서 Data Engineering/과학을 확장합니다.
28. 	Spark 설정을 선택합니다.
29. 	Spark 설정 메뉴에서 환경 탭을 선택합니다.
30. 	기본 환경 슬라이더를 켜기로 설정합니다.
31. 	작업 영역 기본값 드롭다운을 선택합니다.
32. 	드롭다운에서 방금 생성한 환경: FAIAD_<username>_env을 선택합니다.
33. 	저장을 선택합니다.
 

예측용 Notebook 만드는 방법
34. 	Synapse Data Engineering 홈 페이지로 이동합니다.
35. 	새로 만들기 -> Notebook을 선택합니다.
 
36. 	Notebook, 언어, 환경의 레이아웃, 새 셀을 만드는 방법 등에 대한 간단한 개요를 제공합니다.
37. 	새 셀을 만듭니다.
38. 	다음 코드를 입력합니다.
from pyspark.sql import SparkSession
from pyspark.sql.functions import month, year, col
from prophet import Prophet
import pandas as pd

# Initialize Spark session
spark = SparkSession.builder.appName("Prophet Forecasting").getOrCreate()

# Load data from your specific Spark table
df = spark.sql("SELECT * FROM lh_FAIAD.Sales")

# Aggregate data to monthly level
monthly_df = df.withColumn("Month", month("InvoiceDate"))\
               .withColumn("Year", year("InvoiceDate"))\
               .groupBy("Year", "Month")\
               .sum("Quantity")\
               .orderBy("Year", "Month")

# Convert to Pandas DataFrame and prepare for Prophet
pandas_df = monthly_df.toPandas()
pandas_df['ds'] = pd.to_datetime(pandas_df[['Year', 'Month']].assign(DAY=1))
pandas_df['y'] = pandas_df['sum(Quantity)']

# Fit the Prophet model
model = Prophet(yearly_seasonality=True, weekly_seasonality=False,daily_seasonality=False)
model.fit(pandas_df[['ds, 'y']])

# Create a DataFrame for future predictions (e.g., next 12 months)
future = model.make_future_dataframe(periods=12, freq='M')

# Forecast
forecast = model.predict(future)

# Plotting the forecast
model.plot(forecast)
model.plot_components(forecast)

39. 	코드의 각 단계를 설명합니다(코멘트 입력 수).
40. 	셀 옆에 있는 재생 버튼을 선택하여 코드를 실행합니다.
 
생성된 세 가지 차트(아래)를 참석자들에게 안내합니다. 2023년 4월까지의 실제 데이터가 있고 12개월을 예측합니다. 
첫 번째 차트에서는 계절성과 2024년 4월까지의 예측이 제거된다는 점에 주의합니다.
두 번째 차트에서는 추세가 제거되고 계절성을 추가하여 2023년 4월까지 예측합니다.
 
세 번째 차트는 추세와 계절성을 모두 사용하여 예측합니다. 이 차트는 상한과 하한도 제공합니다.
 
41. 	새 셀을 만듭니다. 
42. 	다음 코드를 셀에 추가합니다.
display(forecast)
#write forecast data to a table
spark.createDataFrame(forecast).write.saveAsTable("Sales_Forecast", mode="overwrite")

43. 	재생 버튼을 선택하여 셀을 실행합니다.
 
44. 	표시되는 데이터를 참석자들에게 안내합니다.
45. 	사용자에게 새 테이블이 생성되었음을 표시합니다. sales_forecast
 
46. 	테이블을 쿼리 하고 사용자에게 테이블의 내용을 표시합니다.
