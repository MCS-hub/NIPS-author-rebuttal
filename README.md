# NIPS2022-author-rebuttal

## Reviewer qvSP

Thank you very much for your positive and constructive comments.

**Q1.  It looks the nonconvex structure is special composed of two convex objectives. Acutally, the convex-splitting methods have been used in numerical analysis for a long time. Please refer to:...**

**A1.** We have read the two mentioned articles and found that the nature of the two articles is quite different from ours. These articles studied numerical schemes for solving PDEs, where the strategies include the well-known finite difference and finite element methods. In contrast, our work does not study how to solve PDEs via numerical analysis. Rather, PDEs have been used as a tool for smoothing out the loss surface of a neural network. The PDEs used in our work are Hamilton-Jacobi and its viscous version, which admit closed-form solution (no need to solve them by numerical schemes). These solutions play a role as the smooth versions of the original loss surface. We then spot the DC structure of the solution, hence our proposed MCSDCA can be applied to minimize the solution (minimize the solution w.r.t. to the space variable $x$ at a fixed time $t$). Therefore, we think that, although the two mentioned articles and ours have a common point which is the study on PDEs, the link between them is very weak.

**Q2. The theoritical analysis have not been linked with the recent related paper about theory of SGD.**

**A2.** The paper [Shi] seems not to be much relevant to our work; Meanwhile, the paper [Cheng] has a special link to our work. Firstly, for the paper [Shi], it is essentially a study on the theoretical aspect of the stepsize of Stochastic gradient descent. For example, in practice, the staircase schedule is usually employed to anneal the stepsize, but very little is known about this from the theoretical point of view. Motivated by that, the authors tried to shed some light on the theoretical properties of the stepsize. The main tool being used is the continuous form of SGD which is given by a stochastic differential equation (SDE). This continuous form is analyzed, for example, its convergence to the optimal solution. Then, the authors tried to figure out which convergence properties of this continuous form can be carried out to its discrete form, which is the SGD. Although this work and ours both have a common point of considering some kind of SDE, the purposes are totally different and the link is weak and vague. 

The paper [Cheng] introduces a novel discretization scheme for the underdamped Langevin diffusion. The link between the paper and our work is clear. In our work, for the deep learning application, we use the vanilla Lagenvin diffusion (aka. overdamped Langevin diffusion) with Euler-Maruyama discretization at each step to construct a Markov chain. The paper [Cheng] offers us an alternative: the Markov chain can be constructed based on the underdamped Langevin diffusion with the novel discretization scheme in [Cheng]. Hence, the scheme in [Cheng] can be used as a part of our proposed MCSDCA. In fact, we have already researched on this combination for the past few months and obtained some promising results. We are happy to provide more information if the reviewer is interested.

**Q3. How about your new algoirhtms comparing with SGD with momentum.**

**A3.**  We have additionally run the SGD with momentum as another baseline algorithm. Please see our updated paper.

**Q4. Limitations: The nonconvex structure -- combined of two convex objectives, is so spectial that it rarely apply it in practice.**

**A4.** In fact, the difference-of-two-convex functions structure (yielding a nonconvex structure known as DC) is very well-known in the optimization community. It is widely acknowledged that most real-world nonconvex optimization problems have some DC structure. The class of DC functions forms a very large class of nonconvex programming (e.g., it contains all L-smooth functions, all weakly convex functions, it can uniformly approximate any continuous function on a compact set). Please refer to the following works [1,2,3,4]  and references therein for the pervasiveness of the DC structures in optimization, machine learning, and statistics.

*References.*

[1] Le Thi, H.A. and Pham Dinh, T. DC programming and DCA: thirty years of developments. Math. Program., Special Issue dedicated to : DC Programming - Theory, Algorithms and Applications, 169(1):5–68, 2018.

[2] Nouiehed, M., Pang, J. S., \& Razaviyayn, M. (2019). On the pervasiveness of difference-convexity in optimization and statistics. Mathematical Programming, 174(1), 195-222.

[3] Xu, Y., Qi, Q., Lin, Q., Jin, R., \& Yang, T. (2019). Stochastic optimization for DC functions and non-smooth non-convex regularizers with non-asymptotic convergence. In ICML (pp. 6942-6951). PMLR.

[4] Nitanda, A., \& Suzuki, T. (2017). Stochastic difference of convex algorithm and its application to training deep boltzmann machines. In Artificial intelligence and statistics (pp. 470-478). PMLR.


##Reviewer 9Q1s

We are grateful to the reviewer for the time to read our paper and the constructive comments.

**Q1. The theoretical results are kind of expected and lack interpretation. Address the gap between global optimal and local optimal.**

