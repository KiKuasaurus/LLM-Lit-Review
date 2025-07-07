# Goal of the paper
Test the hypothesis that adding bounding boxes to ground text to images will improve performance

# Main points of research
- Set up a pipeline that is able to generate a large amount of clean usable data that has bounding boxes
- Tune a model with that data and test the results
- Enable the user to point to objects in images directly using bounding boxes
- Enable he model to respond with visual answers 

# Findings
## Multimodal Grounding
### Phrase Grounding
- The model outperforms finetuned mmodels while not involving prior designs
### Referring Expression Comprehension
- Significantly outperforms other zero shot models
- Outperformed by finetuned models
  - Likely due to the way the test refers to objects
## Multimodal Referring
- Outperforms finetuned model
- Fewshot demonstrations improve performance
## Perception Language Tasks
- Similar to KOSMOS-1 that does not have grounding capabilities
## Language Tasks
- Similar performance to KOSMOS-1 in most tests
- decrease on CB
- improvements in BoolQ and COPA


# Notes


  
[original article](https://arxiv.org/abs/2306.14824)
