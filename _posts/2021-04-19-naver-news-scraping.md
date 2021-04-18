---
layout: single
title:  "네이버 뉴스 부동산페이지 웹스크래퍼"
---

네이버 뉴스 부동산 페이지 웹스크래퍼를 만들어 보았는데요.

각 기사별 제목과 언론사, 그리고 게시된 날짜를 가져올 수 있었습니다.



## 필요한 모듈 가져오기

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
from datetime import datetime
```


```python
def get_data(url):
    resp = requests.get(url)
    html = BeautifulSoup(resp.content, 'html.parser')
    score_result = html.find('div', {'class': 'economy_submenu'})
    lis = score_result.findAll('li')
    
    df = pd.DataFrame(columns=('title', 'journal', 'date'))
    
    for li in lis:
        title = li.find('strong').getText()
        journal = li.find('b').getText()
        date = datetime.strptime(li.findAll('b')[-1].getText().split(' ')[0], "%Y.%m.%d.")
        df = df.append(pd.DataFrame([[title, journal, date]], columns=['title', 'journal', 'date']), ignore_index=True)
    
    return df
```


```python
test_url = 'https://m.news.naver.com/newsflash.nhn?mode=LS2D&sid1=101&sid2=260'
resp = requests.get(test_url)
html = BeautifulSoup(resp.content, 'html.parser')
total_count = 5 #웹스크래핑 할 페이지 수

df = pd.DataFrame(columns=('title', 'journal', 'date'))

for i in range(2, total_count): 
    #2부터 잡은 이유는... 당일 게시된 기사의 경우 날짜가 표시되지 않아 문자열 파싱 문제로 스크래핑이 안됩니다...
    url = test_url + '&page=' + str(i)
    print('url: "' + url + '" is parsing....')
    df = df.append(get_data(url), ignore_index=True)
    