**A1.** We work with nonconvex, nonsmooth DC programs, and the proposed algorithm is guaranteed to converge to DC critical points in both asymptotic and non-asymptotic senses. For a DC function $f = g-h$, where $g,h$ are proper, convex, lower semicontinuous, the **global minimizer** of $f$ is characterized as follows (see, e.g, Theorem 1 in the paper [1]): $x^* $ is the **global minimizer** of $f$ if and only if $\partial_{\epsilon} h(x^*) \subset \partial_{\epsilon} g(x^* ), \quad \forall \epsilon \geq 0,$ where $\partial_{\epsilon}$ is the $\epsilon$-subdifferential. The $\epsilon$-subdifferential is a relaxation of the standard subdifferential and becomes the standard differential when  $\epsilon = 0$. 

[1] Pham Dinh, T., \& Le Thi, H.A. (1997). Convex analysis approach to DC programming: theory, algorithms and applications. Acta mathematica vietnamica, 22(1), 289-355.

It is nice that - for DC programming - globality can be characterized in terms of $\epsilon$-subdifferentials. However, in practice, the condition $\partial_{\epsilon} h(x^* ) \subset \partial_{\epsilon} g(x^* )$ for all $\epsilon \geq 0$ is impossible to achieve as it requires the inclusion to hold for all $\epsilon\geq 0$. A natural relaxation is to require the above inclusion to hold for $\epsilon = 0$ only, giving rise to the notion of "strong DC criticality": A point $x^* $ is called strongly DC critical for $f=g-h$ if $\emptyset \neq \partial h(x^* ) \subset \partial g(x^* ).$ To our knowledge, throughout the history of DC programming, this requirement is also very hard to develop corresponding iterative algorithms. Therefore, the notion of "DC criticality" is a further relaxation of strong DC criticality: a point $x^* $ is called a DC critical point of $f=g-h$ if $0 \in \partial g(x^*)-\partial h(x^*)$, or equivalently, $\emptyset \neq \partial g(x^* ) \cap \partial h(x^* ).$ When $h$ is differentiable at $x^* $, DC criticality becomes strong DC criticality. As a convex function is differentiable almost everywhere, a DC critical point is very likely to be strongly DC critical.

Now, the connection between DC criticality and globality is clear.  Our proposed MCSDCA is guaranteed to locate such DC critical points. We plan to include this discussion in the revision.

**Q2. Theorem 1 is hard to understand. The implication of each equation?**

**A2.** First, by definition, $x^* $ is a DC critical point of $f=g-h$ if $0 \in \partial g(x^* ) \cap \partial h(x^* )$, equivalently, $\text{dist}(\partial h(x^* ),g(x^* )) = 0.$ Therefore, the distance between two subdifferential sets $\partial g(x)$ and $\partial h(x)$ measures how good a candidate point, $x$, is a DC critical point. Theorem 1 reads: 

1) The distance - in expectation - between the two subdifferentials at the solution founded by MCSDCA is close to 0 when $T$  is large ($T$: the number of iterations).

2) The solution found by MCSDCA is very close to a point, where the distance - in expectation - between the two subdifferentials at that point is close to 0 when $T$  is large.


**Q3. Why the estimator is biased?**

**A3.** The estimator is biased since it is constructed using samples from a Markov chain rather than i.i.d. samples as in the classical settings. Starting from a given fixed initial state, the distribution of each element of the Markov chain is **different from** (but converges to) the target distribution.  Since the distribution of each element is not the same as the target distribution, the estimator is biased.

**Q4. In Algorithm 1, how to determine/choose the values of $\gamma_k$, $b$, and $n_k$?**

**A4.** According to Theorem 1, $n_k$ and $\gamma_k$ should be chosen as two increasing sequences of rates $O(k^{\lambda})$ and $O(k^{\beta})$ with $\lambda>0$ and $\beta \in [0,1)$, respectively. About the burning parameter $b$, it is chosen empirically. When $b=0$, there is no burn-in period. We have some insight from the experiment on testing $b$ for training deep neural networks via PDEs regularization (see supplemental material): Discarding more samples improves the training convergence rate, but is also more vulnerable to overfitting.

**Q5. What are the usage of Hamilton-Jacobi equation and its "viscous" version in deep learning?**

**A5.** The loss surface of a deep neural network is very rugged. The H-J and its viscous version are used to smooth out the loss while preserving its main characteristics. For example, by setting the initial condition of the viscous H-J to be the original loss, $u(x,0) = f(x)$, as time goes by, the obtained solution $u(x,t)$ tends to be smoother and smoother, where $t$ controls the smoothness. By choosing a fixed $t$, one now minimize $u(x,t)$ instead of $f(x)$ which is the initial condition of the PDEs system.

##Reviewer i9Qz

The authors are thankful for the constructive comments from the reviewers.

