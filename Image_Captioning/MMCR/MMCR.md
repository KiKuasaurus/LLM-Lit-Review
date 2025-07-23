# Goal of the paper
Construct a dataset based on multi turn conversations as well as an evaluation benchmark to go along with it.

# Main points of research
- Create a multimodal dataset that is also multi-turn toto enable llms to be better at assessing multi-turn dialogue contextual reasoning abilities
- The "Less is More" phenomenon, where data with poor distribution but greater volume gets outperformed by a smaller dataset with better distribution

# Findings
- Generating data based on a single image and its context leads to hallucination issues.
  - Due to the lack of semantic information, GPT-4o tends to engage in artistic creation
  - Limit single image data to 4 turns and allow up to 8 turns for multi-image data
- Biased Phenomenon
  - When additional data reaches a certain proportion of the total, the performance on public benchmark declines
  - maintaining a comprehensive and balanced data distribution is more important than dataset size



# Notes
## Dataset construction
### Data collection
- Dataset has to be kept consistent otherwise generated dialogue will become too random
- OmniCorpus-CC-210M already has interleaved data, so clustering is not needed
### Generation pipeline
- Uses GPT-4o
- “Ensure that each turn of the dialogue progressively
deepens the exploration of image details, connections be-
tween images, and related topics. The dialogue must main-
tain clear contextual continuity, with subsequent questions
based on previous requests or questions and their corre-
sponding answers. At the same time, the AI assistant’s
responses must be detailed and contextually relevant, fo-
cusing only on observable details directly inferred from the
images, avoiding descriptions of fictional content or intro-
ducing creative interpretations.”
- Strictly limit the dialog style in the prompts
- Stipulate that the special token marking each image only appears once at the beginning of the first dialogue the image is mentioned.
- Use CLIP to filter out irrelevant data
### Data statistics
- 4 dialogue turns for single image, 8 for multi image
- each entry has 2-4 images with a 61.5/27.6/10.9 split
- All data folows  strict alternating human-AI assistant dialogue mode, with each dialogue turn closely related to the image.
## Benchmark construction
- Probably not relevant, refer to annotations in pdf

[original article](https://arxiv.org/abs/2503.18533)
