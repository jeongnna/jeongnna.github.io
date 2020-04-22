---
layout: post
title: "확률변수열의 수렴 - p수렴과 as수렴"
date: 2018-9-29
use_math: true
---
수열의 수렴과 같이 확률변수열에 대해서도 수렴성을 이야기할 수 있다. 일반적인 수열의 수렴과 달리, 확률변수열의 수렴은 여러 가지 종류로 나뉜다. 여기에서는 **p수렴**(convergence in probability)과 **as수렴**(almost sure convergence)의 차이를 이해하는 데 중점을 둘 것이다.

### 수열의 수렴

수열 $\{a_n\}$이 $a$로 수렴한다는 것을 일반적으로

$$
\underset{n \to \infty}{\lim} a_n = a
$$

와 같이 나타내며, 이를 풀어서 쓰면 다음과 같다.

$$
^\forall \epsilon > 0, \quad ^\exists N \in \mathbb{N} \quad such\; that
$$

$$
n \geq N \Rightarrow |a_n - a| < \epsilon
$$

쉽게 말해서, $\epsilon$를 아무리 작게 잡더라도 $n$이 충분히 크다면 반드시 $\|a_n - a\| < \epsilon$를 만족한다는 뜻이다.

### p수렴(convergence in probability)

확률변수열 $\{X_n\}$에 대하여, p수렴은

$$
X_n \overset{p}{\to} X
$$

로 표기하며, $\lim$ 기호를 사용하면

$$
^\forall \epsilon > 0, \quad \underset{n \to \infty}{\lim} \Pr (\|X_n - X\| \geq \epsilon) = 0
$$

와 같이 정의된다. $\Pr (\|X_n - X\| \geq \epsilon)$는 $X_n$과 $X$가 멀리 떨어져 있을 확률을 의미한다. 이것이 0으로 수렴한다는 것은, $n$이 커지면 커질수록 *확률적으로* $X_n$과 $X$가 거의 같아진다는 의미이다.

여기에서 $a_n \equiv \Pr (\|X_n - X\| > \epsilon)$ 라고 두면, 수열 $\{a_n\}$이 0으로 수렴하는 것으로 봐도 무방하다. 따라서 p수렴의 정의를 다시 쓰면 다음과 같다.

$$
^\forall \epsilon > 0, \quad ^\forall \delta > 0, \quad ^\exists N \in \mathbb{N} \quad such\; that
$$

$$
n \geq N \Rightarrow \Pr (\|X_n - X\| > \epsilon) < \delta
$$

일반적인 수렴과 확률수렴의 차이점은, 확률수렴의 경우에는 반드시 $\|X_n - X\| < \epsilon$가 되는 것은 아니다. 예를 들어, 대수의 법칙(Law of Large Number)은 표본평균이 모평균으로 확률적으로 수렴한다는 법칙이다. 이를 수식으로 쓰면 다음과 같다.

$$
\overline{X} \overset{p}{\to} \mu
$$

$$
i.e. \quad ^\forall \epsilon > 0, \quad \underset{n \to \infty}{\lim} \Pr (\|\overline{X} - \mu\| \geq \epsilon) = 0
$$

즉, 표본이 아주 커지면 확률적으로 표본평균이 모평균에 가까워진다는 지극히 상식적인 얘기이다. 그런데 표본평균이 ++반드시++ 모평균에 수렴한다고 말할 수 있을까? 만약 운이 정말로 나쁘다면 표본을 아무리 많이 뽑더라도 모평균에 가깝지 않게 될 수도 있다. 이 것이 일반적인 수렴과 확률수렴의 차이이다.

### as수렴(almost sure convergence)

as수렴은 그야말로 *거의 확실한* 수렴이라는 뜻이며, p수렴보다 더 강한 수렴이다(as수렴하면 반드시 p수렴한다). as수렴은 

$$
X_n \overset{a.s.}{\to} X
$$

로 표기하며, 정의를 가장 간단하게 나타내면 다음과 같다.

$$
\Pr (\underset{n \to \infty}{\lim} X_n = X) = 1
$$

이 정의를 있는 그대로 읽으면 "$X_n$이 수렴할 확률이 1"이다. 처음엔 고개가 끄덕여질 수 있지만 조금 더 생각해보면 어딘가 잘 와닿지 않는 정의이다. 확률이 수렴하는 것과 수렴할 확률이 1인 것은 무엇이 다른가? $X_n$이 수렴하는 사건, 다시 말해 $\{\underset{n \to \infty}{\lim} X_n = X\}$ 이라는 사건은 어떻게 정의되는가?

이를 이해하기 위해서는 확률변수를 표본공간에서의 함수로 생각해야 한다. 저번 글에서 확률변수열 $\{X_n\}$은 표본공간의 원소 $w \in \Omega$에 의해 유일하게 결정되는 함수열 $(X_1 (w),\; X_2 (w),\; X_3 (w),\; \cdots)$이라고 설명했었다. $w \in \Omega$가 주어져 있으면 $\{X_n (w)\}$는 더 이상 랜덤 요소가 없으므로 일반적인 수열로 취급할 수 있다. 그러면 $X_n$이 $X$로 수렴하는 사건은, 표본공간으로부터 $\underset{n \to \infty}{\lim} X_n (w) = X(w)$를 만족하는 $w$가 관측되는 사건이라고 이해할 수 있다. 즉, as수렴은

$$
\Pr (\{w \in \Omega :\; \underset{n \to \infty}{\lim} X_n (w) = X(w)\}) = 1
$$

와 같이 쓸 수 있다.
