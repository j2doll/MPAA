# MPAA 전략

:zap: :kr: 본 내용은 systrader79 님의 ```"주식투자 ETF로 시작하라"```의 일부 입니다.

1. MPAA 전략은 자산군 간 분산, 자산군 내 분산, 듀얼 모멘텀, 현금 혼합, 타임 프레임 분산, 수익곡선 모멘텀의 6가지 구조적인 투자 전략이 조합된 동적 자산 배분 전략이다.

2. 기존의 단순한 듀얼 모멘텀 전략의 단점인 횡보장에서의 손실과 자산군 간 상관성 증가에 따른 큰 폭의 손실 문제를 구조적으로 개선한 모델이다.

3. 기존의 동적 자산 배분 모델에 비해 현저히 낮은 손실폭과 안정적이고 높은 수익률을 얻을 수 있다.

4. 다양한 투자 로직이 결합되어 있고, 변수 값들의 인위적인 최적화를 배제하여 변수 값과 무관하게 안정된 성과를 보여준다.

5. 장기투자 시 ```연복리 10%```, ```최대 손실폭 -7%``` 이내의 꾸준하고 우수한 성과를 기대할 수 있다.

6. MPAA 전략 기반의 펀드도 있어 쉽게 투자할 수 있다.

## MPAA 전략의 로직

그러면 지금부터는 MPAA 전략의 구체적인 로직에 대해 알아보겠습니다.

MPAA 전략은 기본적으로 글로벌 멀티 애셋 포트폴리오 전략입니다. 현재 우리나라에는 상당히 다양한 자산군(주식, 채권, 상품 등)의 ETF 가 많이 출시되어 있어 이런 멀티 애셋 포트폴리오를 구성하는 데 전혀 지장이 없을 정도가 되었습니다.

MPAA 전략을 시뮬레이션하기 위해서는 원칙적으로 다양한 ETF 종목의 가격을 기반으로 테스트하는 것이 맞습니다. 하지만, 실제 ETF 가격을 이용하면 백테스트할 수 있는 종목과 기간이 제한됩니다. 그렇기 때문에 여기서는 ETF가 추적하는 다양한 인덱스 데이터를 기준으로 시뮬레이션해보도록 하겠습니다. 테스트 대상이 되는 인덱스는 ETF로 출시된 것도 있고, 출시되지 않은 것도 있습니다. 그러나 워낙 유니버스가 넓고 다양하기 때문에 출시되지 않은 인덱스를 대상으로 시뮬레이션하더라도 분산 효과에 의해 실제 성과에는 큰 차이가 없습니다.

 ### 투자 대상 : ETF로 투자가 가능한 다양한 자산군의 인덱스

- ```주식``` : 12 종목(국가)

	S&P500(미국), 러셀3000(미국), 니케이225(일본), 항셍(홍콩), 홍콩 H(홍콩), 대만 가권, 상해 종합(중국), FTSE 100(영국), CAC 40(프랑스), DAX(독일), KOSPI(한국), SENSEX(인도)

- ```주식(국내 섹터)``` : 26 종목(FnGuide 섹터 인덱스)

	에너지, 화학, 금속 및 광물, 기타 소재, 건설, 기타 자본재, 상업 서비스, 운송, 자동차 및 부품, 소비재 및 의류, 소비자 서비스, 미디어, 유통, 음식료 및 담배, 생활용품, 의료, 은행, 보험, 증권, 기타 금융, 소프트웨어, 하드웨어, 반도체, 디스플레이, 통신서비스, 유틸리티

- ```주식 팩터(국내 스마트 베타)``` : 30 종목(FnGuide 인덱스)

	블루칩30, 모멘텀, 경기방어주, 베타플러스, 로우볼, 배당성장, 빅볼지수, 컨트래리안, 퀄리티밸류, 대형가치, 대형성장, 대형순수가치, 대형순수성장, 중형가치, 중형순수가치, 중형순수성장, 소형가치, 소형성장, 소형순수가치, 소형순수성장, 중대형가치, 중대형성장, 중대형순수가치, 중대형순수성장, 중소형가치, 중소형순수가치, 중소형성장, 중소형순수성장

- ```채권(국내 장기 국고채)``` : 3 종목

	10년 만기 국고채 총수익 지수, 20년 만기 국고채 총수익 지수(10년 만기 국고채 레버리지 지수로 계산), 10년 만기 채권 인버스 지수(10년 만기 국고채 지수 인버스로 계산)

- ```현금(국내 단기 국고채)``` : 1 종목

	3년 만기 국고채 총수익 지수

 
