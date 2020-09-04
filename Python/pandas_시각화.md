**시각화 라이브러리는 다양한 패턴이 있으므로 해당 라이브러리 API와 Gallery를 직접 확인하는 것이 더 편하다.**
* [matplotlib Gallery](https://matplotlib.org/3.1.1/gallery/index.html)
* [Seaborn Gallery](https://seaborn.pydata.org/examples/index.html)
* [Plotly Gallery](https://plotly.com/python/)

# 폰트
``` python
# matplotlib 한글 깨짐 방지
import matplotlib.font_manager as fm
from matplotlib.pyplot import rc

# 설치 폰트 확인
font_list = [font.name for font in fm.fontManager.ttflist]
for idx, f in enumerate(font_list):
    print(idx, f)
f_name = fm.FontProperties(fname=font_list[44])
font_name = f_name.get_name() # NanumSquareRound

# 폰트 설정
rc('font', family=font_name) 
#font = {'family': font_name,
#       'weight': 'blod',
#       'size': 'larger'}
#rc('font',**font)

# 마이너스 폰트
plt.rcParams['axes.unicode_minus'] = False
```

# Ipython display
``` python
# Shell의 모든 값을 연속하여 출력하기
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"
```

# matplotlib
## 설정
```  python
immprt matplotlib.pyplot as plt

# 화면 내에 표시
%matplotlib inline 
# 별도의 화면에 표시
%matplotlib tk(또는 qt5)

# figsize를 전역으로 설정
plt.rcParams['figure.figsize'] = (10, 10)

# plt.rc()로 설정하기
plt.rc('figure', figsize=(10,10))
# 첫 번째 인자: figure, axes, xtick, ytick, grid, legend 등 컴포넌트 이름
# 두 번째 인자: 설정값
```
## Figure & subplot
* figure , subplot
``` python
import matplotlib.pyplot as plt
fig = plt.figure()
ax1 = fig.add_subplot(1,2,1)
ax2 = fig.add_subplot(1,2,2)

ax1.plot()
ax2.plot()
```
* subplots
``` python
fig, axes = plt.subplots(nrows=1, ncols=2, sharex=False, sharey=False)
axes[0].plot()
axes[1].plot()

fig, axes = plt.subplots(nrows=2, ncols=2, sharex=False, sharey=False)
axes[0,0].plot()
axes[0,1].plot()
axes[1,0].plot()
axes[1,1].plot()
```
* subplot 간격
``` python
# 직접 설정하기
plt.subplots_adjust(left=None, bottom=None, right=None, top=None, wspace=None, hspace=None)

# 단단한 레이아웃
plt.tight_layout()
```
## 눈금, 라벨, 범례
``` python
# 제목 : title
ax.set_title("title_1")

# 축 눈금 : tick
ax.set_xticks([0,10,20,30])
ax.set_xticklabels(['One', 'Two', 'Three', 'Four'], rotation=30, fontsize='small')

# 축 범위 : lim
ax.set_xlim(left, right)

# 축 스케일 : scale
ax.yscale('linear') # log, symlog, logit

# 범례
ax.plot(..., label='label')
ax.plot(..., label='_nolegend_') # 범례 생략
plt.legend(loc='best') # upper left, lower center, ...
```
* 축 설정 : axis
``` python
plt.axis([xmin,xmax,ymin,ymax], options)
# options
on, off
equal, scaled
tight, auto (normal)
image, square
```
## 색상, 마커, 선 스타일
``` python
axes.plot(x,y, linestyle='--', color='g', marker='o', drawstyle='steps-post')
```

## 주석 : annotate
``` python
# 일반 주석 : text
ax.text(x좌표, y좌표)
ax.text(x좌표, y좌표, bbox=dict(boxstyle='square',color='gray'))

# 화살표 : arrow
ax.annotate("text_label",xy=(x,y),xytext=(x,y+5),
           arrowprops=dict(facecolor='black', headwidth=4, width=2, headlength=4),
           horizintalalignment='left', verticalalignment='top')
```
## 저장하기
``` python
plt.savefig('figpath.svg') # .pdf, .png
# 옵션
dpi : 해상도. 기본값= 100
facecolor, edgecolore : 플롯 바깥의 배경 색
format : 형식을 명시적으로 지정.
bbox_inches : fig주변 공간 설정. 'tight'= 모두 제거
```

# Seaborn
Seaborn 라이브러리를 import하면 더 나은 가독성을 위해 matplotlib의 기본 color 스킴과 플롯 스타일을 변경한다.
Seaborn은 matplotlib에 비해 더 다양하고 편리한 시각화 패턴을 제공한다.

# Plotly
웹브라우저 상의 동적 대화형 그래프를 그릴 수 있음.
