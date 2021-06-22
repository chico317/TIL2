# TIL2 #파이썬 5회차 수업

# 파이썬 seaborn 스타일 바꾸기
sns.set_style()

# melt: 열에 있던 데이터를 행으로 녹인다는 것. 
깔끔한 데이터를 만들기 위해서 Tidy Data(논문; http://vita.had.co.nz/papers/tidy-data.pdf)
예시. 
df.melt(id_vars='지역' , var_name= '기간', value_name = '평당분양가격')

# 기본 폰트 설정
plt.rc("font", family="Malgun Gothic")
plt.rc("axes", unicode_minus=False)

# 데이터 로드
예시.
df_last = pd.read_csv("data/주택도시보증공사_전국 평균 분양가격(2019년 12월).csv", encoding ='cp949') 
encoding 오류 발생 막기위해서 'utf-8'이 기본  'cp949'을 추천 euc-kr 가능하나 제약이 있음

# 해당되는 폴더 혹은 경로의 파일 목록을 출력해 줍니다.
import os

for root, dirs, files in os.walk('data'):
        print(files)
        
# isnull 을 통해 결측치를 봅니다.
df_last.isnull()
# isnull 을 통해 결측치를 구합니다.
df_last.isnull().sum() ==  df_last.isna().sum()

# 데이터타입 변경(object에서 float64로..pd.to_numeric(errors ='coerce')
df_last['분양가격'] = pd.to_numeric(df_last['분양가격(㎡)'], errors = 'coerce')

# plot에서 특정값으로 선을 나타낼때
예시....plt.axhline(10000,linestyle=':')

# 특정값들과 비교할때
예시..._ = y[['서울', '경기','인천','제주']].plot(subplots= True, figsize =(10,6))
       y[['서울', '경기','인천','제주']].plot(subplots= True, figsize =(10,6))
   
# 모든값을 볼때  
pd.options.display.max_columns = None


# 리스트의 인덱싱과 replace를 사용해서 월을 제거합니다.
date.split("년")[1].replace("월","") 파이썬의 문자열파일이므로 일부만으로도 제거가능


# parse_month 라는 함수를 만듭니다.
# 월만 반환하도록 하며, 반환하는 데이터는 int 타입이 되도록 합니다.
def parse_month(date):
        month = date.split("년")[1].replace("월","")
        month = int(month)
        return month
        

# 위에서 그린 피봇테이블을 히트맵으로 표현해 봅니다.
pv=pd.pivot_table(data=df, index='연도', columns= '지역명', values= '평당분양가격').astype(int)
pv.astype(int).style.background_gradient()
==(동일값) pv= df.pivot_table(index= "연도", columns="지역명", values= "평당분양가격")

plt.figure(figsize=(15,4))
sns.heatmap(pv,annot=True, cmap="Blues",fmt ='.0f')

# barplot 으로 연도별 평당분양가격 그리기
# sd : 표준편차
sns.barplot(data= df, x='연도', y='평당분양가격', ci= 'sd')

# 서울만 barplot 으로 그리기
df_seoul = df[df["지역명"]=='서울']
sns.barplot(data= df_seoul, x='연도', y='평당분양가격', ci =None)




