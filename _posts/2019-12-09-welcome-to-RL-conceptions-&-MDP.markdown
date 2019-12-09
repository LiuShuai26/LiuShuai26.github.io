---
layout: post
title:  "RL conceptions & MDP"
date:   2019-12-09 00:08:45 +0800
categories: RL
---
# RL basic conceptions and Finite Markov Decision Processes

------

### Rewards

- A **reward** $R_t$ is a scalar feedback signal
- Indicates how well agent is doing at step $t$
- The agent's job is to maximize cumulative reward

Reinforcement learning is based on the **reward hypothesis**

> **Definition (Reward Hypothesis)**
>
> *All* goals can be described by the maximization of expected cumulative reward.

------

### History and state

The **history** is the sequence of observations, actions, rewards
$$
H_t = O_1, R_1, A_1, ..., A_{t-1}, O_t, R_t
$$
**State** is the information used to determine what happens next.

state is a function of the history:
$$
S_t = f(H_t)
$$

------

### Information State

An **information state** (a.k.a **Markov state**) contains all useful information from the history.

> **Definition**
>
> A state $S_t$ is **Markov** if and only if
> $$
> \Bbb P[S_{t+1}|S_t] = \Bbb P[S_{t+1}|S_1, ..., S_t]
> $$

- The future is independent of the past given the present
  $$
  H_{1:t} \rightarrow S_t \rightarrow H_{t+1:\infin}
  $$

- Once the state is known, the history may be thrown away

- i.e. The state is a sufficient statistic of the future

- The environment state $S_t^e$ is Markov

- The history $H_t$ is Markov

------

**Full observability:** agent directly observes environment state
$$
O_t = S_t^a = S_t^e
$$

- Agent state = environment state = information state
- Formally, this is a **Markov decision process** (MDP)

**Partial observability:** agent indirectly observes environment.

- Now agent state $\not=$ environment state
- Formally this is a **partially observable Markov decision process** (POMDP)

------

An RL agent may include one or more of these components:

- Policy: agent's behavior function
- Value function: how good is each state and/or action
- Model: agent's representation of the environment

**Policy**

- A **policy** is the agent's behavior
- It is a map from state to action, e.g.
- Deterministic policy: $a = \pi(s)$
- Stochastic policy: $\pi(a|s) = \Bbb P[A_t=a|S_t=s]$

**Value Function**

- Value function is a prediction of future reward

- Used to evaluate the goodness/badness of states

- And therefore to select between actions, e.g.
  $$
  V_\pi(s) = \Bbb E_\pi[R_{t+1} + \gamma R_{t+2} + \gamma^2R_{t+3} + ...|S_t=s]
  $$

**Model**

- A **model** predicts what the environment will do next

- $P$ predicts the next state

- $R$ predicts the next (immediate) reward, e.g.
  $$
  P_{ss'}^a = \Bbb P[S_{t+1}=s'|S_t=s, A_t=a] \\
  R_s^a = \Bbb E[R_{t+1}|S_t=s, A_t = a]
  $$

------

Value Based

- No Policy (Implicit)
- Value Function

Policy Based 

- Policy
- No Value Function

Actor Critic

- Policy
- Value Function

Model Free

- Policy and/or Value Function
- No Model

Model Based

- Policy and/or Value Function
- Model

------

Exploration and Exploitaion

- Exploration ﬁnds more information about the environment
- Exploitation exploits known information to maximize reward
- It is usually important to explore as well as exploit

------

------

*Markov decision processes* formally describe an environment for reinforcement learning

Where the environment is *fully observable*

**Markov Process**

A Markov process is a memoryless random process, i.e. a sequence of random states $S_1, S_2, ...$ with the Markov property.

> **Definition**
>
> A *Markov Process* (or *Markov Chain*) is a tuple $<S, P>$
>
> - $S$ is a (finite) set of states
> - $P$ is a state transition probability matrix,
>
> $$
> P_{ss'} = \Bbb P[S_{t+1} = s'|S_t=s]
> $$
>
> **Definition**
>
> A *Markov Reward Process* (or *Markov Chain*) is a tuple $<S, P, R, \gamma>$
>
> - $S$ is a (finite) set of states
> - $P$ is a state transition probability matrix,
>
> $$
> P_{ss'} = \Bbb P[S_{t+1} = s'|S_t=s]
> $$
>
> - $R$ is a reward function, $R_s = \Bbb E[R_{t+1}|S_t=s]$
> - $\gamma$ is a discount factor, $\gamma \in [0, 1]$
>
> **Definition**
>
> The *return* $G_t$ is the total discounted reward from time-step $t$.
> $$
> G_t = R_{t+1} + \gamma R_{t+2} +\ ...=\underset {k=0} {\overset \infin \sum} \gamma^kR_{t+k+1}
> $$
>
> - The *discount* $\gamma \in [0, 1]$ is the present value of future rewards
> - The value of receiving reward R after k+1 time-steps is $\gamma^kR$
> - This values immediate reward above delayed reward.
> - $\gamma$ close to 0 leads to "myopic" evaluation
> - $\gamma$ close to 1 leads to "far-sighted" evaluation
>
> The value function $v(s)$ gives the long-term value of state $s$
>
> **Definition**
>
> The *state value function* v(s) of an MRP is the expected return starting from state $s$
> $$
> v(s) = \Bbb E[G_t|S_t=s]
> $$

