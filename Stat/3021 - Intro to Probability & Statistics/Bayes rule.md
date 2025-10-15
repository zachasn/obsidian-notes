---
tags:
  - type/lecture
  - course/stat3021
  - subject/statistics
course: STAT 3021
topic(s): Bayes' rule
date:
---
# Bayes’ rule
## Overview
Bayes' rule (or Bayes' theorem) is a fundamental result in probability theory that describes how to update probabilities based on new evidence. It provides a mathematical framework for revising beliefs when new information becomes available.

The rule is particularly useful when we know conditional probabilities in one direction (e.g., $P(A \mid B)$) but need to find the conditional probability in the reverse direction (e.g., $P(B \mid A)$). This "reversal" of conditional probabilities is the key insight that makes Bayes' rule so powerful in applications ranging from medical diagnosis to machine learning.

**Key applications include:**
- Medical diagnosis (disease probability given test results)
- Quality control (source identification given defect status)
- Spam filtering (spam classification given message content)
- Risk assessment and decision-making under uncertainty 
## Theorem
If $B_1, B_2, \ldots, B_k$ form a partition of sample space $S$, then for any event $A$ with $P(A) > 0$:
$$P(B_r \mid A) = \frac{P(B_r \cap A)}{P(A)} = \frac{P(B_r) \cdot P(A \mid B_r)}{\sum_{i=1}^{k} P(B_i) \cdot P(A \mid B_i)}$$
### Simplified Form (Two Events)
For events $A$ and $B$:
$$P(B \mid A) = \frac{P(B) \cdot P(A \mid B)}{P(A)} = \frac{P(B) \cdot P(A \mid B)}{P(B) \cdot P(A \mid B) + P(B') \cdot P(A \mid B')}$$
### Terminology
- **Prior probability**: $P(B_r)$ - probability before observing evidence $A$
- **Likelihood**: $P(A \mid B_r)$ - probability of evidence given the hypothesis
- **Posterior probability**: $P(B_r \mid A)$ - updated probability after observing evidence $A$

> [!important] Bayes' Rule Key Insight
> Bayes' Rule allows us to "reverse" conditional probabilities. If we know $P(A \mid B)$, we can find $P(B \mid A)$.
### Derivation
Starting from the definition of conditional probability:
$$P(B_r \mid A) = \frac{P(B_r \cap A)}{P(A)}$$
Using the multiplicative rule: $P(B_r \cap A) = P(B_r) \cdot P(A \mid B_r)$

Using the law of total probability: $P(A) = \sum_{i=1}^{k} P(B_i) \cdot P(A \mid B_i)$

Therefore:
$$P(B_r \mid A) = \frac{P(B_r) \cdot P(A \mid B_r)}{\sum_{i=1}^{k} P(B_i) \cdot P(A \mid B_i)}$$
## Examples
### Example 1: Medical Diagnosis
A disease affects 0.1% of the population. A diagnostic test has:
- Sensitivity (true positive rate): $P(\text{positive} \mid \text{disease}) = 0.99$
- Specificity: $P(\text{negative} \mid \text{no disease}) = 0.95$

If a person tests positive, what is the probability they actually have the disease?

**Solution**:
Let $D$ = has disease, $T$ = tests positive
Given:
- $P(D) = 0.001$, so $P(D') = 0.999$
- $P(T \mid D) = 0.99$ (sensitivity)
- $P(T' \mid D') = 0.95$, so $P(T \mid D') = 0.05$ (false positive rate)
Using Bayes' Rule:
$$\begin{align}
P(D \mid T) &= \frac{P(D) \cdot P(T \mid D)}{P(D) \cdot P(T \mid D) + P(D') \cdot P(T \mid D')} \\
&= \frac{(0.001)(0.99)}{(0.001)(0.99) + (0.999)(0.05)} \\
&= \frac{0.00099}{0.00099 + 0.04995} \\
&= \frac{0.00099}{0.05094} \\
&\approx 0.0194
\end{align}$$
**Interpretation**: Only about 1.94% of people who test positive actually have the disease! This counterintuitive result occurs because the disease is so rare.
### Example 2: Manufacturing Source (Continuation)
Continuing from the factory example: If a randomly selected item is defective, what is the probability it came from Machine 1?

**Solution**:
We want $P(M_1 \mid D)$. We already calculated $P(D) = 0.017$.
Using Bayes' Rule:
$$\begin{align}
P(M_1 \mid D) &= \frac{P(M_1) \cdot P(D \mid M_1)}{P(D)} \\
&= \frac{(0.50)(0.01)}{0.017} \\
&= \frac{0.005}{0.017} \\
&\approx 0.294
\end{align}$$
We can find the probabilities for the other machines similarly:
$$P(M_2 \mid D) = \frac{(0.30)(0.02)}{0.017} = \frac{0.006}{0.017} \approx 0.353$$
$$P(M_3 \mid D) = \frac{(0.20)(0.03)}{0.017} = \frac{0.006}{0.017} \approx 0.353$$
**Interpretation**: Although Machine 1 produces the most items, given that an item is defective, it's more likely to have come from Machine 2 or 3 (which have higher defect rates).
### Example 6: Email Spam Filter
An email filter classifies emails as spam or legitimate. Based on historical data:
- 40% of all emails are spam
- 60% of spam emails contain the word "free"
- 5% of legitimate emails contain the word "free"

If an email contains the word "free," what is the probability it is spam?
**Solution**:
Let $S$ = spam, $F$ = contains "free"
Given:
- $P(S) = 0.40$, $P(S') = 0.60$
- $P(F \mid S) = 0.60$
- $P(F \mid S') = 0.05$

Using Bayes' Rule:
$$\begin{align}
P(S \mid F) &= \frac{P(S) \cdot P(F \mid S)}{P(S) \cdot P(F \mid S) + P(S') \cdot P(F \mid S')} \\
&= \frac{(0.40)(0.60)}{(0.40)(0.60) + (0.60)(0.05)} \\
&= \frac{0.24}{0.24 + 0.03} \\
&= \frac{0.24}{0.27} \\
&\approx 0.889
\end{align}$$
**Interpretation**: About 88.9% of emails containing "free" are spam.
> [!tip] When to Use Bayes' Rule
> Use Bayes' Rule when you need to find $P(B \mid A)$ but you know:
> - The prior probabilities $P(B_i)$
> - The likelihoods $P(A \mid B_i)$
>
> Common applications: medical diagnosis, quality control, spam filtering, risk assessment.
## Practice
### Practice Problems
**Problem 1: Rare Disease Testing**
A rare disease affects 1 in 1000 people. A screening test is 98% accurate for those with the disease (sensitivity) and 95% accurate for those without the disease (specificity). If a person tests positive, what is the probability they have the disease?

**Problem 2: Factory Quality Control**
A factory has three production lines: Line A (40% of production, 2% defect rate), Line B (35% of production, 3% defect rate), and Line C (25% of production, 5% defect rate). If a randomly selected product is defective, what is the probability it came from Line C?

**Problem 3: Weather Forecasting**
In a certain region, it rains 20% of days. When it rains, the forecast predicts rain 85% of the time. When it doesn't rain, the forecast incorrectly predicts rain 15% of the time. If the forecast predicts rain tomorrow, what is the probability it will actually rain?

**Problem 4: Multiple Testing**
A diagnostic test for a condition has 90% sensitivity and 92% specificity. The condition affects 5% of the population. A patient tests positive. They take the test a second time and test positive again. Assuming the tests are independent, what is the probability the patient has the condition?
### Solutions

