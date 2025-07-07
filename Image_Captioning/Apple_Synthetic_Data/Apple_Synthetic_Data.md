# Goal of the paper
Figure out if different multimodal foundation models have preferences of specific caption formats, and identify the optimal captions for each model.

# Main points of research
## Main Goals
- Identify the role and value of synthetic captions and how they interact withthe original AltText
- Identify the type of synthetic captions that are most effective for different foundational models
## Contributions
- Explore using MLLM as the image describer and present and controllable and human aligned pipeline to convert MLLM into an image captioner
- Synthesize several formats of captions and conduct extensive pre-training experiments to systematically study the role and synthetic captions and their intersection with original AltText
- verify the main goals (AltText provides better diversity and synthetic captions provide better alignment)

# Findings
## Overall Conclusions
- Rewriting techniques can enchance image-text alignment but may reduce data diversity
- Relying on synthetic captions can potentially hinder CLIP's training
- using 95% synthetic captions can yield superior results, especially if the captions are highly descriptive
- a small fraction (7%) of high quality data can significantly boost few-shot performance
## Caption Analysis
### Richness Assessment
- SSC mainly ranges from 10 - 15 tokens
- DSC ranges from 40 - 60 tokens
- Most DSC+ captions exceed 100 tokens
- Propose Average Number of Assertions (ANA) to quantify richness
  - SSC is 2.49, DSC is 8.13, DSC+ is 12.20
  - Higher is better
### Diversity assessment
- [AltText contains a higher number of unique entities](#diversity-assessment-graph)
## Image-Caption Data
- Tradeoff between richness of captions and their accuracy needs to be balanced
- AltText and synthetic captions are important for CLIP training, with shorter captions yielding better performance
- Pre-training and SFT benchmarks perform different on MLLMs
  - For SFT benchmarks, MM1 prefers DSC+
- DSC is the most effective for diffusion models
## CLIP
### Effect of synthetic captions
- SSC enchances retrieval performance, but leads to a drop in zero-shot ImageNet accuracy
- DSC performs worse than SSC across all benchmarks
  - Due to distribution mismatch as prompts in COCO/Flickr30k/ImageNet datasets are short
- DSC and SSC show similar performance to AltText after linear probing
### Intersection of synthetic captions and AltText
- Training CLIP solely on large-scale synthetic captions may get inferior results compared to AltText
- Adding AltText significantly boosts retrieval performance (>10% on COCO)
### Optimal mixture Ratio
- 40-50% of data should be SSC
  - lower proportion of AltText leads to drop in retrieval
  - lower proportion of SSC leads to decreased accuracy in ImageNet and zero-shot classification
- AltText provides broader knowledge coverage and greater diversity
### Optimal way to use AltText
- One approach is to fuse the knowledge from AltText into synthetic captions (AltText Fusion Captions, AFC)
- AFC shows improvement over DSC, but performs worse than a simple mixture of DSC and AFC
## MLLM
### Effect of synthetic captions for pre-training
- Synthetic captions yield better performance in image-text benchmarks across 0-shot to 8-shot settings
- DSC outperforms SSC
- DSC combined with AltText gives the best results
- A 66/33 mixture between synthetic captions and AltText yields the best results
- Using only synthetic captions leads to a significant drop in 0-shot performance
### Effect of synthetic captions on SFT
- DSC+ and the combination of AltText and DSC+ produced the best results
  - Detailed captions are the most important, even if they have the most hallucinations
- Concatenating AltText with synthetic captions does not yield significant improvement
- Hypothesize that image-caption data in pre-training is to enchance image-text alignment
### DSC+ alone can outperform diverse synthetic captions
- DSC+ alone yields the best performance after the SFT stage
- The combination of SSD, DSC, DSC+ and AFC does not lead to better results
- MLLMs may benefit more fro detailed captions during pre-training
## Diffision
### Effect of synthetic captions
- Leads to significantly improved results
- Descriptie captions show better results
### Ablation Study
- Higher DSC ratios lead to improvements in generation quality
- But the metrics for GenEval peaked at 50% DSC
- Use of SSC alone improves performance, but use of DSC and AltText appears to perform better

# Notes
## Overall Notes
- Alt text often has insufficient visual details and noisy content
- Current literature shows that synthetic captions can improve the performance of CLIP and MLLMs
### Caption Examples and Names
![An image with each type of caption as well as an example.](https://github.com/KiKuasaurus/LLM-Lit-Review/blob/master/Image_Captioning/Apple_Synthetic_Data/Caption_Examples.png)
### Diversity Assessment Graph
![An image showing the diversity assessment of each type of caption](https://github.com/KiKuasaurus/LLM-Lit-Review/blob/master/Image_Captioning/Apple_Synthetic_Data/Diversity_Assessment_Graph.png)
### Generation Pipeline
![An image detailing the generation pipeline for synthetic data that is used in the study.](https://github.com/KiKuasaurus/LLM-Lit-Review/blob/master/Image_Captioning/Apple_Synthetic_Data/Generation_Pipeline.png)
  
[original article](https://arxiv.org/abs/2410.02740)
