# Comparisons of LLMs to Reinforcement Learning Models on File Fragmented Datasets 

#### As technology continues to improve along with the threat of cybercrime, it is important that we adapt and find strategies to prevent cybercrime from occuring. In this repo, I analyze two forms of ML in order to determine which one will be the most useful in Digital Forensics:

- ###### Large Language Models (LLM)
- ###### Reinforcement Learning Models (RL)


## Data Preprocessing

#### I modified the original file fragmented datasets to scramble the name, as the names already revealed the label. Then from there I created a mapping to the original file type and file name in mappings.json.


## Methodology
#### I chose BERT as the LLM. Being an encoder-only model, it generates embeddings from the input (image, files,etc), which can then be passed through a classification layer to produce the final label. This architecture is ideal for my task, since the objective is classification rather than text generation. The embeddings are the numerical representation of the input, which allows the model to process the information.

#### For the RL task, I choose a Deep Q Network

### Labels 
- #### BMP  
- #### BPG  
- #### GIF  
- #### JPEG  
- #### JPF  
- #### JXR  
- #### PNG  
- #### TIF  
- #### WEBP  

### Metrics
- ###### Confusion Matrix
- ###### F1 Score


## Results

### Bert (LLM)
- ###### F1 Score: 
- ###### Confusion Matrix

### Bert (LLM)
- ###### F1 Score: 
- ###### Confusion Matrix


### File Fragmented Dataset(Original): https://bmcresnotes.biomedcentral.com/articles/10.1186/s13104-019-4812-0

### File Fragmented Dataset (modified by me): https://www.kaggle.com/datasets/danielanoruo/file-fragment-classification/data?select=mappings.json

### BERT Implementation: https://arxiv.org/abs/1910.10781

### Deep Q Network Implementation: 