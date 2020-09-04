# 날짜, 시간 자료형
``` python
from datetime import datetime, timedelta

>>> datetime.now()
datetime.datetime(2020, 2, 6, 0, 35, 37, 707379)

start = datetime(2020,1,1)
>>> start + timedelta(12)
datetime.datetime(2020, 1, 13, 0, 0)
```
## 문자열과 datetime
* datetime >> 문자열
``` python
stamp = datetime(2020,1,1)

>>> str(stamp)
'2020-01-01 00:00:00'

>>> stamp.strftime('%Y-%m-%d')
'2020-01-01'
```
* 문자열 >> datetime
``` python
value = '2020-01-01'

>>> datetime.strptime(value, '%Y-%m-%d')
datetime.datetime(2020, 1, 1, 0, 0)
```
* dateutil과 parse
``` python
from dateutil.parser import parse

>>> parse('2020-01-01')
datetime.datetime(2020, 1, 1, 0, 0)
```
* pandas.to_datetime
``` python
datestr = ['2020-01-01 12:00:00', '2020-01-02 12:00:00']

>>> pd.to_datetime(datestr)
DatetimeIndex(['2020-01-01 12:00:00', '2020-01-02 12:00:00'], dtype='datetime64[ns]', freq=None)
```

# 시계열 기초
pandas 시계열 객체는 datetime으로 색인된 타임스탬프 객체이다. 
이 객체들간 연산은 자동으로 날짜에 맞춰진다.

