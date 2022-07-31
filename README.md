# NIPS-author-rebuttal


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
