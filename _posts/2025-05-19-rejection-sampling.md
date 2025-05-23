---
title: "Rejection Sampling Algorithm"
layout: post
---

<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>


## Overview:
Rejection Sampling is an algorithm that provides us with an intuitive way o sample from random variables from a **target distribution**, called \(q(x)\), that cannot be sampled from directly but is known and a **proposal distribution**, call it \(p(x)\), that can be sampled from directly. The rejection sampling algorithm requires samples to be drawn from \(p(x)\) and are evaluated to accept whether the sample can be assumed to be drawn from \(q(x)\). If the sample cannot be assumed to be drawn from \(q(x)\), the sample is rejected and the process is repeated. The underlying acceptance of the sample drawn from \(p(x)\) relies on the overlap between \(p(x)\) and \(q(x)\). If \(p(x)\) and \(q(x)\) are similar distributions then we say the two distributions have high overlap and samples drawn from either distribution could be drawn from the other. The opposite is true for distributions with low overlap. Rejection sampling provides us with an introductory understanding of overlap as we look for distributions from which we construct the constant $M$: 

$$M = \sup_{x \in \Omega} \frac{q(x)}{p(x)} < \infty$$

We note that $M \geq 1$:
**Proof**:
	Suppose $M < 1$. Since M is the supremum of $\frac{q(x)}{p(x)}$ then the following is true:
	$$M \geq \frac{q(x)}{p(x)} \Rightarrow Mp(x) \geq q(x)\)$
	Since \(p(x)\) and \(q(x)\) are p.d.f's then they integrate to 1. Therefore, if $M < 1$ then $\int p(x)dx \neq 1$ which is a contraction.
	Suppose $M = 1$, then clearly \(p(x)\)= q(x)\).

Since $M \geq 1$, we know the following must be true for \(q(x)\):
1. \(q(x) \geq p(x) \space \space \space \forall x \in \Omega$
2. \(p(x)\) and \(q(x)\) share the same $\Omega$

We can quantify the measure of overlap with $M$ since $M$ is a measure of similarity between \(p(x)\) and \(q(x)\). If $M$ is very close to 1, \(p(x)\) and \(q(x)\) are quite similar. if M is very close to 0, \(p(x)\) and \(q(x)\) are not similar. Note, a downside to rejection sampling is that $M$ must exist which heavily constrains the number of density functions we can apply rejection sampling sampling to. 

Once we've determined $M$, we can generate samples $X_1,X_2,…,X_n$ from \(p(x)\) and apply the rejection sampling algorithm. The accepted samples will then follow the target distribution \(q(x)\).

## Rejection-Sampling algorithm:
1. Draw a sample $X \sim p(x)$
2. Draw a sample $u \sim U[0,1]$
3. Test
	If: 
		$u < \frac{q(x)}{M*p(x)}$
	then:
		$X \sim q$
	else:
		Discard $X$ and $u$
4. Repeat

Here is an example of the algorithm implemented in Python:
```
def rejection_sampling(num_samples, M, prop_dist, target_dist):
    samples = []
    for _ in range(num_samples):
            #Random variable from our proposed distribution
            x = prop_dist.rvs()
            #Random variable from our uniform distribution
            u = scipy.stats.uniform.rvs()
                
            #Ratio is the realized random variable evaluated at both the proposed and the target distributions
            ratio = target_dist.pdf(x) / (prop_dist.pdf(x)*M)
            
            
            if u <= (ratio):  # Accept-reject condition
                
                samples.append(x)
    return np.array(samples)

```

## Example:
Here we use \(p \s\)m \text{Exp}(\lambda = \frac{1}{2})$ to target a standard exponential distribution:
![Alt Text](Animations/rejection_sampling_example.png)

## Proof of Rejection Sampling
Our goal is to show once we follow the rejection sample algorithm, the random variable we drew $X \sim q(x)\). Probability theory tells us that if  $X \sim q(x)\), the following are equivalent:

$$\Pr(X \in A) = \int_A q(x)\text{dx} = Q(x)\)$

Where $A$ is sample space of \(q(x)\). This will be the idea we wish to follow for proof of the rejection sampling algorithm.

