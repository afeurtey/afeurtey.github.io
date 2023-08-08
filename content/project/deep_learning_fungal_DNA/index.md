---
title: "Using deep learning to unveil the language of fungal DNA"
date: 2023-08-02
tags:


summary: "."

image:
  preview_only: true
  alt_text: AI generated image of fungi and plants in and around a computer
---

![Main question motivating my first deep learning project](/images/NN_00_Main_question.png)

Fundamentally, biology can be thought of as an information processing system. This system transfers information from one generation to the next imperfectly but while maintaining its core structure and functionality. Taken down to its core elements, biology can thus be conceptualised in an information science framework. Furthermore, biological information is transferred in an organized manner, using common patterns of only four letters to form a complex language. Deciphering this genetic grammar is still one of the biggest challenges in biology today. Some DNA grammar elements exhibit near-universality across the tree of life, such as the codon "ATG" (encoding a methionine) to mark the start signal of a gene in most organisms. However, while such common signals as start/stop codons exist, many evolutionary lineages have adopted specific language elements, contributing to the diversification and adaptation of organisms. Such specificities vary in how evolutionary recent they are and thus how clade-specific they appear. Although coding sequence exhibit such patterns (such as with codon optimization for example), this is particularly true in the non-coding parts of the genome.  

In the last years, new sequencing technologies have promoted the production of a deluge of new -omics data, allowing for tremendous progress in various fields such as genomics, transcriptomics or epigenomics. However, until recently finding methods to synthesize the information contained within such large amounts of data has been near impossible. I propose to take advantage of the groundbreaking advances in artificial intelligence (AI) to mine the huge amount of available -omics data and provide new insights into old standing biological questions. In particular, I want to apply the latest development in Deep Learning (DL) and Natural Language Processing (NLP) to investigate the universality and specificities of the genetic language. 

As a first step toward answering these questions, I tested whether nucleotides can be successfully predicted in fungal genomes with a neural network. I used a simple convolution neural network (CNN), including 25 layers and previously developed to do prediction on *Arabidopsis thaliana* by [Gonzalo et al.](https://github.com/songlab-cal/gpn/tree/main). Initial tests showed that the CNN trained on Brassicales genomes was unable to predict fungal sequences, so I trained the model again using 25 genomes of Pezizomycotina species, a clade representing a large part of the fungal diversity but excluding budding yeasts. The model trains following the logic of a masked language model and learns to predict each nucleotide identity based on the sequence context. 

![Training data](images/NN_01_Training_data.png)

The original CNN was trained on several Brassicales including on *A. thaliana* and the predictions were done on the same species (specifically on an unseen chromosome). My goal of using deep learning to learn the DNA language of fungi requires that the knowledge learned on a species ensemble be transferable to other related species. Consequently, I chose to test the model on 9 Pezizomycotina which were never seen during the training of the model. The prediction quality was generally similar between sequences from the seen and unseen species, indicating that the model successfully learned transferable patterns from the initial genomes.

In neural networks, embeddings are the learned representations of the processed inputs (here, corresponding to DNA sequences) and they should map similar objects close to each other in the embedding space. If the trained CNN has successfully learned the underlying structure of the DNA sequence as suggested by the comparable results between seen and unseen species, the embedding vectors should reflect the known origin of the sequences (i.e. CDS or non-CDS for example). We analysed the embeddings for 100bp sequences using a dimensionality reduction method (UMAP) and indeed showed that sequences from CDS, intronic sequences and repeats were clustering together. 

![UMAP embeddings](images/NN_02_Embeddings.png)

For each position, the model can output probabilities for each possible base (A, T, C, G). Based on these probabilities we can compute a prediction score that correspond to the probability of the correct base divided by the average of the prediction for the three other bases. Represented along a sequence in the genome this score reveals which positions are easy to predict (highly negative). In a random gene example from *Zymoseptoria tritici* (see panel A of the figure below), we observe that the start and stop codon are predicted correctly with a very high probability. Other easily predicted positions include slicing sites at intron/exon borders. This is confirmed by the values observed across the sequences of 849 genes (panel B).

![Nucleotide predictions](images/NN_03_Prediction_in_gene.png)

Taken together these results indicate that a simple CNN is able to successfully learn the basics syntax elements of the fungal DNA language. These preliminary results provide hint that even such a simple model could be used to predict genes in newly sequenced non-model species, for example, and suggest that more complex syntax elements could be uncovered such as promoter sequences could be identified in fungal genomes using deep learning. Future directions of research also include classifying repeat elements as preliminary results suggest that sequences belonging to the same TE families cluster together in the embedding space. 