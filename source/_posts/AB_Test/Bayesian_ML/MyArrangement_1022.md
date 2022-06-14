---
layout: post
categories: BayesianML
tag: [] 
date: 2018-10-22

---



## Real World AB Testing

### Pattern

- about cmp 2 or more items
- each group of itmes produces numbers
- Which group has a higher/lower number (stat speaks)?
  - We want a hard drive to last longer (higher #)
  - We want blood pressure to go down (lower #)
  - How do we detect these differeneces?





**Stat & CS dept use diff programming and priorities and paradigms of thinking!**

past of my exposure was probably isolated to one of the two sides. Bayesian for Stat and Bayesian for ML are very different!

- Route: 

  have to understand the old then the **new** , side by side, So a complete picture fo all the techs available and then able to their merits appropriately

  - Old: 

    - review of prob & stats

    - Frequency Statistics & hypothesis testing --> Bayesian Stat 

      ​			 ↓==AS==↓
      Newton --> Quantum Physics

  - So, **frequencies method (Maximum Likelihood)**, then move onto **adaptive methods**

    - for Online platforms with millions of users
    - Culminate in Bayesian Approach to AB testing! --> for online advertising

  - adaptive:

    - ε-greedy
    - UCB
    - thompson -- related to Bayesian ML

  - **MLE VS MAP**

|      | click    | not click |      |
| ---- | -------- | --------- | ---- |
| A    |          |           | sumA |
| B    |          |           |      |
|      | sumclick |           |      |





## Conclusion of Experiments

P-Value

**t value** 

**p value** big --> significance! --> B is diff from A



t-test

Confidence Interval , 0.05 5%, 1%, 0.1%



## Why t --> Chi2?

- "1% vs 2%" talks different stories under diff whole amount

1 out of 10 VS 2 out of 10 

10 out of 100 vs 20 out 200, 



## exp on CTR







## Freq approaches vs Bayesian ML

### 

- Heights of 
  1. collect data
  2. likelihood
  3. max 2)



- Diff: 
  - Freq : mean & var are numbers
  - Bayesian : mean & var are prob distributions, I have to **find their prob distributions.**, instead of a number.
- Sampling is a domain. --> Markov Chain Monte Carlo, Gibbs, 
  - main: numerical approximate of an **integral**



- WHOLE other direction:

  - ML & "Bayesian" predictive models
  - Weight is "distribution", not merely number

- Non-Bayesian Example

  - as NN - **no assumptions** about how the inputs are related

- **Bayesian Network:**

  ```mermaid
  graph LR
  	studying --> A[pass/failed]
  	video_game --> sleep --> A[pass/failed]
  ```

  
  - We can model these dependencies explicitly, whereas with NN, we always use the same model and let the NN figure out what the weights should be

- Bayesian ML may even be ***more complex*** than DL

- Bayesian Logistic Regression > Logistic Regression





for stat. 

if in a summer school  --> 3 weeks, twice 3hrs in a week

biologists, psychologists, web developers







#### My Appendix

- discipline myself
  - Appendix -- How to code by yourself
  - Should you code along? on youtube
  - Theory --> Code
- How to suc?
  - answer to my Q might be a Q
  - **ML is experimentation, not philosophy**





