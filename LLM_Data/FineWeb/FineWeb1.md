# Goal of the paper
Make an open sourced public dataset that outperforms the current options

# Main points of research
- Apply different deduplication methods on their granuarities and how it impacts performance
- Compare and figure out what heuristics are the most impactful

# Findings
- cleaner data in WET format from Common Crawl is significantly worse than extracting from WARC data
- for deduplication, doing deduplication for each snapshot counterintuitively produced better results than doing a full sweep of the whole dataset
  - this was due to the fact that it had a larger percentage of junk data
- Applying heuristic based filters to remove more poor quality data provides very significant boosts to performance
- Additional heuristic filters were developed by comparing the poor quality full minhash data with the better independent minhash data
- Llama-3-70B-Instruct was then used to rank the value of the data points in order to create an education quality classified dataset

[original article](https://arxiv.org/abs/2406.17557v1)