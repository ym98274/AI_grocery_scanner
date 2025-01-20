# AI_grocery_scanner

This repo provides the code for developing and running complete end to end AI grocery scanning system.

## How the system works
This system allows users to scan grocery items and add these to a basket which is updated dynamically. 
Scanning involves moving a grocery item from the left to the right portion of the screen.
In order for this to work, users must have a camera and this must have access to frames being captured.

The system was trained with a very limited sample of training data; only 150 samples of training data were used per class. Users interested
in developing a complete AI grocery scanning system are encouraged to use the model weights provided in this repo.

## How the system was developed

1. Data generation
   
There are currently no publicly available grocery item datasets with sufficient depth and quality. Thus it was necessary to scrape the data from grocery stores
in order to fine-tune and then evaluate the YOLOv8 model. This involved using web crawlers to identify and then download thousands of relevant image and text pairs. To assist with this, the BrightData api was integrated into the scraping code.  

2. Data processing/preperation

A number of preprocessing steps were implemented with respect to the scraped image/label pairs. 

The raw text descriptions contained tokens such as quantities and brand names which limited the effectiveness of the clustering process that followed.
To optimise this, the meta Llama-3.1-8B-Instruct model was used to reduce description to their most essential form. The processed labels were then passed to
the thenlper/gte-small model to generate vector embeddings. The dimensionality of these text embeddings was reduced by applying UMAP. 

Processed embeddings were clustered using HBSCAN to identify grocery item classes that could be used to build a reliable YOLO object detector. 
Topic modelling was finally applied to identify overarching class descriptions for each cluster using the bertopic library.

The scraped images also required processing. This consisted of applying image enhancing techniques together with background removel.
Fine-tuning the YOLO agorithim requires that a purpose built dataset is generated. Specifically,  
   
4. Model training/ evaluation
The efficacy of this system was first tested on pre-recorded videos where grocery items were displayed. Once the system proved effective, testing
and fine-tuning was undertaken to achieve similar performance on realtime video streams.

6. Model integration

## System demonstration
For a concise demonstration of this system in action, please visit https://www.ymageit-ai.com/groceryvisionapp 

## Code and Data
The core scripts necessary for following the system development section of this repository is provided together with inference script.

The generated and processed data easily exceeds 10GB but be made available on request!