**Q1. It is unclear how the term $-<x,y^k>+ \frac{\gamma_k}{2} |x-x^k|^2_A$ is derived.**

**A1.** We minimize $F(x)= G(x)-H(x)$ where $G, H$ are convex. At iteration $k$, $y^k$ is a stochastic gradient of $H$ at $x^k$ (constructed based on Markov samples). Let $z^k$ be an exact gradient of $H$ at $x^k$, $x \mapsto H(x^k) + \langle z^k, x - x^k \rangle $ is an affine minorant of $H$ at $x^k$ (the line touching the graph $H$ at $x^k$ from below). So, $G(x)- (H(x^k) + \langle z^k, x - x^k \rangle)$ is a convex function touching the graph of $G-H$ at $x^k$ from above. The standard DCA minimizes this convex function to obtain the next point $x^{k+1}$. This guarantees $F(x^{k+1}) \leq F(x^{k})$. In our setting, as $z^k$ is unavailable, we replace $z^k$ by its stochastic counterpart $y^k$. Moreover, we add the proximal term $\frac{\gamma_k}{2} |x-x^k|^2_A$ to keep the new solution in the proximity of the old solution.

**Q2. Is ULA geometrically ergodic?**

**A2.** There are two points: (1) The Langevin diffusion is geometrically ergodic to the target distribution $\pi$. However, to be implementable, the Langevin diffusion must be discretized, giving rise to the ULA. There is discretization error making the target distribution of the ULA deviates a little bit from the original target. Let $\pi_{\eta}$ be a target distribution of the ULA, where $\eta$ is the discretization stepsize. When $\eta$ is small, the gap between $\pi$ and $\pi_{\eta}$ is small, and one can also bound this gap. Second, for the algorithm to work in big data, the full gradient in the ULA is usually replaced by the gradient computed on mini-batch data, resulting in a mini-batch noise.

We have been thinking about bridging the two mentioned gaps. First, even when the discretization stepsize $\eta$ is small, there is still a small discrepancy between $\pi$ and $\pi_{\eta}.$ To fix this gap one can use a sequence of diminishing stepsize $\{\gamma_k\}$ instead. However, the Markov chain is no longer time-homogeneous. Our previous analysis assumes the used Markov chain is time-homogeneous, so we only use a unique constant stepsize $\gamma$. In an effort to address diminishing stepsize, in the revised version, we have additionally proved that: Theorem 1 and 2 are still valid for **time-inhomogenous** Markov chains (Appendix B in the revised version).

**Q3. Can ULA be replaced by SGLD?**

**A3.** As in answer A2, for large-scale datasets, we use the practical version of ULA based on mini-batch data, which is exactly the SGLD as you said.

**Q4. The construction of functions G(x) and H(x) are non-natural.**

