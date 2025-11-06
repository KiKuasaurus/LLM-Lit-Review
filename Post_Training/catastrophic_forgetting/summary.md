# Goal of the paper
Evaluate the forgetting phenomenon in LLMs' knowledge during continual instruction tuning

# Main Points of research
* If general knowledge stored in LLMs are forgotten during continual instruction tuning
* The effects of model scales, architectures and general instruction tuning on the forgetting problem
* Ways to mitigate the forgetting problem

# Findings
## Forgetting phenomenon generally exists
- Models tend to lose some amount of domain knowledge, reasoning and reading comprehension ability after instruct tuning
## Impact of model size
- Larger models tend to experience larger losses in the above fields
- The final performance of BLOOMZ-1.1b and BLOOMZ-7.1b was about the same, even though the larger model had better performance initially
- Models likely still shift parameters to a large extent during instruct tuning
- No correlation betwen initial performance in bias and model size
## Impact of model architecture
- Decoder-only model architectures can maintian more knowledge than encoder-decoder models
- Difference could be caused by the autoregressive nature of the model or the differences in training objectives
## Impact of general instruction tuning
- General instruction tuning tends to have a positive impact
- Most evident when comparing ALPACA and LLAMA, where ALPACA does not have better inital performance, yet it performs better after continual instruction tuning
## Ways to mitigate the problem
- General instruction tuning data can be included during continual instruction tuning
- General instruction tuning performed before continual instruction tuning helps to further mitigate this issue
