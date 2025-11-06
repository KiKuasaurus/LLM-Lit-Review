# Goal of the paper
Make an OCR LLM that has better awareness and comprehension of the reading order as well as semantic classes

# Main points of research
- Develop a model that can extract text and semantic classes, provide bounding boxes and preserve reading order
- Develop a data generation pipeline using a modified LaTeX compiler
- Create a benchmark DROBS with human-labeled data that can show SOTA accuracy 

# Findings
## Architecture
- Uses a vision encoder initialized from RADIO which follows VIT-H/16
- Uses a decoder mBART to predict text tokens
- Uses input prompt to modify the output with 8 valid combinations
- pretrained on a custom dataset that has labels for the maximal-information setting, which is then decreased for later groups in the fine tuning stage
- Repetition penalty of 1.1 was applied during inference
## Data
### arXiv-5M Dataset
- Modified the open-source TeX Live distribution to generate high-quality ground truth dataset with ~5M pages

## Results
### Reading order benchmark
- Pages sampled from Common Crawl corpus for DROBS
- annotated by humans
- Reading order is included as a signficant addition
- eclair outperforms both Kosmos and GOT on most metrics
- Kosmos 2.5 performed better in OCR mode, while GOT performed better in md mode
### Other benchmark
- Outperforms GOT in some metrics, while using a smaller decoder
- Good extraction quality, but still better for text than tables or formulae
- While it is inherently disadvantaged due to architectural reasons, it still performs competitively on Document Object Detection benchmarks
### Multi-token Inference
- Most OCR models are slow as they predict tokens incrementally
- By using n-token prediction, speeds can be increased significantly
- 2 or 3 token inference actually outperformed 1 token inference in some metrics
- however at 4 there was significant degredation
- 2 was chosen, resulting in a doubling of generation speed
