# Goal of the paper
Figure out what the most effective way to do post training to get the best results.

# Main Points of Research
- Identify a set of core sils to improve and build an evauation framework to suport it
- Use multiple stages and different types of data to do post training
- Introduce Reinforcement Learning wih Verifiable Rewards (RVLR)

# Findings
## Data
- Public Datasets
    - **Diversity:** improves generalisation, reduces forgetfulness and improves robustness to uncommon inputs
    - **Target Skills:** focus of this paper is complex reasoning, precise instruction following and coding
- Synthesizing for Target Skills
- Synthetic data is cheap and customisable, but LMs are susceptible to "mode collapse" (repetitive modes/patterns)
- Solved by using *persona-driven* methodology
    - use different personas with a data synthesis prompt to synthesize dta with corrresponding perspectives
- **Precise Instruction Following:** ability to follow instructions in natrual language (*e.g.,* number of words, number of paragrahs)
- **Math and Coding:** Make a variety of problems using GPT-4o, then use GPT-4o to write solutions to the math problems and claude-3-5-sonnet to solve python problems
- **Noncompliance and Safety:** use noncompliance taxamony from Brahman et al. to ensure model can reliably reject unsafe and appropriately handle nuanced and out of scope queries
- Prompt Decontamination
    - Remove overlap between training prompts and evaluation set
    - used n-gram method (n=8)
    - matched tokens if they share an 8-gram containing the token, significant overlap if >50% maatch
    - if instances overlap with >2% of of eval set consider it contaminated
    - remove entire dataset if it doesnt affect performance, else remove specific instance

## Supervised Finetuning
### SFT Data
  - Prompt responses are all from either frontier models or humans
  - Start with skill specific data sets then mixed them (establishes upper bound)
  - Diversity, the new persona data and specialised data generally improved performance
  - Safety was also orthogonal to performance
  - Stratified subsampling generally did not improve performance  

### SFT Analysis
  - Using a model with better pre-training tends to lead to better final results
  - Replacing newlines with an eos token improved performance, but led to generation inconsistency
  - The best model soup generally performs equal to the best random seed
  - Transformers had a bug with loss aggregation - sum loss was used instead of mean loss

## Preference Finetuning
Uses human or ai feedback to simulate human preferences  

### Key findings
- the inclusion of unused prompts leads to better performance
- combining both led to best results
- including on-policy data improves aggregated downstream DPO performance
- Performance across LLM judges are similar, but GPT-4o is slightly better
- Scaling number of unique prompts improves downstream DPO performance
- Ultrafeedback has limited performance as its completions are mainly sourced from models that are less capable than the 70b model
- only Tulu 3 persona IF improves the average eval score and the targeted IFEval score  
- IF persona significantly imrpves IFEVal scores, IF-augmented-verified leads to only a small improvement while tanking average [[Targeting IF](#targeting-if)]
- a mix of IF augmented and persona IF led to the best results
- the WildChat IF generally improves DPO performance
- using the prompt to data prefgerence pipeline generally improves datasets

### Hyperparameter and Algorithm Design
  - out of DPO SimPO and length-normalized DPO, only the third option outperformed the base checkpoint
  - a learning rate of $2.0 \times 10^{-7}$ or $5.0 \times 10^{-7}$ performs better than a lower learnign rate
  - RM performance on RM-specific benchmarks does not necessarily translate to better downstream performance
  - iterating with RM and PPO is more expensive than iterating with DPO
    - PPO gets similar average scores with DPO in a non-tuned setup
    - PPO is more computationally expensive
  - Pre-computing and caching DPO log probabilities eliminates the need to allocate GPU memory for the reference model
  - Separating forward passes for chosen and rejected sequences results in near identical training loss, but saves gpu memory

## Reinforcement Learning with Verifiable Rewards (RLVR)
- Uses tasks with verifiable outcomes (math problems/ instruction following)
- Replace the reward model with a verification function
- demonstrates improvements on benchamrks like GSM8K

### Data
- consists of prompts with accompanying binary verifiers
- answer extraction and verification method is domain dependant

### Key Findings
- RLVR improved test performance for all 3 settings
- verifiable rewards improves consistency
- Value function plays an important role in RLVR's training 
  - Initializing the value from a general RM obtains the highest GSM8K test score
- Using only the verifiable rewards outperforms using scores from the RM
- Starting with a weaker model can converge to the same verifiable rewards
  - starting from both SFT and DPO can lead to the same level of verifiable rewards
  - SFT incurs a larger KL as it is further from good at GSM8K then DPO

## Discussions
### Scaling Tulu 3 up to Llama 3,1 405B
- there were several hardware related issues
- an 8B value model was used for RLVR to reduce compute costs
- RLVR was only done for MATH as initial RLVR runs did not show as much improvement in the other 2 tests
- only trained for 75 steps

### Insights from the Unfruitful
- **Online DPO:** resulted in no or little improvement on GSMSK and degredation on MATH
- **Rejection Sampling:** performance gain was minimal and publicly available models are not strong judges.
  - Adding the original response as a choice for the judge performed better


# Notes
## Preference Finetuning
### Setup
- **Preference Data:** consists of prompts $x$ and two responses $y$ and $y'$, then the judge will choose and label the preferred and rejected response
-**Reward Model:** maximises teh difference between the rewards, which is the log-likelihood that $y_c$ will be preferred over $y_r$


### Prompt to preference data pipeline
1. include both prompts from in and out of SFT data
2. sample 4 models from a model pool to generate responses
3. include on-policy data by sampling completions from a Tulu model
4. Use GPT-4o to rate each response and take the highest rated response as the chosen one
5. randomly sample another one as the rejected response  

### Targeting IF
1. **Persona IF:** relax one of the constraints and use the prompt output as a rejected response
2. **IF augmented:** randomly sample instrcutions from the Tulu 2 SFT mix and combine them with extra contraints, then use the prompt to preference data pipeline to get completions
3. **WildChat IF:** sample instructions from WildChat and use GPT-4 to extract whether a prompt contains a constraint.

## Reinforcement Learning with Verifiable Rewards (RLVR)
### Data
- **GSM8K:** augment each sample with an 8-shot prompt then extrac the final number produced and compare to the ground-truth label to determine correctness
- **MATH:** augment each sample with a 3-shot prompt and extract the answer and determine correctness following the 'flex' MATH evaluation logic
- **IFEval:** randomly sample instructions from the Tulu 2 SFT mix and combine them with constraints, then use a verification function for each of the constraint templates
- the models are then trained via PPO

### Recipes and Analysis
1. Initialize the value model from a general RM
2. Set the dropout probability to 0 so that tokenlog probabilites can be computed deterministically during the forward pass
3. Train with SFT dataset and shufle between Epochs
  - PPO can train for more episodes than the total available prompts
4. Add a -10 penalty if the sampled response does not end with an EOS token
5. Normalize the advantages by subtracting mean followed by dividing by standard deviation

[original article](https://arxiv.org/abs/2411.15124)