**A4.**  Due to the space limit, we do not present how to derive this construction in detail. In fact, the construction is very natural and the idea is interesting. Let us explain in plain language. The function $\tilde{u}(x,t) :=-\epsilon \log \int_{\mathbb{R}^n}{\exp \left( \frac{1}{\epsilon}\left[-f(x')-\frac{1}{2t}\langle x'-x,\mathcal{H}(x'-x)\rangle \right] \right) dx'}$ is nonconvex w.r.t. $x$ and we want to decompose it as a DC function (fixed $t$). The structure $\log\int\exp$ is nice, and it can be viewed as the continuous form of a more familiar structure $\log\Sigma\exp$. And we know that the $\log \Sigma_i \exp (\phi_i(x))$ is a convex if $\phi_i(x)$ are affine. In our case, the $\phi_i(x)$ is replaced by the continuous form $\phi(x,x')$, and $\phi(x,x')$ is quadratic. A natural idea is to split $\phi(x,x')$ into the quadratic part and the affine part, resulting in the construction of $G$ and $H$.

**Q5. Sampling from p(x'|x) is essentially to simulate of the posterior distribution of the deep neural network at a low temperature $\epsilon$. This can be extremely hard. It is unclear whether the algorithm is practical.**

**A5.** Sampling from $p(x'\vert x)$ involves a simulation of the posterior distribution of a neural network. It can be very expensive to obtain i.i.d. samples from such a posterior. This is why we need Markov chain Monte-Carlo in such a case. The cost to obtain one element of the Markov chain is a forward-backwards pass through a neural network over mini-batch data, which is practical.

**Q6. More numerical experiments**

**A6.** We have added SGD-momentum as a baseline and conducted new experiments on the Langevin diminishing stepsize.

**Q7. The algorithm can be inefficient for large number of iterations.** 

**A7.** We think it is not sufficient to conclude this increase is not efficient. E.g, in our deep learning application, the algorithm progresses inside each Markov chain, not only after the chain is finished: when one element from the chain is obtained, the algorithm takes a step (the update step after the chain is finished can be considered as an aggregation of small updates inside the chain). 

**conflict with the Monte Carlo nature of the algorithm:** We do not see any conflict here, could you please explain more, thank you.

##Reviewer aXhd 

The authors are grateful to the reviewer for constructive and thoughtful comments.

**Q1. It may result in significant loss of the sampling efficiency.**

**A1.** The authors agree that using such a burn-in period can result in the loss of samples. In the Markov chain aspect, we need a lot of samples to fully explore the target distribution. However, in the optimization aspect, it is not necessary to have that many as we only need a noisy (sub)gradient, and it is fine to discard some initial samples. Moreover, the burn-in period is just an optional choice in our design. One can, however, use all samples from the Markov chain, leading to no loss of samples. In our experiments on the burn-in period, discarding more samples even help improve training convergence rates.

For certain applications, e.g., training NN via PDEs, it is also fine to use all samples. Let's give an intuition. For a density $\pi$, if the Markov chain starts from an initial point in the region of low density of $\pi$, this sample is poor and should be discarded. The Markov chain then navigates towards the region of high probability, its states then gradually become similar to samples from $\pi$. If the chain starts from a high probability region, all samples are likely to be high quality (so no need burn-in). In the general context, we do not know if the initial state is good enough, so the burn-in period should be included as an option in our design. Return to the deep learning example, at iteration $k$, we simulate a Markov chain $x^k_1 \to x^k_2 \to \ldots x^k_M$ with target $p(x \vert x^k).$ Once simulated, $x^k_M$ should be in the region of high probability of $p(x \vert x^{k})$. Next iteration, we have to simulate a Markov chain with target $p(x \vert x^{k+1}).$ As $x^{k+1}$ is expected to be close $x^k$, the shapes of $p(x \vert x^k)$ and $p(x \vert x^{k+1})$ are somehow similar. So, $x^k_M$ is expected to be in a high probability region of $p(x \vert x^{k+1})$ too. If we, at iteration $k+1$, start the Markov chain from $x^k_M$, all samples can be kept.

**Q2. Convergence guarantees provided to be a natural extension of existing works on MM algorithms such as...**

**A2.** The above paper [Mairal] studies stochastic MM, meanwhile, the MM is closely related to DCA. However, we believe that our contribution deviates far away from the mentioned work, especially in terms of Markov samples. For e.g., the stochastic MM still requires i.i.d. samples from an exogenous source, which is unavailable in our setting. 

**Q3. Recent works have provided in-depth analysis of state-dependent Markov chain driven stochastic algorithms, e.g.,...**

**The above works consider stochastic gradient algorithms without the burn-in period.**

**A3.** For paper [Atchade], the distance between this paper and ours seems relatively far. In terms of the considered problem, they studied the objective $f + g$, where $f$ is L-smooth and $g$ is convex. Meanwhile, we study (nonsmooth) DC functions, and it is known that the class of DC functions is strictly larger than the above class (e.g, take $u(x) = - \vert x \vert$). In terms of algorithmic design, our MCSDCA is based on DCA, while their algorithm is based on the proximal gradient algorithm.

The paper [Karimi] studied $L$-smooth objective function, which is an even smaller class than  [Atchade]. From the algorithmic view, they studied stochastic gradient descent.

We have added these papers to the literature review of the revised version.

Lastly, as mentioned, the burn-in period is only an option in our algorithm.

**Q4. Without discussing the potential sampling complexity in doing so.**

**A4.** Thank you for suggestion. We view $G$ as a general function (deterministic, exogenous or endogenous stochastic). The key point is that $G$ is convex, where the host of algorithms for convex optimization is available with convergence rates provided in a variety of settings. Diving into analyzing these complexities for each case of $G$ is out of the scope of our paper.

**Q5. While the application to deep learning in Sec. 4 is interesting, currently it reads like a separate section.**

**A5.** Thank you, the purpose of Sec. 4 is to introduce the PDEs-regularized deep learning problem and point out its DC structure so that the MCSDCA can apply.

**The assumptions required by MCSDCA shall be verified**

Please refer to the Q-A2 of Reviewer i9Qz: MCSDCA applying to deep learning has two gaps: discretization error and mini-batch noise. In this revised version, in an effort to bridge the first gap, we give an additional analysis of time-inhomogeneity (Appendix B).

**Q6. Why such $A$ is needed?**

**A6.** $A$ can be used to encourage $x^{k+1}$ to be close to $x^k$ in some directions while being more free in other directions. E.g, if $A$ is the Hessian matrix (or its approximation) of the objective at $x^k$, the update will take a large (resp. small) step along the direction of low (resp. high) curvature (reduce oscillation).
