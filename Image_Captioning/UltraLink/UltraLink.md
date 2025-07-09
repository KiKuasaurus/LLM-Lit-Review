# Goal of the paper
Construct an open-source multilingual SFT dataset

# Main points of research
- Introduce a knowledge grounded data augmentation approach to elicit more language-specific knowledge of LLMs
- Find ways to prune the dataset without performance loss to improve efficiency
- Combat the low cultural diversity and mprecise translations caused by cultural differences
- Linearly increase data volume instead of exponentially increase it with each additional language

# Findings
## Data
### Language agnostic data
- It is not necessary to re-train the model on the same math/coding questions in multiple lannguages
- In fact they faced a reduction in performance at a point
### Distribution
- Focuses on 4 types of SFT data
  - chat data with language specific knowledge
  - chat data with language agnostic knowledge
  - math data
  - code data
- Compared to other data sets Ultralink has more turns on average, longer answers and shorter questions relative to the answer length
### Results
- Guanaco perform better in english OMGEval, but much worse in every other language
- UltraLink has significantly better perfromance in HumanEval and MGSM in all languages

# Notes
## Data Curation
### Language specific curation
- 2 main steps
  - Prepare and sample knowledge from knowledge bases as cultural backgrounds
  - Steer LLMs to generate informative conversations based on the backgrounds
### Knowledge preparation
- Use wikipedia dumps for localized knowledge base
- Use fastText to remove non target language data
- Use OpenCC for chinese to convert traditional text to simplified
- Filter out documents that are too short or too long (<1k and >10k)
- Divide text segments into lengths that are 1-2k tokens long
### Dialogue Generation
- Use prompts with GPT 3.5 to generate conversations
- Randomly steer between in depth and expansive questions


  
[original article](https://arxiv.org/abs/2306.14824)
