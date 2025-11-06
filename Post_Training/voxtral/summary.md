# Goal of the paper
Tune and build a multimodal audio chat model

# Main Points of research
- Create 2 open weights sota audio models with transcription and multilingual speed understanding
- Native function calling supoort with audio
- New evaluation benchmarks

# Findings
## Model
Consists of an audio encoder, an adaptive layer to downsample audio embeddings and a language decoder to reason and generate text outputs.

## Methodology
Pretrained with a 50/50 mix of audio transcript pairs and audo text cross-model continuation patterns. Text pretraning data is also included to preserve text capabilities.
- Only the Adapter is trained in the first pass as the warm up has been found to be beneficial for speed understanding evaluations.  

SFT was done with a mixture of audio context + text query pairs as well as audio only questions that can be answered with just contextual kowledge.  
DPO was done with the same reward infrastructure as Magistral, where 2 candidate responses are taken and their transcripts are evaluated.

## Additional benchmarks
### Speech synthesized benchmarks
- Created speech-synthesized versions of GSM8K, TriviaQA and MMLU
- filtering was done on the prompts so only speech viable ones are kept. Can be split into 3 categories
   - *Verbalizable:* no changes needed
   - *Verbalizable with rewrite:* contains markdown or latex, can be re written into text form and then verbalised.
   - *Non-verbalizable:* Tables/figures/lengthy math or code, cannot be spoken and hence is discarded
- Text synthesized with a random sample of speaker embeddings
- No change for scroing as generations are in the text-space
### Speech Understanding (SU) Benchmark
- Use an LLM as a judge. LLM is provided with transcript, question, reference answer and proposed answer
- LLM returns 2 complementary metrics
   - `LLM_JUDGE_SCORE` binary helpfulness indicator. 1 if correct and helpful, else 0
   - `GRADE_LLM_JUDGE_SCORE` a 0-5 quality grade. 0 means bad, 5 means good
- multiple runs are done each time to capture sampling variability

## Results
### Speech recognition
- Voxtral Small achieves SOTA performance
- Outperforms competitively with GPT-4o mini Transcribe and Gemini 2.5 Flash across all tasks
### Speech Translation
- Also outperforms both closed sourced models!
- Whisper only supports X -> en, so no comparisons for that
### Speech Understanding
- Performs competitively with closed soruce models
- Beats GPT-4o mini in 3/7 tasks (Openbook QA, MMLU speech, SU Benchmark)
- Beats Gemini 2.5 flash in Llama Questions
### Text benchmarks
- Performs quite close to Mistral Small 3.1 on most benchmarks

## Analysis
### Padding
- Disabling padding incurs no penalty in FLEURS english and llama QA, but results in a 0.5% WER degradation in French, so it is kept
### Adapter Downscaling
- Little degradation on ASR benchmarks for english
- At 8x downsampling (6.25Hz) there is a penalty of over 1% for French FLEURS
- 12.5Hz surpasses the 50Hz baseline for LlamaQA, suggesting that at that frame-rate, the audio encoder likely encodes similar amounts of information to the text decoder, whcih gives better performance
### Pretraining patterns
- If only one type of data is used (ASR/Interleave) the benchmark for the other will degrade significantly
- However when data is mixed at a 50/50 ratio, it performs similarly to using only one type of data. As such a 50/50 mix is used
### SFT vs DPO vs Online DPO
- For both models, online dpo performs better than offline dpo
- However for voxtral small, it is accompanied by a slight regression on the english short form benchmarks