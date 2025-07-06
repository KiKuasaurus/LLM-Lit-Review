# Goal of the paper
Test the hypothesis that the inclusion of fine grained concept annotations will improve mllm performance

# Main points of research
- Set up a way to test the effectiveness of the inclusion of the 4 main components of their annotated data
- 4 main components
  1.  Coarse-grained image captions (C)
  2. Fine-grained category labels (L)
  3. Label descriptions (D)
  4. Object regions (R)

# Findings
## Identification
- Inclusion of D helps to improve object identification
- Inclusion of R helps with identifying object relations with other objects and the space around them
## Generation
- Addition of L and D help the model to understand visual details
- Addition of R helps the model to understand space better 
## Comparison and Collaboration with IC data
- [IC dataset](#ic-dataset) refers to course grain image/caption pairs
- MMGiC achieves better results even with a smaller number of samples
- Joint training performs better than IC but worse than MMGiC
- Training on IC first then MMGiC gives significantly better performance
- Doing joint training on both, then training on MMGiC leads to the best average performance (MMGiC & IC will refer to this)
## Evaluation
### Comprehension
- MMGiC & IC has the best performance
- MMGiC outperforms IC in terms of in-depth understanding of common concrete concepts
- IC outperforms MMGiC on benchmarks that require a broader understanding of concepts
### Generation
- MMGiC outperforms both
- Likely due to the noise created by IC not being well allieviated by SFT
## Impact of granularity
- Course grained: C only
- Fine graned: LDR
- Multi-grained: CLDR
### Quatitative
- FG provides deeper understanding of concepts than CG
- MG fully integrates each granularity's strength
### Qualitative
- Deeper understanding of concepts allows FG trained MLLMs to more accurately identify visual details and concepts
- CG provides better holistic understanding of the environment
  - FG has a tendency to overfocus on details and miss the forest for the tree
- For harder images, MG can leverage the strengths of FG and CG to get better performance

# Notes
## IC dataset
- It is based on the datasets BLIP-Captions, CapsFusion, LAIN-COCO-Aesthetic and JourneyDB
- Some amount of data filtering is done following LLaVA and then caption synthesis is done for the images
- IC data has the following format
  - Image: [image]
  - Caption: [caption_coco]
  - Detailed caption: [caption_capsfusion]
- Helps improve performance as using **high-quality** data **later** in pre-training seems to lead to better performance


  
[original article](https://arxiv.org/abs/2506.20920)
