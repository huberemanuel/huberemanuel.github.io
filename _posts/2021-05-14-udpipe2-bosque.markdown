---
layout: single
title:  "Training UDPipe 2 on Bosque treebank"
date:   2021-09-05 14:37:00 -0300
categories: dependency-parsing
---

# Training UDPipe 2 on Bosque treebank

This tutorial is a sequence in a previous [tutorial](https://huberemanuel.github.io/parsing/udpipe2/) that shows how to install UDPipe 2.
In this tutorial, we are going to train UDPipe 2 on the Bosque treebank, which is a Brazilian Portuguese treebank annotated from news articles firstly created in 2008, and by 2016 was ported into Universal Dependencies format.

## Downloading the dataset

Bosque is available at [Github](https://github.com/UniversalDependencies/UD_Portuguese-Bosque), and currently, it's on the 2.8 version.

```bash
cd ~
git clone https://github.com/UniversalDependencies/UD_Portuguese-Bosque.git
```

## Creating .npz files

The next step is to create `.npz` files need for training by running the `udpipe/scripts/compute_embeddings.sh` script by running:

```bash
cd ~
./udpipe/scripts/compute_embeddings.sh ~/UD_Portuguese-Bosque
```

> If you get any error regarding package version check if you are on the virtual environment of `wembeddings_service` as mentioned in the previous tutorial.

## Training UDPipe 2 on Bosque

Now you should be able to run the entire training process with the following command:

```bash
cd ~/udpipe
python3 udpipe2.py my-model --train ~/UD_Portuguese-Bosque/pt_bosque-ud-train.conllu \
                            --dev ~/UD_Portuguese-Bosque/pt_bosque-ud-dev.conllu \
                            --epochs 8:1e-3,8:1e-4
```

The resulting model will be saved inside `udpipe2/my-model` folder. 

## Evaluating trained model

Firstly, make the predictions on the Bosque test set:

```bash
cd ~/udpipe
python3 udpipe2.py my-model --predict --predict_input ~/UD_Portuguese-Bosque/pt_bosque-ud-test.conllu --predict_output my-model-test.conllu
```


To evaluate your model on the gold standard Bosque test set, run the following command:

```bash
cd ~/udpipe
python3 udpipe2_eval.py ~/UD_Portuguese-Bosque/pt_bosque-ud-test.conllu my-model-test.conllu --verbose

```
The output should be similar to:

```bash
Metric     | Precision |    Recall |  F1 Score | AligndAcc
-----------+-----------+-----------+-----------+-----------
Tokens     |    100.00 |    100.00 |    100.00 |
Sentences  |    100.00 |    100.00 |    100.00 |
Words      |    100.00 |    100.00 |    100.00 |
UPOS       |     90.92 |     90.92 |     90.92 |     90.92
XPOS       |    100.00 |    100.00 |    100.00 |    100.00
UFeats     |    100.00 |    100.00 |    100.00 |    100.00
AllTags    |     90.92 |     90.92 |     90.92 |     90.92
Lemmas     |    100.00 |    100.00 |    100.00 |    100.00
UAS        |      0.00 |      0.00 |      0.00 |      0.00
LAS        |      0.00 |      0.00 |      0.00 |      0.00
CLAS       |      0.00 |      0.00 |      0.00 |      0.00
MLAS       |      0.00 |      0.00 |      0.00 |      0.00
BLEX       |      0.00 |      0.00 |      0.00 |      0.00
```
