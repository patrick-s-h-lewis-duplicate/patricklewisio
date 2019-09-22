+++
# Date this page was created.
date = 2019-04-27T00:00:00

# Project title.
title = "LAMA"

# Project summary to display on homepage.
summary = "LAMA ia a probe for analyzing factual and commonsense knowledge in language models. "

# Optional image to display on homepage (relative to `static/img/` folder).
image_preview = "projects/lama3.png"

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = ["language-model","evaluation"]

# Optional external URL for project (replaces project detail page).
external_link = ""

# Does the project detail page use math formatting?
math = false

# Optional featured image (relative to `static/img/` folder).

[header]
image = "projects/lama3.png"

+++
LAMA ia a probe for analyzing the factual and commonsense knowledge contained in pretrained language models.
The LAMA probe was built to run experiments for our paper ["Language models as Knowledge-bases?"](/publication/lama/) by my colleague Fabio Petroni. 
LAMA was open-sourced as a stand-alone probe for assessing how factual Language models are.

The codebase for the LAMA probe is available [here](https://github.com/facebookresearch/LAMA), 
and the dataset is available at [here](https://dl.fbaipublicfiles.com/LAMA/data.zip).

LAMA contains a set of connectors to pretrained language models.
LAMA exposes a transparent and unique interface to use:

*  Transformer-XL (Dai et al., 2019)
*  BERT (Devlin et al., 2018)
*  ELMo (Peters et al., 2018)
*  GPT (Radford et al., 2018)
*  RoBERTa (Liu et al., 2019)
