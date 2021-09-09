---

title: “Review: algorithmic trading(Jan Fraenkle & Svetlozar, 2009)”

tags:

- Market Micorstructure

- Algorithmic Trading

---

  

Jan Fraenkle, Svetlozar T. Rachev의 “Review: algorithmic trading”을 정리하는 글이다.

  

## Intorudction

  

Algorithmic trading은 시장 참여자와 시장을 연결해주며, 시장에 영향 없이 거래를 smooth하게 성사시키는데 목표를 두고 있다. 이를 위해 market microsturcture와 trading stategy에 대한 이해를 해보려한다.

  

## 1. Market Microstructure

  

금융 시장은 증권 거래를 희망하는 party들을 한 곳에 모으는 역할을 한다. 그 거래를 진행하는 platform은 broker를 통해서 이루질 수도 있으며 electronic한 환경에서 이루어질 수도 있다. New York Stock Exchange(NYSE)는 두 가지 성질을 모두 갖는 hybrid market의 대표적인 예이다.

시장은 여러 주문들을 높은 liquidity와 낮은 가격 volatility 하에 빠르게 성사키는 것이 주요 과제이다. Walrasian auction이 기본적인 거래 방식이며 discrete하게 이루어진다. 하지만 현재 주식시장은 continuous trading을 제공하며 추가적인 discrete call auction을 가지기도 한다.

  

### Open limit order book

  

Open limit order books는 continuous trading의 핵심적인 요소이다. Limit price, quantity of share나 trading direction같은 정보를 가지고 있으며 이 모두를 공개하는 형태이다. 반대되는 개념으로 closed order book(dark pool)이 있다. Bid-ask spread는 open limit order book의 measure인데, 증권의 유동성을 나타내는 좋은 지표가 된다. Bid-ask spread가 작으면 거래가 활발하게, 유동적으로 일어난다고 본다.

Trading은 buy order이 ask보다 높을 때 일어난다. 그 외의 주문들은 book에 추가되며, 거래는 항상 limit price에서 발생한다. 이는 결국 증권 가격의 변동을 야기한다.

### Trading cost

  

Trading cost는 3가지로 구성되는데, 1. fees and commissions for brokers, 2. market impact cost, 3. oppertunity cost이다.

이 중에서 market impact는 짧은 시간에 큰 거래가 이루어질 때 커진다. 그 원인은 information acquisition과 liquidity에 대한 수요에 있다. 반면 oppertunity cost는 원래 원했던 거래량보다 적은 거래가 일어날 때 발생한다. 또한 이는 investor specific하여 market impact사이의 trade-off를 찾아 cost를 최소화해야 한다.

  

## 2. Trading Strategies

  

### Benchmarks

  

Benchmark는 trading strategy의 기준이 되는 점이며 이를 어떻게 설정하냐에 따라서 전략의 방향성이 달라진다. 이는 execution price와 비교되며 그 중에서도 pre-, intra-, post trade price, 3가지 정도로 나뉜다. Pre-trade price는 arrival price와 같이 주문이 도착하는데 까지의 price이다. 거래에 직접적인 영향을 받지 않으므로 market impact의 mearsure로 적합하다. Intra-trade price는 뒤에서 설명할 VWAP나 TWAP가 좋은 예시가 될 수 있고, post-trade prices는 close price가 그 예가 될 수 있겠다.

### Examples of algorithmic trading strategy

#### Arrival price
첫 order가 전송되기 직전의 증권 가격을 arrival price라고 한다. 이를 benchmark로 한다면 (implementation shortfall) 거래 초반에 trading volume을 집중하여 변동성 risk를 최소화한다. 

#### TWAP(time weighted average price)
TWAP로 시간을 동일하게 나누어, 동일한 수량을 기계적으로 주문한다.

#### VWAP(volume weighted average price)
$VWAP = \Sigma Price*Volume/\Sigma Volume$
Market impact를 줄이기 위해 VWAP를 참고하여 전략을 세운다. 시간을 기준으로 trading period를 등분하고 각각의 slot에 trading volume을 배분하는데, 그 기준은 과거 자료를 참고한다. 

#### TVOL(target volume)
Benchmark를 설정하지 않고 현재 시점을 기준으로 고정된 target volume을 거래한다. 

## 3. Empirical analysis

### Trading volume
증권의 trading volume은 유동성을 판단하는데 중요한 지표이다. Trading volume은 seasonality를 띈다. 하루 동안의 거래량을 그래프로 나타내보면 U자형을 띈다. 즉, 시장 개장 직후와 폐장 직전에 높은 거래량이 관측되며, 정오에 가장 적은 량의 거래가 이루어진다.

### Dynamics in trading volume
PCA(principal component analysis)를 stock turnover에 적용해보면 데이터를 간소화하여 dynamics를 확인할 수 있다. 시간과 asset에 따른 turnover series의 variance-covariance matrix을 decompostion 해주면 다음과 같은 식을 얻을 수 있다.
$\frac{x_{it}-\bar x_i}{\sigma_i}=\sum_{k}u_k^iC_t^k$
$x_{it}$는 i번째 asset의 시간t에서의 turnover이며 $u_k^i$는 i번째 component의 k번째 eigenvector이고 $C_t^k=x'_{it}u_k$,$Cov(C_t^k,C_t^l)=\lambda_k\sigma_kl$이다. 그리고 이 식을 다시 재배열하면 다음과 같은 식이 나온다.
$x_{it}-\bar x_i=\frac{1}{\lambda_1}Cov(x_{it},C_t^1)C_t^1+\sum_{k>1}Cov(x_{it},C_t^k)C_t^k$
여기서 첫 번째 term은 market turnover, 그 외는 short-term arbitrage라고 해석할 수 있다. Market에 따라 두 번째 항은 일정하며 첫 번째 항이 trend에 따라 변화한다.

## 4. Algorithmic Trading

컴퓨터의 발전에 따라 짧아진 reaction time과 빠른 decision으로 인간이 직접하는 trading에 비해 많은 이점을 갖는다. 이때 strategy를 발전시키고 백테스트하여 좋은 결과를 얻을 수 있도록 해야한다. 
Market은 fragmented되어있고 execution에 복잡한 영향을 받기에 분석하기 쉽지 않지만, 컴퓨터의 발전으로 미래에는 이를 분석할 수 있을 것이다.
