# Goal of the paper
A joint post-training framework can improve performance while maintaining similar computational costs

# Main points of research
- Mathematically prove that with sequential DPO and SFT, there is the existence of a non-diminishing performance gap
- Propose and test and algorithm to allow for joint training that uses similar compute while producing better results

# Findings
## Experiment Results
### ALRIGHT provides better control over the trade-off
- Final models produced by ALRIGHT are more evenly distributed int he objective space
- The spread of the models is comparable to that of the mix method
- Sequential tends to produce models that are biased towards SFT, even if $T_{DPO} >>T_{SFT}$
### MAXRIGHT achieves near ideal performance
- Behaviour is supported by Ideal Distance comparison 
- Attributed to the adaptive nature of MAXRIGHT, which dynamically selects what to optimise for
### ALRIGHT and MAXRIGHT require minimal additional resources
- ALRIGHT has almost no increase (even negative when $\lambda$=0.01) compared to sequential
- MAXRIGHT has slight increase in runtime compared to sequential
- significantly less compared to mixed
### Effect of maximum evaluation step for memory efficient MAXRIGHT
- incurs high runtime costs when $\lambda$ was low
- however it has near ideal optimisation trajectory
- high values (1000) cause significant oscillations
- also causes high gpu utilization from excessive SFT updates
# Notes
## ALRIGHT
- Uses a bernoulli distribution to decide whether to optimize for DPO or SFT
- Basically a (weighted) coinflip and just alternate optimization
## MAXRIGHT
- computes difference between current state and that of the ideal model
- scales it with $\lambda$
- optimizes the parameter based on the calculated value
- Additional optimizations can be made to only calcualte the value every $k$ steps to reduce compute costs
## Graphs for reference

![alt text](Figure_1.png)
![alt text](Figure_2.png)
![alt text](Figure_3.png)
![alt text](Figure_4.png)

[original article](https://arxiv.org/abs/2410.15483)