``` python
dates = [datetime(2020,1,2), datetime(2020,1,5), datetime(2020,1,7), datetime(2020,1,8), datetime(2020,1,10), datetime(2020,1,12)]

ts = pd.Series(np.random.randn(6), index=dates)

>>> ts.index
DatetimeIndex(['2020-01-02', '2020-01-05', '2020-01-07', '2020-01-08',
               '2020-01-10', '2020-01-12'], dtype='datetime64[ns]', freq=None)
>>> ts.index[0]
Timestamp('2020-01-02 00:00:00')

>>> ts + ts[::2]
2020-01-02   -2.813032
2020-01-05         NaN
2020-01-07   -0.291461
2020-01-08         NaN
2020-01-10    0.239208
2020-01-12         NaN
```
## 색인, 선택
``` python
dates = [datetime(2020,1,2), datetime(2020,1,5), datetime(2020,2,1), datetime(2020,2,5), datetime(2021,1,10), datetime(2021,1,12)]

ts = pd.Series(np.random.randn(6), index=dates)

>>> ts['2021']
2021-01-10    0.326049
2021-01-12   -1.454251

>>> ts['2020-01']
2020-01-02    1.109855
2020-01-05    0.530081

>>> ts['2020-02-05':]
2020-02-05    1.976165
2021-01-10    0.326049
2021-01-12   -1.454251
```
## 날짜 범위, 빈도, 이동
* date_range : 특정 빈도와 길이에 따라 인덱스 생성
``` python
index = pd.date_range('2020-04-01','2020-06-01') # 범위 지정
index = pd.date_range(start='2020-04-01', periods=20) # start, 기간 지정
index = pd.date_range(end='2020-06-01', periods=20) # end, 기간 지정
index = pd.date_range('2020-04-01','2020-06-01', freq='BM') # 범위, 빈도 지정
index = pd.date_range('2020-04-01 12:56:31', periods=5, normalize=True) # 범위 지정 및 자정에 맞춤(정규화)
```
* 빈도, 날짜 오프셋
[시계열 빈도](https://pandas.pydata.org/docs/user_guide/timeseries.html#dateoffset-objects)
``` python
from pandas.tseries.offsets import Hour, Minute

four_hour = Hour(4)
>>> four_hour
<4 * Hours>

index = pd.data_range('2000-01-01', '2000-01-03 23:59', freq='4h') # 4시간 빈도
index = pd.data_range('2000-01-01', periods=10, freq='1h30min') # 90분 빈도
index = pd.data_range('2000-01-01', '2000-09-01', freq='WOM-3FRI') # 매월 3째주 금요일
```
* shift : 인덱스 변경 없이 데이터를 이동
``` python
ts = pd.Series(np.random.randn(4),
              index=pd.date_range('1/1/2020', periods=4, freq='M'))

>>> ts.shift(2)
2020-01-31         NaN
2020-02-29         NaN
2020-03-31   -0.228032
2020-04-30   -0.876859

>>> ts.shift(-2)
2020-01-31   -1.008437
2020-02-29    0.583569
2020-03-31         NaN
2020-04-30         NaN

>>> ts.shift(2, freq='M') # 빈도를 넘기면 timestamp 확장 가능
2020-03-31    1.238092
2020-04-30   -0.248921
2020-05-31   -1.653268
2020-06-30   -0.186429
```
## 시간대
``` python
import pytz

>>> pytz.common_timezones[-5:]
['US/Eastern', 'US/Hawaii', 'US/Mountain', 'US/Pacific', 'UTC']

>>> pytz.timezone('America/New_York')
<DstTzInfo 'America/New_York' LMT-1 day, 19:04:00 STD>
```
``` python
>>> pd.date_range('3/9/2020 9:30', periods=10, freq='D', tz='UTC')
DatetimeIndex(['2020-03-09 09:30:00+00:00', '2020-03-10 09:30:00+00:00',
               '2020-03-11 09:30:00+00:00', '2020-03-12 09:30:00+00:00',
               '2020-03-13 09:30:00+00:00', '2020-03-14 09:30:00+00:00',
               '2020-03-15 09:30:00+00:00', '2020-03-16 09:30:00+00:00',
               '2020-03-17 09:30:00+00:00', '2020-03-18 09:30:00+00:00'],
              dtype='datetime64[ns, UTC]', freq='D')
```
* 지역화 시간으로 변환
``` python
ts_utc = ts.tz_localize('UTC')
```
* 시간대 변경
``` python
ts_utc.tz_convert('America/New_York')
```

## 기간
* Period / period_range
``` python
p = pd.Period(2007, freq='A-DEC')
>>> p
Period('2007', 'A-DEC') # 2007 1/1 ~ 12/31의 period 객체

rng = pd.period_range('2000-01-01', '2000-06-30', freq='M')
>>> rng
PeriodIndex(['2000-01', '2000-02', '2000-03', '2000-04', '2000-05', '2000-06'], dtype='period[M]', freq='M')

idx = pd.PeriodIndex(['2001Q3','2002Q2','2003Q1'], freq='Q-DEC')
>>> idx
PeriodIndex(['2001Q3', '2002Q2', '2003Q1'], dtype='period[Q-DEC]', freq='Q-DEC')
```
* 기간 객체의 연산
``` python
>>> p + 5
Period('2012', 'A-DEC')

>>> pd.Period('2014', freq='A-DEC') - p
7
```
* 빈도 변환
``` python
p.asfreq('M', how='start')
p.asfreq('M', how='end')
```
* timestamp와 period 변환
``` python
rng = pd.date_range('2000-01-01', periods=3, freq='M')
ts = pd.Series(np.random.randn(3), index=rng)
pts = ts.to_period()

>>> pts
2000-01   -1.685143
2000-02   -1.154184
2000-03   -0.136544

>>> pts.to_timestamp()
2000-01-01   -1.685143
2000-02-01   -1.154184
2000-03-01   -0.136544
```
* 여러 칼럼으로 PeriodIndex 생성
``` python
data = pd.DataFrame({'year':[2000,2000,2001,2001,2002,2002],
                    'quarter':[1,3,1,3,1,3]})
>>> pd.PeriodIndex(year=data.year, quarter=data.quarter, freq='Q-DEC')
PeriodIndex(['2000Q1', '2000Q3', '2001Q1', '2001Q3', '2002Q1', '2002Q3'], dtype='period[Q-DEC]', freq='Q-DEC')
```
