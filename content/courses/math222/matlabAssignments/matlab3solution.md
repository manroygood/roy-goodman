---
title: MATLAB Assignment 3 Solution
linktitle: Lab 3 Answer
toc: true
type: book
date: "2021-01-06"
draft: 	false
weight: 235
---

__NJIT Math 222, Fall 2020__  

__Third MATLAB Assignment__

__Due week of November 30__ 

#### Question 1

Show that $\frac{\mathrm{d}}{\mathrm{d}t} (S+I+R)=0$, so that if $S(0)+I(0)+R(0)=N$ then $S(t)+I(t)+R(t)=N$ for all $t$.

##### Answer

$$
\begin{aligned}
\frac{\mathrm{d}}{\mathrm{d}t} (S+I+R)&= 
\frac{\mathrm{d}S}{\mathrm{d}t}+ \frac{\mathrm{d}I}{\mathrm{d}t}+\frac{\mathrm{d}R}{\mathrm{d}t}\\
&= -\beta S I + (\beta S I - \gamma I) + \gamma I\\
&= 0.
\end{aligned}
$$



#### Question 2

* Where would the imposition or a stay-at-home order effect this model?

  _As people stay at home, they have less contact with others, which decreases the contact rate $c$ and thus of $\beta$._

* Where would widespread adoption of mask wearing effect this model?

  _The more people wear masks, the lower the probability that any individual contact leads to transmission. That is, it reduces $p$ and thus reduces $\beta$._ 

* Come up with a feature that you think is missing from the model but straightforward to add to the model, and propose a modification to the model that you think would incorporate this feature. In particular, identify any new variables or terms needed for you modified model.

  * There are many features we know about the disease that we have simplified out of the model. I'll focus on two

    1. An important fact about COVID is that it has what's estimated to be a two-week incubation period before infected individuals show symptoms and that infected people are most likely to pass on the disease for a few days before and after the onset of symptoms. Expressing this relationship precisely using mathematics is somewhat beyond the scope of this class. 

       * One way to do this might be to make the rate at which individuals transition from susceptible to infected proportional to a _delayed_ value $S(t-\tau)I(t-\tau).$ Solving such _delay differential equations_ (DDE) is tough, both theoretically and numerically. 

       * Another way to do this is to introduce a fourth group of individuals, the _exposed_ group, represented by $E(t)$. Individuals now move from state to state like this:

         

         ```mermaid
         graph LR;
           S((S))-- β S I -->E((E));
           E -- c E --> I((I))
           I-- ɣ I -->R((R));
         ```

         Now, of course the number of equations is larger so we can't plot things nicely in a two-dimensional phase plane anymore.

    2. Another assumption that we could remove is the assumption of a well-mixed population. In a gross oversimplification, we can divide the population into two subpopulations. __Subpopulation A__, the "safe" group, is able and willing to follow safety precautions. __Subpopulation B__ the "risky" group, engages in more risky behavior, either because their economic situation forces them to work a riskier job or because they ignore the recommendations of public health experts. The rates at which __A__ people and __B__ people meet each other and the probability that a contact leads to transmission are different. Further, type **B** people receive higher viral loads and, on average, get sicker, so the rate at which they recover is slower.

       There are some places everyone may meet (the grocery store), and there are some places only __A__ type people go and some places only __B__ type people go (they might have to work in a meat-processing plant, or they might be going to underground dance parties, or who knows where). They also have different rates of recovery.
       $$
       \begin{aligned}
       \frac{\mathrm{d}S_A}{\mathrm{d}t}&=-\beta_{AA}S_A I_A - \beta_{BA} S_A I_B, &\qquad
       \frac{\mathrm{d}S_B}{\mathrm{d}t}&=-\beta_{AB}S_B I_A - \beta_{BB} S_B I_B, \\\\
       \frac{\mathrm{d}I_A}{\mathrm{d}t}&=\beta_{AA}S_A I_A + \beta_{BA} S_A I_B -\gamma_A I_A,&\qquad
       \frac{\mathrm{d}I_B}{\mathrm{d}t}&=\beta_{AB}S_B I_A + \beta_{BB} S_B I_B -\gamma_B I_B, \\\\
       \frac{\mathrm{d}R_A}{\mathrm{d}t}&=-\gamma_A I_A, &
       \frac{\mathrm{d}R_B}{\mathrm{d}t}&=-\gamma_B I_B.
       \end{aligned}
       $$

    

    Note that $\beta_{BA}$ is involved in the transmission of the pathogen from the $B$ subpopulation to the  $A$ subpopulation, while $\beta_{AB}$ measures transmission rate from the $A$ subpopulation to the $B$ subpopulation. There are a lot of constants to figure out in this model!

  * You may have come up with your own idea, and that's great too. 

#### Computational project part 1

I ran the code with $N=10000$, $\gamma=1$ and $\beta$ varying. The assignment says to expect different behavior depending on whether or not $R_0>1$, where $R_0 = \frac{\beta N}{\gamma}$. 

For these values of $N$ and $\gamma$, this an epidemic when $\beta> 10^{-4}$. Therefore I tried three values of $\beta$: $2\times 10^{-4}$, $1.25\times 10^{-4}$, and $9\times10^{-5}$. For the first two values, I saw growth, with a larger epidemic in the first case $\beta= 2\times10^{-4}$.

My results are in a {{% staticref "math222/SIRsolution.html" %}} MATLAB-generated webpage{{% /staticref %}}. This was generated from {{% staticref "math222/SIRsolution.mlx" %}} this MATLAB code{{% /staticref %}}.

#### Computational project part 2

Here you were asked to use phase-plane drawing software to examine solutions graphically. It was pointed out that $\frac{dI}{dt}=0$ when $S^* = \frac{\gamma}{\beta}$. From the following three images, we see that $S_0> S^*$ when $\beta>10^{-4}$, which is when we see epidemics.

{{< figure library="true" src="math222/beta_0p0002.png" title="Phase plane with $\beta=2\times10^{-4}$, $S^*=5000$" lightbox="true" >}}

{{< figure library="true" src="math222/beta_0p000125.png" title="Phase plane with $\beta=1.25\times10^{-4}$, $S^*=8000$" lightbox="true" >}}

In the third case, $\beta=9\times10^{-5}$ $S_0=11111>N=10000$, so the infection number is decreasing at $t=0$.

{{< figure library="true" src="math222/beta_0p00009.png" title="Phase plane with $\beta=9\times10^{-5}$" lightbox="true" >}}


**There is also a question asked "Can this model support a sustained epidemic."** The answer is that it cannot. In a sustained epidemic, the infection rate $I(t)$ would have to reach a nonzero steady state but the phase plane shows that $\lim_{t\to\infty}I(t)=0$ for all solutions, so it can't sustain an epidemic. To support a sustained epidemic, a model must refresh its supply of susceptible individuals. Fortunately for the germs, we do that anyway by having babies! A more complete models will include births.