### 투자 방법

#### Step 1 : 상대 모멘텀 적용

현금을 제외한 각 자산군(```주식 국가```, ```주식 섹터```, ```주식 팩터```, ```채권```)에 상대 모멘텀 전략을 적용합니다.

- 1 단계 : 각 자산군에 속한 개별 종목의 ```1~12개월 평균 모멘텀```을 각각 계산합니다.

- 2 단계 : 각 자산군에 속한 종목을 1~12개월 평균 모멘텀이 ```높은 순으로 정렬```합니다.

- 3 단계 : 각 자산군에서 평균 모멘텀이 높은 ```상위 30% 종목```을 선정합니다.

  * ```주식 국가``` 자산군은 총 12 종목이므로, ```12 종목의 상위 30%``` = 3.6 종목(=4종목), 모멘텀이 높은 상위 ```4 종목``` 선정

  - ```주식 섹터``` 자산군은 총 26 종목이므로, 대략 ```9 종목``` 선정

  - ```주식 팩터``` 자산군은 총 30 종목이므로, ```10 종목``` 선정

  - ```채권``` 자산군은 3 종목이므로, ```1 종목``` 선정
 
#### Step 2 : 절대 모멘텀 적용(현금 혼합)

- 1 단계 : Step 1 에서 선정된 상대 모멘텀 ```상위 종목```의 ```1~12개월 평균 모멘텀 스코어```를 각각 계산합니다.

- 2 단계 : 선정된 각각의 종목을 ```평균 모멘텀 스코어에 비례```하여 일정 비율의 ```현금과 혼합```합니다.

	* Step 1 ```주식 국가```에서 ```S&P500```이 선정되었고, 이때 ```평균 모멘텀 스코어```가 ```0.75``` 였습니다. 
	* ```현금``` 비중을 ```1```로 정한 경우, ```S&P500 : 현금```= ```0.75 : 1```. 
	* 이런 방식으로 선정된 종목에 대해 현금과 혼합한 포트폴리오를 구성합니다(```현금 혼합 비율```은 ```투자자 기호```에 맞게 조절 가능).

#### Step 3 : 자산군 배분 비중 적용

- 1 단계 : 각 자산군 간의 배분 비중을 결정합니다. 투자자 기호에 맞게 조절할 수 있으나 시장 상황에 무관한 절대 수익을 얻기 위해서는 ```주식군```과 ```채권군```을 ```1 : 1``` 로 배분하는 것이 가장 안정적입니다. 주식군은 ```국가, 섹터, 팩터```의 3가지 군인데, 채권은 1가지 군이므로 주식군과 채권군의 비중을 1 : 1 로 맞추려면 ```주식 국가``` : ```주식 섹터``` : ```주식 팩터``` : ```채권``` = ```1 : 1 : 1 : 3``` 이 되어야 합니다 (1+1+1=3).

- 2 단계 : 정해진 전체 자산군의 배분비에 따라, Step 2에서 선정된 모멘텀 비중 현금 혼합 포트폴리오 개별 종목에 비례하여 배분합니다.

	* ```주식 국가군```의 기본 배분비율 = 1 / (1 + 1 + 1 + 3) = 0.17 (```17%```)

	* ```주식 국가군```에서 선정된 ```종목 수``` = ```4 종목```

	* 주식 국가군에서 선정된 모멘텀 비례 현금 혼합 포트폴리오 ```한 종목```당 투자 ```비중``` = 17% × 1/4 = ```4.25%```

	* 이런 방식으로 각 자산군에서 선정된 종목의 현금 혼합 포트폴리오의 개별 배분 비중을 결정합니다.

#### Step 4 : 수익곡선 모멘텀

- 1 단계 : Step 3 의 과정을 ```매월 말 반복```하여 ```리밸런싱```합니다.

- 2 단계 : 이를 통해 나타난 전체 투자의 수익곡선을 하나의 종목처럼 간주, 이것의 ```1~6개월 평균 모멘텀 스코어```를 계산합니다.

- 3 단계 : 최종 투자 비중 결정

   ```MPAA 전략 투자 비중``` : ```현금``` = ```수익곡선 1~6개월 평균 모멘텀 스코어``` : ```1-수익곡선 1~6개월 평균 모멘텀 스코어```

   ＊ MPAA 전략 최근 6개월 평균 모멘텀 스코어가 0.75인 경우, 
   
     MPAA 전략 투자 비중 : 현금 = 0.75 : 0.25

- 4 단계 : ```매월 리밸런싱```

