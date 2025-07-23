# Goal of the paper
Construct an evaluation benchmark based on multi turn conversations as well as make an open sourced data set with better multi-turn data.

# Main points of research
- Create a benchmark that can assess muti-turn and multi-image dialog understanding capabilities of LVLMs
- Benchmark should have these features
  1. Multi-turn and Multi-image
  2. Long Context
  3. Open-ended Evaluation
- Conduct extensive research on exisitng LVLMs and proprietary MLLMs on MMDU
- Develop a dataset to enchance dialog understanding abilities

# Findings
- Closed source models (46-70) performed much better than open sourced ones (14-42)
- Training on MMDU-45k yielded a sizeable (15-40%) increase in performance
- Ablation study showed that MMDU can increase context window size of LLaVA



# Notes
## Benchmark Constuction
### Data collection
- Use clustering to construct high-quality image sets from wikipedia
### Construction with GPT-4o
- Used GPT-4o to generate corresponding question answer pairs for each image
- Input combinations of multiple images into GPT-4o to generate multi turn Q&A pairs based on multiple images
- Combined the 2 sets of multi-turn images to create dialogues that reference both one and many images.
- got gpt to organise the generated text according to a specified Text-Image Interleaving Format
### QC with human annotators
- Combine automated and manual screening methods to select image annd texts that meet standards
- Did multi round review
  1. first do a preliminary check by regular reviewers (junior PhD or senior researchers)
  2. then do an involved in depth examination and modification by experts (senior researchers or PhD students with relevant background)
- Can refer to the appendix for their web UI

## Dataset construction
- Same as benchmark, except withoout the manual review
- Dataset had an average of 9 turns, max of 27
- Each data includes 2-5 images

[original article](https://arxiv.org/abs/2406.11833)
