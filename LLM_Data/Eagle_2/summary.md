# Goal of the paper
Address VLM post training from a data-centric perspective

# Main points of research
1. Come up with a data collection strategy that allows one to collect a large scale and diverse data pool
2. Come up with a data filtering strategy to remove low quality samples
3. A data selection strategy to construct high-quality subsets
4. A series of data augmentation techniques to enrich the existing data

# Findings
## Three staged training recipe
- Augment the 2 stage training strategy from LLaVA to improve its performance
- An intermediate stage (Stage 1.5) is used to train the full model on a large-scale, diverse dataset
- Due to the fact that when moving to stage 2 from 1, the model is unsuitabkle for quick SFT data updates
- Stage 1.5 allowed the model to be pre trained on a larger dataset to reduce dependency on SFT data
### Re-updating stage 1.5
- Reupdating stage 1.5 with data from stage 2, improves scores on ChartQA, MMVer and mathVista
## Data collection
### Key strategies
**Passive gathering:** Data is gathered from arXiv and HF before being filtered  
**Proactive searching:** By figuring out what is missing from the model through error analysis, targeted web searches can be used to fill the gaps in data.  
Large amounts of non-QA data is also collected and labelled
### Filtering
- A similarity score is used to preserve diversity  
- The following types of low quality data is also filterered:
    - *Mismatched QA pairs*
    - *Irrelevant image-question pairs:* The image and the question are not related
    - *Repeated texts:* Generally hallucinated content
    - *Numeric formatting issues:* Numbers that are not rounded off appropriately
    - *Questions with the same answer:* Some are due to ethical or safety issues, but others are just poor quality "Sorry, I cannot", and so a keyword-based filter is used to exclude these
- Larger datasets are generally given smaller samples as they are often synthetic and may contain errors or lack diversity
- K means clustering is used to ensure an even sample of different types of data
- Datasets with <20k samples do not get filtered
- Subset selection requires the removal of at least halfof its data
### Augmentation
Third party VLMs are used to generate fine grained descriptions of the image. The following is usually done:
- Adding CoT explainations
- Rule based QA generation
- Expand short answers into longer responses    

For CoT augmentation, multiple LLMs are used to compare generated answers to the original answer to filter out erroneous samples.  
- If a model is not trained on CoT data, it will not be able to give answers in a CoT format, and asking it to think might reduce answer accuracy.
- Gives better performance on MMMU and MathVista
### Formatting
The following is done to improve data formatting.
- *Remove unnecessary decorations* like the beginning and end of math for LaTeX formatting
- *Add more specific instructions:* appending "provide a short answer" or "please answer yes or no" helps to improve answering quality, but it should not always be done or it will hinder a models ability to answer questions when such prompts are not given
### Packing
- Packing is used to accelerate trainig speeds
- Balance-aware packing provuides a more even distribution that improves data quality
