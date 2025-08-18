# Comparisons of LLMs to Reinforcement Learning Models on File Fragmented Datasets 

#### As technology continues to improve along with the threat of cybercrime, it is important that we adapt and find strategies to prevent cybercrime from occuring. In this repo, I analyze two forms of ML in order to determine which one will be the most useful in Digital Forensics:

- ##### Large Language Models (LLM)
- ##### Reinforcement Learning Models (RL)


## Data Preprocessing

### To prevent data leakage from file names, the original fragmented dataset was modified by scrambling the filenames. This ensured that the names themselves did not reveal the corresponding file type label. A separate mapping file, mappings.json, was then created to preserve the relationship between each scrambled filename and its original file type and filename. This allowed the dataset to remain anonymized during training, while still enabling accurate label retrieval when necessary.


## Methodology
#### I chose BERT as the LLM. Being an encoder-only model, it generates embeddings from the input (image, files,etc), which can then be passed through a classification layer to produce the final label. This architecture is ideal for my task, since the objective is classification rather than text generation. The embeddings are the numerical representation of the input, which allows the model to process the information.

#### For the RL task, I choose a Deep Q Network

### File Types 
#### BMP (Bitmap), BPG (Better Portable Graphics), GIF (Graphics Interchange Format), JPEG (Joint Photographic Experts Group), JPF (JPEG 2000), JXR (JPEG Extended Range), PNG (Portable Network Graphics), TIF (Tagged Image File Format), WEBP (Web Picture)

### Metrics
- ##### Confusion Matrix: A table that summarizes the performance of a classification model by comparing predicted labels to actual labels.
- ##### F1 Score: A metric that balances precision (how many of the predicted positives are correct) and recall (how many of the actual positives are correctly identified)

### Classification Task
- #### Originally Planned to planned to do a multiclassification task, but switched it to Binary Classification. (Legacy File Fragments vs Modern File Fragments)

### Labels
- #### Legacy (Image formats through the early 2000s)
- #### Modern (Image formats from 2010s-Present)
### 1 For Legacy, 0 for Modern

## Results


### Bert (LLM)
- ##### F1 Score: 
- ##### Confusion Matrix

### Deep Q Networks (RL)
- ##### F1 Score: 
- ##### Confusion Matrix


### File Fragmented Dataset(Original): https://bmcresnotes.biomedcentral.com/articles/10.1186/s13104-019-4812-0

### File Fragmented Dataset (modified by me): https://www.kaggle.com/datasets/danielanoruo/file-fragment-classification/data?select=mappings.json

### BERT Implementation: https://arxiv.org/abs/1910.10781

### Deep Q Network Implementation: https://www.sciencedirect.com/science/article/pii/S1474034619305506