# Goal of the paper: 
Create a dataset that consists of many diverse languages

# Main points of research: 
- Setup a good pipeline that allows for better collection and cleaning of data for less resource rich languages
- Pipeline should be able to be automatically adapted to fit other languages well
- 

# Findings:
- builds upon the orginal fineweb research for the general strcuture of the pipeline (filtering, deduplication, more fine grained filtering)
- Used simple character level n-gram models to classify languages
  - calculates threshold for each language by using standard deviation and median
  - helps to account for differences in prediction confidence between languages
- Deduplication is basically the same as FineWeb but performance varies between languages
- Filtering
  - Stopwords are used like in english, but more work had to be done to remove english from the list of stopwords
  - precision filtering was used to ensure that the data isn't too contaminated by another language that is similar but different
  - Weights were assigned to data removed during dedup so that good quality data can be added back through rehydration to improve data quality

  
[original article](https://arxiv.org/abs/2506.20920)