**Bellman Equation for MRPs**

The value function can be decomposed into two parts:

- immediate reward $R_{t+1}$
- discounted value of successor state $\gamma v(S_{t+1})$

$$
v(s) = \Bbb E[G_t|S_t=s] = \Bbb E[R_{t+1}+\gamma v(S_{t+1})|S_t=s]
$$

------

**Markov Decision Process**

A Markov decision process (MDP) is a Markov reward process with decisions. It is an *environment* in which all states are Markov.

> **Definition**
>
> A *Markov Decision Process* is a tuple $<S, A, P, R, \gamma>$
>
> - $S$ is a finite set of states
> - A is a finite set of actions
> - P is a state transition probability matrix,
>
> $P_{ss'}^a=\Bbb P[S_{t+1}=s'|S_t=s, A_t=a]$
>
> - R is a reward function, $R_s^a = \Bbb E[R_{t+1}|S_t=s, A_t=a]$
> - $\gamma$ is a discount factor $\gamma \in [0, 1]$

**Policies**

> **Definition**
>
> A *policy* $\pi$ is a distribution over actions given states,
> $$
> \pi(a|s) = \Bbb P[A_t=a|S_t=s]
> $$
>
> - A policy fully defines the behavior of an agent
> - MDP policies depend on the current state (not the history)

**Value Function**

> **Definition**
>
> The *state-value* function $v_\pi(s)$ of an MDP is the expected return starting from state s, and then following policy $\pi$
> $$
> v_\pi(s) = \Bbb E_\pi[G_t|S_t=s]
> $$
> The *action-value* function $q_\pi(s, a)$ is the expected return starting from state s, taking action a, and then following policy $\pi$
> $$
> q_\pi(s, a)=\Bbb E_\pi[G_t|S_t=s, A_t=a]
> $$

------

Bellman Expectation Equation

The state-value function can again be decomposed into immediate reward plus discounted value of successor state,
$$
v_\pi(s) = \Bbb E_\pi[R_{t+1}+\gamma v_\pi(S_{t+1})|S_t=s]
$$
The action-value function can similarly be decomposed,
$$
q_\pi(s, a)=\Bbb E_\pi[R_{t+1}+\gamma q_\pi(S_{t+1}, A_{t+1})|S_t=s, A_t=a]
$$

------

### Formula derivation:

Expected discounted return:
$$
\begin{align}
G_t & \dot= R_{t+1} + \gamma R_{t+2} + \gamma^2R_{t+3} + \gamma^3R_{t+4}+ \dots \\
& = R_{t+1} + \gamma (R_{t+2} + \gamma R_{t+3} + \gamma^2R_{t+4}+ \dots) \\
& = R_{t+1} + \gamma G_{t+1}
\end{align}
$$
where $\gamma$ is a parameter, $0 \le \gamma \le 1$, called the discount rate.

Bellman equation for $v_\pi$
$$
\begin{align}
v_\pi(s) &\dot=\Bbb E_\pi[G_t|S_t=s]\\
&= \Bbb E_\pi[R_{t+1}+\gamma G_{t+1}|S_t=s]\\
&= \underset a \sum\pi(a|s)\underset {s'} \sum\underset r \sum p(s', r|s,a)[r+\gamma \Bbb E_\pi[G_{t+1}|S_{t+1}=s']]\\
&=\underset a \sum \pi(a|s) \underset {s',r} \sum p(s', r|s,a)[r+\gamma v_\pi(s')],\ for\ all\ s \in \cal S
\end{align}
$$

------

We denote all the *optimal policies* by $\pi_*$.

$\pi \ge \pi'$ if and only if $v_\pi(s) \ge v_{\pi'}(s)$ for all $s \in \cal S$

*optimal state-value function*

$v_*(s) \dot= \underset \pi \max v_\pi(s)$, for all $s \in \cal S$

*optimal action-value function*, denoted $q_*$:

$q_*(s, a) \dot=\underset \pi \max q_\pi(s, a)$, for all $s \in \cal S$ and $a \in \cal A(s)$.

$q_*$ in terms of $v_*$:

$q_*(s, a) = \Bbb E[R_{t+1} + \gamma v_*(S_{t+1}) | S_t=s, A_t=a]$.

------

Bellman optimality equation for $v_*$
$$
\begin{align}
v_\pi(s) &= \underset {a \in \cal A(s)} \max q_{\pi_*}(s, a)\\
&= \underset a \max \Bbb E_{\pi_*}[G_t|S_t=s, A_t=a]\\
&= \underset a \max \Bbb E_\pi[R_{t+1}+\gamma G_{t+1}|S_t=s, A_t=a]\\
&= \underset a \max \Bbb E[R_{t+1} + \gamma v_*(S_{t+1}) | S_t=s, A_t=a] \\
&=\underset a \max\underset {s',r} \sum p(s', r|s,a)[r+\gamma v_\pi(s')]
\end{align}
$$
Bellman optimality equation for $q_*$
$$
q_*(s, a) = \Bbb E[R_{t+1} + \gamma \underset {a'} \max q_*(S_{t+1}, a') | S_t=s, A_t=a]\\
= \underset {s', r} \sum p(s', r|s, a)[r+ \gamma \underset {a'} \max q_*(s', a')].
$$