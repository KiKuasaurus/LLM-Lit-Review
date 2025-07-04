# Goal of the paper:
Figure out what the most effective way to do post training to get the best results.

# Main Points of Research:
- Identify a set of core sils to improve and build an evauation framework to suport it
- Use multiple stages and different types of data to do post training
- Introduce Reinforcement Learning wih Verifiable Rewards (RVLR)

# Findings:
### Data
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

### Supervised Finetuning
- SFT Data
  - Prompt responses are all from either frontier models or humans
  - Start with skill specific data sets then mixed them (establishes upper bound)
  - Diversity, the new persona data and specialised data generally improved performance
  - Safety was also orthogonal to performance
  - Stratified subsampling generally did not improve performance
- SFT Analysis
  - Using a model with better pre-training tends to lead to better final results
  - Replacing newlines with an eos token improved performance, but led to generation inconsistency
  - The best model soup generally performs equal to the best random seed
  - Transformers had a bug with loss aggregation - sum loss was used instead of mean loss

### Preference Finetuning
- Uses human or ai feedback to simulate human preferences
- *to add full setup info at the bottom (pg 20-21)*
- Prompt to preference data
  - include both prompts in and out of SFT data
    - the inclusion of unused prompts leads to better performance
    - combining both led to best results
  - sample 4 models of a model pool to generate responses
  - include on-policy data by sampling completions from Tulu model 
    - including on-policy data improves aggregated downstream DPO performance
  - Use GPT-4o to rate each response and take the highest rated response as the chosen one and rarndomly sampled another one as the rejected response.
    - Performance across LLM judges are similar, but GPT-4o is slightly better
- Key findings
  - Scaling number of unique prompts improves downstream DPO performance
  - Ultrafeedback has limited performance as its completions are mainly sourced from models that are less capable than the 70b model
  - only Tulu 3 persona IF 

[original article](https://arxiv.org/abs/2411.15124)