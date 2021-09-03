---
title: "Algorithmic and high-frequency trading strategies: A literature review(Mandes & Alexandru, 2016)"
tags:
  - HFT
  - Market Micorstructure
  - Trading
use_math: true
---

이 글은 Mandes, Alexandru (2016)의 Algorithmic and high-frequency trading strategies: A literature review를 정리한 post입니다.

## 1. Intorudction

구조화된 거래 시스템이 발전하면서 시장 참여자들은 매수(bid), 매도(ask) 호가를 통해 시장에 참여할 수 있게 되었다. 시장 참여자들 중 informed trader는 그들이 아는 alpha가 시장에서 사라지기 전에 포지션을 열기 위해 공격적으로 시장가 주문(market order)을 넣는 경우가 많고 따라서 liquidity taker인 경우가 많다. 이러한 공격적 주문은 호가창의 불균형을 야기하기도 한다.  

반대로 uniformed trader의 경우 위와 같은 informed trader들이 시장에 참여하면서 기존에 소유하고 있는 포지션과 반대 방향으로 가격이 움직일 위험해 빠지게 된다. 이들은 이러한 위험을 해결하기 위해 지정가 주문(limit order)을 이용하고 따라서 liquidity provider인 경우가 많다. 

시장에서는 이렇듯 지속적으로 시장과 유동성의 변화를 반영해 호가를 업데이트 하려는 liquidity provider와 자신들이 아는 정보의 유출을 숨기고 빠른 시간안에 최대한 적은 비용으로, 최대한 많은 position을 열려고 하는 informed trader들이 경쟁하게 된다. 

기술의 발전은 양쪽 모두에게 이득이 되었다. liquidity provider들은 자동 마켓 메이킹 프로그램을 개발하였고 informed trader들은 거래 비용을 낮추기 위해 Algorithmic Trading(AT) 전략을 개발하였다. 또한 주문 집행까지의 시간을 단축하여 속도의 우위를 점하고 이를 이용해 수익성을 극대화하는 High-Frequency Trading(HFT)이 나타났다. 

본 논문에서는 이러한 AT 전략과 HFT 전략을 다룬 다양한 문헌들을 정리해 보고자 한다.

## 2. Algorithmic trading

큰 물량을 매수 혹은 매도하는 것은 makret impact와 signaling risk를 높여 위험하다. 이러한 이슈를 해결할 수 있는 방법은 주문을 쪼개어 일정 시간에 걸쳐 매수 혹은 매도 함으로서 내재적 거래비용을 줄이는 것이며 이와 같은 전략적 주문 집행을 자동으로 해주는 것을 Alogrithmic Trading(AT)이라고 한다. AT에 포함되기 위해서는 사람의 개입이 없어야 하며 실제 거래를 위한 trading parameter를 시스템 스스로 결정할 수 있어야 한다. 그 외에 이미 사람이 정해둔 가격에 자동으로 호가를 걸거나 입력해둔 방식으로 자동으로 거래하는 시스템은 AT에 포함되지 않는다.

![Figure 1](../assets/HFT_figure1.png)

매수 혹은 매도하고자 하는 물량이 많을 때 위 그림처럼 정해진 기간 동안 같은 비율로 주문을 처리하는 것이 하나의 방법이 될 수 있을 것이다. 이러한 방법은 시장에 주는 영향력(market impact)은 매우 낮지만 주가의 변동으로 인한 내재적 위험(intrinsic price risk)이 따를 수 있다. 최적의 주문 집행을 찾는 것은 이렇듯 최소 변동 전략($t_0$ 시점에 한번에 매수 혹은 매도)과 최소 영향력 전략($T$ 기간 내에 같은 비율 만큼 매수 혹은 매도)의 트레이드 오프에서 어떻게 문제를 해결할지를 결정하는 것이다.

Almgren, Chriss (2001)는 이러한 주문 집행 과정을 식으로 일반화 하였으며 식은 다음과 같다.

$x(t)$: the number of units reamining to be traded at time t    
$v(t)$: the number of units to be traded in one time interval  

- $x(0)=X_0$, $x(T)=0$  
- $v(t) = x(t-1) - x(t)$