When we run the rejection sampling algorithm, theres a chance we accept or reject the sample. This chance is random before the algorithm begins and know the outcome is binary. Whether or not the outcome occurs for every iteration of the algorithm is a random variable (r.v) that we call $Z$ where $Z$ take on values 1 or 0, either the algorithm accepts or rejects the sample. We've defined the conditions when this sample is accepted rejected in the algorithm and it is based on the inequality:

$U < \frac{q(x)}{M*p(x)}$ 

It follows that since M is the supremum of $\frac{q(x)}{p(x)}$, $\frac{q(x)}{M*p(x)}$ is bounded on the interval of $[0,1]$ for all x in the support of \(p(x)\) and \(q(x)\).This is perfect for us because we can now assign the probability the event occurs to our new random variable $Z$ as:

$Z \sim  \text{Bernoulli}(\frac{q(x)}{Mp(x)})$


Now that $Z$ is well defined, we turn our attention to the following:

$$X | Z=1$$

Essentially, what is the distribution of $X$ after we've accepted the sample? We want to show that given we've accepted our sample, we can say $X|Z=1 \sim q(x)\). To do so we'll need the following:
$$\Pr(X \in A | Z=1) = Q(x)\)$
where $A$ is the sample space of \(q(x)\).
Using Bayes' Theorem we can rewrite $\Pr(X \in A | Z=1)$ as:

$$
\Pr(X \in A | Z=1) = \frac{\Pr(Z = 1 | X \in A) \cdot \Pr(X \in A)}{\Pr(Z=1)}
$$

Lets turn our attention to the numerator. Since we have a conditional probability, we can make the following observation:

$$
\Pr(Z = 1 | X \in A) = \frac{\Pr(Z=1 \cap X \in A)}{\Pr(X \in A)}
$$

By multiplying both sides by $\Pr(X \in A)$ and plugging the right hand side into the numerator we arrive at:

$$
\Pr(X \in A | Z=1) = \frac{\Pr(Z=1 \cap X \in A)}{\Pr(Z=1)}
$$

Looking deeper into $\Pr(Z=1 \cap K \in A)$, this is something we can evaluate:
By the law of Total Probability:

$$
\Pr(Z=1 \cap K \in A) = \int_{A} (U < \frac{q(x)}{Mp(x)}) \cdot p(x)\text{dx} = \iint_{0}^{1} (U < \frac{q(x)}{Mp(x)}) \cdot p(x) \text{dx} = \int_{-\infty}^{a} \frac{q(x)}{Mp(x)}\cdot p(x) \text{dx}
$$

$$
\Rightarrow \int_{-\infty}^{a} \frac{q(x)}{M}\text{dx} = \frac{1}{M} \int_{-\infty}^{a}q(x)\text{dx} = \frac{1}{M} \Pr(X \in A) = \frac{1}{M} \cdot Q(x)
$$

Now we turn our focus to the denominator $\Pr(Z=1)$. 

$$
\Pr(Z=1) = \int_{-\infty}^{\infty} 1(U < \frac{q(x)}{Mp(x)}) p(x) \text{dx} = \int_{-\infty}^{\infty} \frac{q(x)}{Mp(x)}p(x) \text{dx} = \frac{1}{M} \int_{-\infty}^{\infty} q(x)\text{dx} = \frac{1}{M} (1) = \frac{1}{M}
$$

The key step here is $1$ which is an indicator variable that tells us if the condition: $U < \frac{p(x)}{Mq(x)}$ 
is met or not.

Finishing up we arrive at out final result:

$$
\Pr(X \in A | Z=1) = \frac{\frac{1}{M} \Pr(X \in A)}{\frac{1}{M}} = \Pr(X \in A) = Q(x)
$$

Therefore, given we accept the random variable we sampled, We can say $X \sim q(x)\)



While rejection sampling does provide us with the result we want, there are two major disadvantages to it:
1.  It is horribly inefficient algorithm. This is because there's no guarantee how many samples we'll need to get n samples of X coming from q. This is because if we don't accept a sample, we discard it and run the algorithm again. *Example of low acceptance rate algo here:*
2. 