df
```

    url: "https://m.news.naver.com/newsflash.nhn?mode=LS2D&sid1=101&sid2=260&page=2" is parsing....
    url: "https://m.news.naver.com/newsflash.nhn?mode=LS2D&sid1=101&sid2=260&page=3" is parsing....
    url: "https://m.news.naver.com/newsflash.nhn?mode=LS2D&sid1=101&sid2=260&page=4" is parsing....
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>journal</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>호반건설, 인천 동진3차 가로주택정비사업 맡는다</td>
      <td>파이낸셜뉴스</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>1</th>
      <td>‘해외 부진’ 대형 건설사, 국내 주택사업이 1분기 먹여살렸다</td>
      <td>파이낸셜뉴스</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>"올 공시가격 동결하고   산정과정 감사원 조사"</td>
      <td>서울경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>3</th>
      <td>'40년 모기지'에 주거용 오피스텔은 빠질듯</td>
      <td>서울경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>4</th>
      <td>서울시 주택본부 확대… 오세훈의 ‘공급 드라이브’</td>
      <td>조선비즈</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>5</th>
      <td>野 5개 단체장 공시가 반발···"조세로 엄포 놓는 부동산 정책 멈춰라"</td>
      <td>서울경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>6</th>
      <td>포스코건설 `더샵 양평리버포레` 이달 분양</td>
      <td>디지털타임스</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>7</th>
      <td>이의신청 4년전보다 30배 늘어…"공시가 작년수준 동결하라"</td>
      <td>매일경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>8</th>
      <td>野소속 5개 광역단체장 "공시가 동결하고 조사권한 이양해야"</td>
      <td>파이낸셜뉴스</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>9</th>
      <td>서울 재건축발 급등 우려에… ‘공급확대-규제’ 투트랙 검토</td>
      <td>파이낸셜뉴스</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>10</th>
      <td>노형욱 "2·4대책 계승"… 서울시와 부동산 정책 갈등 예고?</td>
      <td>파이낸셜뉴스</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2년간 임대 수익 보장해 투자 안정성 확보</td>
      <td>파이낸셜뉴스</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>12</th>
      <td>노형욱 국토부장관 내정자, 첩첩산중 당면과제 어떻게 넘을까</td>
      <td>경향신문</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>13</th>
      <td>"최문순 강원도지사 탄핵을 요구합니다"…`한중 문화타운 건설`에 들끓는 민심</td>
      <td>디지털타임스</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>14</th>
      <td>[분양캘린더] 전국서 5,500가구 쏟아져···검단서 대규모 분양도</td>
      <td>서울경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>15</th>
      <td>오세훈의 ‘35층 룰’ 폐지, 결국 기부채납 규모에 달려</td>
      <td>서울경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>16</th>
      <td>반도건설, ESG경영 강화…사업다각화 본격 추진</td>
      <td>데일리안</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>17</th>
      <td>한강변 용산 산호아파트 최고 35층으로 재건축</td>
      <td>서울경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>18</th>
      <td>10년 암흑기 끝나간다…서울 재건축 '吳! 별의 순간' 올까</td>
      <td>한국경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>19</th>
      <td>용적률 높이려면 조례개정 필수…의회 협조 '난관'</td>
      <td>한국경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>20</th>
      <td>길 뚫리면 집값 '탄탄대로'…화성·오산 분양 노려볼까</td>
      <td>한국경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>21</th>
      <td>비규제 지역선 부부 각각 청약해 같은 단지 2채 당첨 가능</td>
      <td>한국경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>22</th>
      <td>방배동 재건축 입주권 '웃돈 3억 더'…"매물 없어서 못판다"</td>
      <td>한국경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>23</th>
      <td>"도시재생 폐지"…뉴타운 반대파도 돌아섰다 [창신동 르포]</td>
      <td>매일경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>24</th>
      <td>吳 "서울시민 셋 중 하나는 재산세 전년보다 30% 올랐다"</td>
      <td>매일경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>25</th>
      <td>서울시 주택본부 확대 개편…'오세훈표 공급' 속도내나</td>
      <td>매일경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>26</th>
      <td>1172가구 검단 예미지 퍼스트포레 분양</td>
      <td>매일경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>27</th>
      <td>'강남권' 2·4대책 후보지, 내달 발표…일원 대청마을 유력</td>
      <td>머니투데이</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>28</th>
      <td>野광역단체장 "공시가 동결" 공개 압박…정부여당 '진퇴양난'</td>
      <td>이데일리</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>29</th>
      <td>국민의힘 5개 시도지사 "공시가격 동결…결정권 지자체로 넘겨야"</td>
      <td>한국경제TV</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>30</th>
      <td>野 시·도지사 5人 "공시가격 결정권 지자체에 넘겨라"</td>
      <td>한국경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>31</th>
      <td>'4차 국가철도망 계획' 발표 초읽기…건설·부동산업계 '촉각'</td>
      <td>아이뉴스24</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>32</th>
      <td>똑같은 구조 지겹다…건설사들 선보이는 신평면 이모저모 [부동산360]</td>
      <td>헤럴드경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>33</th>
      <td>아파트값 '들썩'</td>
      <td>뉴스1</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>34</th>
      <td>서울 집값 들썩</td>
      <td>뉴스1</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>35</th>
      <td>재건축 규제 완화 기대감에 뛰는 집값</td>
      <td>뉴스1</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>36</th>
      <td>'재건축 규제 완화 기대감' 집값 들썩</td>
      <td>뉴스1</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>37</th>
      <td>오세훈 당선 일주일.. 뛰는 아파트 가격</td>
      <td>뉴스1</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>38</th>
      <td>상승하는 아파트 매매 가격</td>
      <td>뉴스1</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>39</th>
      <td>천정부지로 뛰는 재건축 기대감</td>
      <td>뉴스1</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>40</th>
      <td>오세훈 한강르네상스 첫 주자…성수동 50층 개발 '재시동'</td>
      <td>매일경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>41</th>
      <td>블루보틀도 둥지…소녀시대 슈퍼주니어 '인싸' 몰리는 성수동 '천지개벽'</td>
      <td>매일경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>42</th>
      <td>포스코건설, 일산 풍동2지구 도시개발사업 수주…공사비 1조1000억</td>
      <td>국민일보</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>43</th>
      <td>변창흠 109일만에 불명예 퇴진…노형욱 후보자 집값 잡을 수 있을까</td>
      <td>중앙일보</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>44</th>
      <td>'예산통' 노형욱, '정책 계승' 방점..오세훈과 협력이냐 대립이냐</td>
      <td>파이낸셜뉴스</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>45</th>
      <td>오세훈 "서울시민 3명중 1명 재산세 늘어…공시가 급등이 문제"</td>
      <td>머니투데이</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>46</th>
      <td>강남 신축도 신고가 속출… 매수심리 반등 시작</td>
      <td>국민일보</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>47</th>
      <td>금호건설, ‘포천 금호어울림 센트럴’ 4월 분양</td>
      <td>이데일리</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>48</th>
      <td>민주당, 부동산 정책 수정 작업 돌입···보유세 완화하나</td>
      <td>서울경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>49</th>
      <td>반도건설, 부산 광안지역주택조합 사업 수주…908억 규모</td>
      <td>이데일리</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>50</th>
      <td>'공시가 동결' '조사권한 이양'...독해진 오세훈, 정부와 본격 대립각</td>
      <td>파이낸셜뉴스</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>51</th>
      <td>野지자체장, 공시가격 맹공…與 세부담 완화 추진하나</td>
      <td>뉴시스</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>52</th>
      <td>'오세훈 시장 1주일'…부푼 기대감에 재건축 아파트값 2~3억원씩 '들썩'</td>
      <td>아시아경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>53</th>
      <td>원희룡 “정부 일방적 공시가 현실화, 조세법률주의 어긋나”</td>
      <td>이데일리</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>54</th>
      <td>野시도지사들 "공시가 동결·결정권 지자체 이양" 촉구(종합)</td>
      <td>연합뉴스</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>55</th>
      <td>창원 마창대교 반도유보라 아이비파크, 19일부터 당첨자 계약</td>
      <td>이데일리</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>56</th>
      <td>라온건설, 남양주 ‘덕소 강변 라온프라이빗’ 공급 나서</td>
      <td>한국경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>57</th>
      <td>野 소속 광역단체장 "공시가격 신뢰도 떨어져…동결해야"</td>
      <td>아시아경제</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>58</th>
      <td>오세훈·박형준… 국민의힘 5개 시·도지사 “공시가격 동결”</td>
      <td>조선비즈</td>
      <td>2021-04-18</td>
    </tr>
    <tr>
      <th>59</th>
      <td>野광역지자체장들, 文대통령에 "공시가 동결" 공동건의</td>
      <td>이데일리</td>
      <td>2021-04-18</td>
    </tr>
  </tbody>
</table>
</div>


