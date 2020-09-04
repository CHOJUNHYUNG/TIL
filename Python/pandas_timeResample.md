# 리샘플링
시계열의 빈도를 변환하는 과정
``` python
# resample 인자
freq : 리샘플링 빈도
axis : 리샘플링 축
fill_method : 샘플링시 보간 방법. 'ffill'/'bfill'
closed : 다운샘플링시 각 간격의 포함 방향. 'right'/'left'
label : 다운샘플링 집계시 라벨 결정. 'right'/'left'
loffset : 나뉜 그룹의 라벨에 맞추기 위하 오프셋.
limit : 보간을 적용할 최대 기간
kind : 기간별/타임스탬프별 집계 구분
convention : 하위 빈도에서 상위 빈도로 변환 시의 방식. 'start'/'end'
```
``` python
rng = pd.date_range('2000-01-01', periods=100, freq='D')
ts = pd.Series(np.random.randn(len(rng)), index=rng)
```
``` python
>>> ts.resample('M')
<pandas.core.resample.DatetimeIndexResampler object at 0x7fe631af8b00>

>>> ts.resample('M').mean()
2000-01-31   -0.136343
2000-02-29    0.188737
2000-03-31   -0.067443
2000-04-30    0.373316
```
## 다운샘플링
다운샘플링 : 상위 빈도에서 하위 빈도로 **집계**하는 것
다운샘플링시 고려할 사항

* 각 간격의 끝중 어느 쪽을 닫을 것인가 : closed
* 라벨을 간격의 시작과 끝중 무엇으로 할 것인가 : label
``` python
>>> ts.resample('5min', closed='right').sum()
1999-12-31 23:55:00   0
2000-01-01 00:00:00  15
"""
left : 00:00:00 ~ 00:05:00
right : 23:55:00 ~ 00:00:00
"""
```
``` python
>>> ts.resample('5min', closed='right', label='right').sum()
2000-01-01 00:00:00   0
2000-01-01 00:05:00  15
"""      left       right
left  : 00:00:00 ~ 00:05:00
right : 23:55:00 ~ 00:00:00
"""
```
``` python
>>> ts.resample('5min', closed='right', label='right', loffset='-1s').sum()
1999-12-31 23:59:59   0
2000-01-01 00:04:59  15
```
### OHLC 리샘플링
금융 분양에서 흔히 사용하는 집계방식으로 시가, 고가, 저가, 종가를 갖는 DataFrame을 생성할 수 있다.
``` python
ts.resample('5min').ohlc()
```
## 업샘플링과 보간
업샘플링 : 다운샘플링의 반대. 빈도에 맞게 데이터를 늘임.
업샘플링시에는 집계가 필요하지 않다.

``` python
frame = pd.DataFrame(np.random.randn(2,4),
                    index=pd.date_range('1/1/2000', periods=2,freq='W-WED'),
                    columns=['Colorado','Texas','New York','Ohio'])

>>> df_daily = frame.resample('D').asfreq()
            Colorado	Texas	     New York	Ohio
2000-01-05	0.035043	-0.547184	-0.159547	2.398447
2000-01-06	NaN	NaN	NaN	NaN
2000-01-07	NaN	NaN	NaN	NaN
2000-01-08	NaN	NaN	NaN	NaN
2000-01-09	NaN	NaN	NaN	NaN
2000-01-10	NaN	NaN	NaN	NaN
2000-01-11	NaN	NaN	NaN	NaN
2000-01-12	-0.415270	0.039105	2.180710	0.990811

>>> df_daily = frame.resample('D').ffill()
	          Colorado	Texas	    New York	Ohio
2000-01-05	0.035043	-0.547184	-0.159547	2.398447
2000-01-06	0.035043	-0.547184	-0.159547	2.398447
2000-01-07	0.035043	-0.547184	-0.159547	2.398447
2000-01-08	0.035043	-0.547184	-0.159547	2.398447
2000-01-09	0.035043	-0.547184	-0.159547	2.398447
2000-01-10	0.035043	-0.547184	-0.159547	2.398447
2000-01-11	0.035043	-0.547184	-0.159547	2.398447
2000-01-12	-0.415270	0.039105	2.180710	0.990811
```
## 기간 리샘플링
* 기간의 다운샘플링의 경우 대상 빈도는 반드시 원본 빈도의 **하위 기간**이어야 한다.
* 기간의 업샘플링의 경우 대상 빈도는 반드시 원본 빈도의 **상위 기간**이어야 한다.
``` python
frame = pd.DataFrame(np.random.randn(24,4),
                    index=pd.period_range('1-2000', '12-2001', freq='M'),
                    columns=['Colorado','Texas','New York','Ohio'])

>>> annual_frame = frame.resample('A-DEC').mean()
	    Colorado	Texas	    New York	Ohio
2000	0.092220	0.483889	-0.134155	0.367483
2001	0.237316	-0.169246	-0.206669	-0.290338

>>> annual_daily.resample('Q-DEC').ffill()
        Colorado	Texas	    New York	Ohio
2000Q1	0.092220	0.483889	-0.134155	0.367483
2000Q2	0.092220	0.483889	-0.134155	0.367483
2000Q3	0.092220	0.483889	-0.134155	0.367483
2000Q4	0.092220	0.483889	-0.134155	0.367483
2001Q1	0.237316	-0.169246	-0.206669	-0.290338
2001Q2	0.237316	-0.169246	-0.206669	-0.290338
2001Q3	0.237316	-0.169246	-0.206669	-0.290338
2001Q4	0.237316	-0.169246	-0.206669	-0.290338

>>> annual_daily.resample('Q-DEC', convention='end').ffill()
        Colorado	Texas	    New York	Ohio
2000Q4	0.092220	0.483889	-0.134155	0.367483
2001Q1	0.092220	0.483889	-0.134155	0.367483
2001Q2	0.092220	0.483889	-0.134155	0.367483
2001Q3	0.092220	0.483889	-0.134155	0.367483
2001Q4	0.237316	-0.169246	-0.206669	-0.290338

```
# 이동창 함수
## rolling
이동창과 여타 함수들을 활용하여 누락된 데이터로 인해 매끄럽지 않은 시계열 데이터를 다듬을 수 있다.
이동창 함수도 누락된 데이터를 자동으로 배제 한다.
``` python
# 20일 크기의 이동창 평균
frame.rolling('20D').mean()

# 250일 크기의 창으로 그룹핑한 객체를 생성. 이동평균을 구함
ma250 = frame.rolling(250).mean()

 # 이동창은 창의 크기보다 적은 데이터를 가지는 구간이 발생하므로 이에 대한 옵션 지정 가능. 
ma250 = frame.rolling(250, min_periods=10).mean()
```
## 확장창 함수 : expending
시계열의 시작점에서부터 창의 크기가 시계열의 전체 크기가 될 때까지 점점 창을 키움
``` python
expanding250 = ma250.expanding().mean()
```
## 지수 가중 함수 : ewm
감쇠 인자 상수에 많은 가중치를 주어 더 최근 값을 관찰함.
``` python
frame.ewm(span=30).mean()
```
## 이진 이동창 함수
상관관계나 공분산과 같은 몇몇 통계 연산은 두개의 시계열을 필요로 한다.
``` python
frame1.rolling('20D').corr(frame2)
```
## 사용자 정의 이동창 함수
rolling 등의 메서드에 apply를 호출해 사용자 정의 연산을 수행할 수 있다.
``` python
from scipy.stats import percentileofscore

score_at_2percent = lambda x: percentileofscore(x, 0.02)

result = frame.rolling(250).apply(score_at_2percent)
```