기대 거래 비용과 보유 기간 동안의 불확실성 위험 사이의 균형을 찾는 최적화 문제는 다음 식과 같다.

<div align="center">
    $\min_{x}(E[C(x) + \lambda Var[C(x)])$
</div>

- $C(x)$: cost of deviating from the benchmark  
- $Var[C(x)]$: variance of the execution cost as a proxy for risk  
- $\lambda >= 0$: agent's level of risk aversion which penalizes the varaince relateive to the expected cost  

Johnson(2010)은 AT의 최적화 문제들의 목적 함수를 impact-driven, cost-centric, 그리고 opportunistic algorithms로 분류하였다.

먼저 impact-driven은 schedule-based이고 전체 시장의 market impact로 인한 비용을 최소화 하는 것을 목적으로 한다. 대표적인 예로는 위의 TWAP 방식이 있다. 또 다른 impact-driven 방식은 Volume Weighted Average Price(VWAP)이 있는데 이 방식은 시장의 실제 거래량 중 일부 만큼에 참여하여 거래하는 것을 목표로 한다. 

두번째로 cost-centric 방식은 내재적 비용과 market impact로 인한 비용, 그리고 시간 노출 비용(exposure to timing risk), 시장 참여자의 risk aversion을 모두 조합하여 거래 비용을 낮추는 것을 목표로 한다. 가장 대표적인 예로 Implementation Shortfall이 있는데, 이는 주문량, 가용 시간, 예상 가격 변화량, 예상 유동성 및 변동성 등을 이용해 만들어진 다양한 비용 및 시장 모델을 이용하여 trading schedule을 결정한다. 

마지막으로 opprotunistic algorithms에는 Price Inline(PI)가 대표적이며 현재 시장 상황에 따라 impact-driven 방식을 적절히 적용하는 방식을 의미한다.

### 2-1. Volume-Weighted Average Price

Volume-Weighted Average Price 전략은 VWAP와 비슷한 가격에 주문을 집행하는 것을 목적으로 한다. VWAP 전략은 alpha가 없거나 급하게 주문을 처리할 필요가 없는 passive trader에게 적합하다. 이때 VWAP는 아래의 식으로 계산된다.

<div align="center">
    $$VWAP = \sum v_np_n/\sum v_n$$
</div>  

하지만 실제로 VWAP 전략을 적용할 때 하루 동안의 거래량을 미리 알 수 없는 문제가 발생한다. 이 때문에 actual VWAP와는 다른 가격에 주문이 집행될 수 있다. 이러한 문제는 VWAP 계산 과정에서 historical volume을 사용하지 않고 다른 intraday volume dynamics를 사용하여 개선할 수 있다. 이러한 방식은 거래량을 두 부분으로 나누어 분석하며 이는 각각 일반적, 계절적 market evolution을 포착하는 모델(PCA 분석을 통해 추정된 팩터들을 이용한 CAPM 이용)과 intraday specific volume dynamics를 포착하는 모델(ARIMA(1,1) 혹은 SETAR 모델 이용)이다. 이렇게 예측 모형을 이용하는 방법은 VWAP처럼 거래 시작시 모든 거래 스케쥴을 미리 알 수는 없지만, 예측을 기반으로 하여 시점마다 한 단계씩 주문 집행 수량을 업데이트 해 나간다는 특징이 있다.

![Figure 2](../assets/HFT_figure2.png)

### 2-2. Implementation Shortfall

Implementation Shortfall은 주문 집행 시작 시점의 가격을 벤치마크로 설정하며 사전에 정해진 risk aversion 계수에 맞춰 집행 기간 동안의 전반적인 잠재적 위험 조정 비용을 최소화 하고자 한다. 이러한 전략은 자신의 위험 회피 수준을 명확히 알며 미래의 기대 수익률에 확신이 있는 투자자에게 특히나 적합하다.

<div align="center">
    $$C(x)=X_0P_0-\sum v(t)\tilde{P}_t$$
</div>  

먼저 전체 거래 비용인 $C(x)$은 위의 식처럼 initial market value와 effective execution value의 차로 결정된다. 이때 각 거래에서 주문 집행 가격 $\tilde{P}_t$는 temporary market impact의 영향을 받는다. 이때 이러한 market impact는 집행 가격 $\tilde{P}_t$에만 영향을 미치며 시장 균형 가격(market equilibrium)에는 영향을 주지 않는다. 이러한 liqudity premium(일시적 market impact로 인한 비용)은 주문을 적은 수량으로 나누어 집행하며 낮출 수 있다. 그러나 permanent market impact는 주문 집행 전략과 상관없이 결과적으로 항상 같은 영향을 주게된다.

주문 집행 스케쥴 $x_t$는 (a)linear execution과 (b)deviation 으로 나뉠 수 있으며 이때 deviation은 근이 t=0, t=T인 2차 함수로 표현할 수 있다. 이를 식으로 표현하면 아래와 같다. 이때 함수의 계수인 $k$는 주문 집행의 속도, 즉 urgency를 의미한다.

<div align="center">
    $$x(t)=X_0\frac{T-t}{T}-kt(T-t)$$
    $$v(t)=\frac{1}{T}X_0 +k(T+1)-2kt$$
</div>

tilting factor인 $k$는 다음과 같이 계산되며 계산을 위해서는 몇가지 특징과 temporary market impact model에 대한 가정이 필요하다.

<div align="center">
    $$k=\frac{\lambda\alpha\sigma}{\eta}$$
</div>

- $\hat{\alpha}$: price change expectation  
- $\hat{\sigma}$: expected stock volatility 
- $\hat{\eta}$: expected temporary market impact cost  


아래의 그림은 $k$에 따른 주문 집행 스케쥴 차이를 보여준다.

![Figure 4](../assets/HFT_figure4.png)

## 3. High-frequency Trading

HFT는 그 자체로 하나의 전략인 것이 아니라 통신의 발전으로 가능해진 자동화 거래 기술이다. EUREX에 따르면 HFT들의 주요 전략은 유동성 공급(마켓 메이킹), 차익거래, 단기 모멘텀, 유동성 detection 등이 있다. 학자들이 명시하고자 한 HFT의 주요 특징은 다음과 같이 8가지로 정리할 수 있다.

**(1)** computer-based non-discretionary / automated strategies  
**(2)** proprietary trading  
**(3)** use of low-latency HFT tehcnologies  
**(4)** real-time tick-by-tick data processing    
**(5)** high amount of intrady messages   
**(6)** small margins per trade  
**(7)** high captial turnover    
**(8)** flat over-night positions  

이러한 short time-frames에서의 수익 실현 기회는 일시적이며 한정적이라는 것에 유의해야한다. 또한 liquidity-supply 전략의 경우 다른 전략과 경쟁으로 인한 손해(eg. adverse selection risk)를 보지 않기 위해서 빠르게 지정가 주문을 변경하는 것이 가능해야 한다.

### 3-1. Passive HFT

전통적으로 on-exhcange designated market maker(DMM) 혹은 specialists는 bid-ask 호가 내 잔량을 계속해서 제공하여 유동성을 공급하는 역할을 하였다. 이에 따라 시장 참여자들은 적은 거래 비용을 빠르게 거래를 할 수 있었다. 또한 market maker들은 round-trip(the buy and sell of the same quantity, without any price change)을 완료하면 bid-ask spread 만큼의 수익을 얻을 수 있다. 

NYSE 거래소에서는 monopolistic market maker를 지정하지만, NASDAQ과 같은 거래소에서는 multiple market maker를 허용해 경쟁이 이루어진다.
또한 NYSE Euronext’s Arca Options platform, NASDAQ’s NOM platform, BATS Global Markets options exchange와 같은 marker-taker 가격 모델을 취하는 거래소의 경우, market maker들은 거래소로부터 유동성을 공급한 것에 대한 리베이트를 지급 받아 수익을 얻기도 한다.

Harris(2002)에 따르면 vanilla market making 전략은 미래 시장 방향성에 대한 베팅이 아니므로 재고 위험에 노출될 수 있다. 재고위험을 줄이기 위해서는 bid-ask 양쪽의 주문을 관리하며 적절히 유지해야한다. 만약 target level과 inventory 사이에 격차가 생기면 market maker는 불균형을 없애기 위해 지정가 주문을 조정한다. vanilla market making의 또다른 위험은 time-horizon risk이다. 이는 잔여 trading time이 줄어들면서 함께 줄어들게 된다. 결국, 시장 정보에 대한 확신도에 따라 보유기간, 재고위험 관리 등이 달라지게 되는 것이다.

Aldridge(2013)는 inventory models과 information-based models 두가지로 automated market making model을 분류하였다. inventory models은 재고의 효과적인 관리에만 초점을 맞추며 대표적인 모델로는 Avellaneda, Stoikov(2008)이 제시한 Avellaneda-Stoikov 모델이 있다.

Avellaneda-Stoikov 모델은 mid-price가 일정한 변동성과 drift가 없는 표준 brownian motion을 따른다고 가정하며 reservation price는 아래의 식과 같이 현재 재고의 불균형으로 인한, mid-price에서의 편차로 결정된다고 하였다.

<div align="center">
    $$r_t(I_t)=m_t-\gamma I_t\sigma^2(T-t)$$
</div>

- $\gamma$: risk aversion coefficient  
- $\sigma^2$: mid-price variance
- $(T-t)$: remaining time

다음으로 optimal spread size를 reservation price를 기준으로 구한다. bid-ask 지정가 물량의 시장가 거래 확률은 Poissonian process를 따른다고 가정하며 이때 거래 확률(Poisson rate)은 mid-price와 호가 사이의 거리인 $\delta$로 결정된다($\lambda(\delta)$). 또한 호가창의 물량은 power law distribution을 따른다고 가정하며($f^Q(x) \sim x^{-1-\alpha}$, 여러 연구에 의하면 $\alpha\approx1.5$), 주문 집행량에 따른 가격의 변화는 logarthmic shape를 가진다고 가정한다($\Delta p \sim Kln(Q)$)

<div align="center">
    $$s_t =\gamma \sigma^2(T-t)+(2/\gamma)ln(1+\gamma/k)$$
</div>

 $s_t$의 최종 해는 midprice의 variance, 잔존시간(T-t), risk-aversion parameter, 시장환경(k)에 의해 결정되며 최적의 호가는 $r_t(I_t) \pm s_t/2$로 결정된다.

 Avellaneda-Stoikov 모델을 보다 간단하게 만든 모형이 있다. 여기서는 호가별 거래 확률이 더이상 $\delta$의 함수가 아니며 매분마다 변동하는 bid와 ask를 통해 직접 산출한다. 정확히는 위 Avellaneda-Stoikov 모델에서 다음의 식 내의 $\lambda$에 대한 $\delta$의 미분값을 1분 동안의 차이로 대체하였다고 볼 수 있다.

- Avellaneda-Stoikov 모델
 <div align="center">
    $$m_t-r^{b}(s, q, t)=\delta^{b}-\frac{1}{\gamma} \ln \left(1-\gamma \frac{\lambda^{b}\left(\delta^{b}\right)}{\frac{\partial \lambda^{b}}{\partial \delta}\left(\delta^{b}\right)}\right)$$
    $$r^{a}(s, q, t)-m_t=\delta^{a}-\frac{1}{\gamma} \ln \left(1-\gamma \frac{\lambda^{a}\left(\delta^{a}\right)}{\frac{\partial \lambda^{a}}{\partial \delta}\left(\delta^{a}\right)}\right)$$
</div>

- Simple model
 <div align="center">
    $$p_{t}^{b}=m_t-\delta^{b}=r_{t}^{b}-\frac{1}{\gamma} \ln \left(1-\gamma \frac{\lambda_{t}^{b}}{\lambda_{t}^{b}-\lambda_{t-1}^{b}}\right)$$
    $$p_{t}^{a}=m_t+\delta^{a}=r_{t}^{a}+\frac{1}{\gamma} \ln \left(1-\gamma \frac{\lambda_{t}^{a}}{\lambda_{t}^{a}-\lambda_{t-1}^{a}}\right)$$
</div>

두번째 market maing 전략은 다른 시장 참여자들이 가진 정보를 주문의 흐름, 호가창 등을 통해 알아내고자 한다. Glosten, Milgrom(1985)는 bayesian learning을 이용하여 거래 자산의 진정한 시장 가치($V$)에 대한 market maker의 prior beliefs를 새로운 정보(new information)로 업데이트 하고자 하였다. 

<div align="center">
    $$Pr(h|D)=\frac{Pr(h)Pr(D|h)}{Pr(D)}$$
</div>


$Pr(h\vert D)$: the conditional posterior belief  
$Pr(h)$: the prior belief  
$Pr(D\vert h)$: the degree to which the hypothesis predicts the observed data
$Pr(D)$: the prior probability of the data

market maker는 두가지의 상호 배타적인 가설을 세운다. 하나는 true market value(V)가 mid price와 deviate한 경우이고($h:V \neq m$) 다른 하나는 두개가 같은 경우이다($\bar{h}:V=m$). 데이터의 marginal likelihood는 total probability 정리를 이용하여 계산할 수 있다.

<div align="center">
    $$Pr(D) = Pr(D|h)Pr(h)+Pr(D|h')Pr(h')$$
</div>

만약 market maker가 informed trader와 uninformed trader를 구분할 수 없다면 $Pr(informed) = 0.5$이 되고, true value가 deviate 하다는 가정($h:V \neq m$) 하에서 시장 주문을 받을 확률은 $Pr(D\vert h)=\alpha + 0.5(1-\alpha)=0.75$가 된다. 만약 market maker가 uninformed 하다면 $Pr(h)=Pr(\bar{h})=0.5$이 된다. 마지막으로 adverse trading risk와 완전경쟁시장을 가정한다면 optimal bid-ask quote은 각각 두 가지 가능한 시장 이벤트(buy, sell)를 가정할 때의 true market value에 대한 기대치로 계산된다.

<div align="center">
    $$P_b=E[V|sell];P_a=E[V|buy]$$
</div>

Das는 위 식의 $V$를 online 확률 추정 방법을 통해 구함으로써 위 문제의 해답을 구하는 시도를 하였다. 이산적인 분포인 $Pr(V=x)$는 vector로 표현되는데 처음에는 normal pdf를 따르다가 계속해서 비모수 분포에 기반한 베이지안 업데이트를 거친다. 분포를 업데이트 함에 있어 가능한 시장 이벤트에는 총 세가지가 있으며 이는 buy, sell, no order이다. buy의 경우를 예시로 식을 표현하면 아래와 같다. 이때 $Pr(buy)$은 모든 $x$에 대하여 같다.

<div align="center">
    $$Pr(V=x \vert buy)=\frac{Pr(V=x)Pr(buy \vert V=x)}{Pr(buy)}$$
</div>
- where $Pr(V=x)$ is the prior denstiy estimate

이전의 Glosten, Milgrom 모델에 따르면 market maker의 optimal quote은 특정 주문을 받았을 때의 조건부 기댓값으로 나타낼 수 있다. 이 식은 먼저 기댓값의 정의에 따라 다음과 같이 정리되고

<div align="center">
    $$P_{a}=\sum_{x_{i}=V_{\min }}^{V_{\max }} x_{i} \operatorname{Pr}\left(V=x_{i} \mid \text { buy }\right)$$
</div>

위 식은 다시 Das 모델에서의 식을 대입하면 다음과 같이 정리된다. 

<div align="center">
    $$\sum_{x_{i}=V_{\min }}^{V_{\max }} x_{i} \operatorname{Pr}\left(V=x_{i} \mid \text { buy }\right)=\frac{\sum_{x_{i}=V_{\min }}^{V_{\max }} x_{i} \operatorname{Pr}\left(V=x_{i}\right) \operatorname{Pr}\left(\text { buy } \mid V=x_{i}\right)}{\operatorname{Pr}(\text { buy })}$$
</div>

이러한 호가는 완전경쟁시장(zero-profit)에서만 해당하며 market maker가 adverse selection risk를 커버하기 위해 지정하고자 하는 최대 매수 가격 혹은 최소 매도 가격을 의미한다.


Das(2005)는 또한 일정 수준으로 bid-ask 손익 분기 스프레드 주위의 스프레드를 넓힘으로써 수익을 창출하는 전략을 제안하였다. market maker들은 왕복이 거의 없는 큰 spread와 왕복이 잦은 소형 스프레드 사이에서의 trade-off에 직면한다. 이 trade-off의 solution은 시장 내 정보 함량의 정도와 경쟁의 수준에 따라 결정된다. 다스는 경쟁이 심할 때에는 양의 수익을 가져다 주는 한, 현재의 spread 안에서 거래하는 것이 좋다고 제안하였다.


buy order의 사전 확률은 다음과 같다.
<div align="center">
    $$\operatorname{Pr}(\text { buy })=\sum_{x_{i}=V_{\min }}^{V_{\max }} \operatorname{Pr}\left(V=x_{i}\right) \operatorname{Pr}\left(\text { buy } \mid V=x_{i}\right)$$
</div>


위의 확률은 거래자 모집단에 가정에 따라 달라지게 된다. informed trader에 대항하여 거래하는 market maker들은 noisy signal($W=V+N(0,\sigma_W$)을 통해서만 true value를 알 수 있다. 즉, noisy informed trader은 $W > P_a$ 이면 매수하고, $W < P_b$ 이면 매도, 그 사이에 있으면 아무 포지션도 취하지 않는다. 이를 조건부 확률로 표시하면 다음과 같다.

<div align="center">
    $$Pr(buy \vert V=x) = Pr(W > P_a) = Pr(N(0, \sigma_W) > P_a - x)$$
</div>

최적의 bid ask quote은 위 식을 앞서 구한 아래의 식에 대입하여 찾은 solution과 같다. 


<div align="center">
    $$P_{a}=\frac{\sum_{x_{i}=V_{\min }}^{V_{\max }} x_{i} \operatorname{Pr}\left(V=x_{i}\right) \operatorname{Pr}\left(\text { buy } \mid V=x_{i}\right)}{\operatorname{Pr}(\text { buy })}=\frac{\sum_{x_{i}=V_{\min }}^{V_{\max }} x_{i} \operatorname{Pr}\left(V=x_{i}\right) \operatorname{Pr}\left(\text { buy } \mid V=x_{i}\right)}{\operatorname{Pr}(N(0, \sigma_W) > P_a - x)}$$
</div>

market making 모델에는 이 외에도 Lin(2006)이 제안한 discrete Kalman filter를 이용한 true value belief updating과 Chan, Shelton(2001)이 연구한 Reinforcement Learning을 이용한 방법이 있다.

### 3.2-Active HFT
Active HFT는 passive HFT와는 달리 liquidity taker의 역할을 한다. active HFT에는 두가지 노선이 있다. 첫번째로는 초단기 모멘텀이고 두번째로는 large order을 추적하여 앞서 거래를 진행하는 front run이다. 첫번째의 경우에는 HFT의 기술 등장 전부터 있어왔지만 대부분 속도차이 때문에 HFT에 의해 대체되었다. 두번째는 legal version과 illegal version이 있으며 legal version의 경우 제3자의 정보에 기인하여 빠르게 체결하는 방식이고 illegal version의 가짜 주문, market manipulation 등을 이용한 방식이다.

## 4. Conclusion
이 post에서는 Mandes, Alexandru (2016)의 Algorithmic and high-frequency trading strategies: A literature review를 정리해 보았다.

앞에서 논의한 전략들은 시장 참여자와 기술 유형에 따라 분류된다. Infomred trader는 거래 비용을 최소화 하기 위해 alogirthmic trading 전략을 이용하였으며 정보 유출을 최소화하기 위해 smart order routing 기술을 사용한다. 대표적으로 optmial execution 문제에서는 VWAP와 Implementation Shortfall을 살펴보았다. uninformed trader 또는 market maker의 경우 낮은 지연율에서 오는 이점을 얻기 위해 high-frequenct trading 전략을 마련하였다. HFT의 경우 inventory-based와 information-based로 나뉘어진 분야에서 대표적인 모델들을 살펴보았다.