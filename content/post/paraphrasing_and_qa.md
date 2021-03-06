---
title: "Aspects of Paraphrasing for Adversarial training and Regularization in Question Answering"
date: 2018-10-23T11:54:07+01:00
draft: false
tags: ['PhD Blog', 'QA', 'Review']
---

Welcome to my first *real* blog post. Read more about what it's all for [here](/post/blog_intro/). As a reminder, this is mainly a tool for me to organise my time and thoughts. These posts are not going to be infallible pieces of academic writing, (they're not papers and shouldn't be judged as such!) but friendly constructive feedback is welcome! Also, I expect to amend these pieces from time to time too.


Over the last couple of weeks I’ve been thinking about how to make neural QA systems behave more consistently. Paraphrases are particularly interesting here because, an (ideal) paraphrase of a chunk of natural language should preserve the meaning of that language exactly, but can be written quite differently. Systems that make predictions about natural language should (ideally) be invariant to paraphrasing one or more of their inputs.


This document explores some recent work in how paraphrasing has been used to improve the performance in question answering systems and related natural language tasks. The aim of the document is to survey what work has been done, what the trends are and maybe to tease out some gaps or room for improvement. This is not an exhaustive review, and maybe I’ll add to it as time goes by (suggestions welcome).


## Paraphrase datasets and generation techniques

Before diving in, we should quickly look at what paraphrase datasets are available and what techniques exist for generating paraphrases. There are several paraphrase datasets available, some curated, some automatically generated, and others more focused on linguistic phenomena. We can split them into 3 rough categories:

1. Generated from wiki type sites where human users have tagged questions to be duplicates of other ones. This covers:
	* The Quora dataset from a kaggle paraphrase detection task ([here](https://www.kaggle.com/c/quora-question-pairs/data))
	* WikiAnswers question-paraphrase dataset (part of paralex) ([here](http://knowitall.cs.washington.edu/paralex/))
2. Manually annotated datasets:
	* The Microsoft Research Paraphrase Corpus ([here](https://www.microsoft.com/en-us/download/details.aspx?id=52398))
	* The DIRT Paraphrase Collection ([here](https://aclweb.org/aclwiki/DIRT_Paraphrase_Collection))
	* P4p - Paraphrase for Plagiarism dataset ([here](http://clic.ub.edu/corpus/en/paraphrases-en))
	* SemEval 2017 Task 3 ([here](http://alt.qcri.org/semeval2017/task3/))
3. Automatically generated datasets like
	* PPDB - an automatically constructed dataset of paraphrases generated by phrase-based back-translation ([here](http://paraphrase.org/#/download))
	* Para-nmt-50M - 50 million english paraphrases generated neural machine translation ([here](http://www.cs.cmu.edu/~jwieting/))


These datasets vary wildly in size, characteristics and age. Paraphrasing doesn’t appear to have received the fevered interest enjoyed by NLI, QA or MT, which have vast, high quality datasets. Methods to generate paraphrases are mainly based on Back-translation, i.e. translating a sentence to a “Pivot” language, and then translating it back to to the first language. This is a mature technique, but has become easier and more tunable with the arrival of NMT. 

There are other techniques too, from simple synonym swapping and to phrase based swaps and parse tree based re-structuring of sentences.

Bringing the focus back to QA systems, we can make some general observations: 


**Question $q$**  : QA tasks always have a question, usually in natural language. 

**Context/knowledge source \\(c\\)**: QA systems also require some sort of knowledge source or contextualisation at some point. This could be:

1. A Knowledgebase
2. A document of natural language
3. A very large information retrieval on the web (which could be considered a special case of 2).

**Answer \\(a\\)**:
A QA system will also have an answer, which could be:

1. generated natural language
2. extracted natural language
3. some sort of multiple choice (ranking given possible answers, which could extend to ranking all the entities in a between possible answers). This could be natural language or encoded entities

So, when thinking about paraphrasing in QA systems, we have the opportunity to exploit them by paraphrasing questions, paraphrasing cases 2 and 3 in knowledge sources, or perhaps even in paraphrasing answers.


## Question paraphrasing and answer aggregation


Question paraphrasing have been exploited in several works. **“Paraphrase-Driven Learning for Open Question Answering” (Faber et al. 2013)** looks at this. They make two important contributions for us

1. They create the WikiAnswers question paraphrase corpus, a large dataset of groups of paraphrased questions. In its unfiltered form, WikiAnswers is very noisy, the authors report that 55% of paraphrase pairs annotated by humans were considered to be true paraphrases
2. Using paraphrased questions to improve the robustness of a QA system. 

Specifically they use wiki-answers paraphrases to induce a pattern lexicon that maps questions to to database queries. They train a model to rank the patterns that apply to a particular questions, and the top ones are used to query a database. The highest scoring answer is then returned. 

We could interpret their approach at a (very) high level as:

1. **From an initial question, “Generate” paraphrases** (In their case generating paraphrases means mapping the question to other questions that have a known database query, via patterns)
2. **From all the paraphrases, “generate” answers** (in their case, this means executing all the database queries that were mapped to the input question)
3. **Score the possible answers and return the best** (in their case, they actually directly score all the (question, matched pattern) tuples but patterns have a 1:1 mapping to the DB query, and the database query is a proxy for the answer)

This technique of “generate paraphrases, get answers and ensemble” is quite similar to query expansion techniques in information retrieval. The hope is that at test time, a paraphrase of a question can be generated that the QA system is better at understanding, and will result in the right answer.

Another paper worth mentioning at this point is **Semantic parsing via paraphrasing (Berant and Liang 2014)**. They firstly use a deterministic process to generate many logical forms for an inputted question. These logical forms correspond to database queries, but can also be translated into “canonical utterances”, i.e. potential paraphrases of the input question. These potential paraphrases are ranked by a paraphrase detection model to the original input question, and the highest scoring canonical utterance’s logical form is translated into a Sparql query to get an answer.


We can see the “generate paraphrases, get answers and ensemble pattern” that we identified for Faber et al. 2013 in action in two recent works:  
**Learning to Paraphrase for Question Answering** (Dong et al. 2017) fits our framework nicely:

1. Generate paraphrases \\(q_i\\) from question \\(q\\) using back-translation. 
2. Each paraphrase is used to generate an answer using a neural question answering model, \\(p(a | q_i)\\)
3. Each paraphrase is also scored using a neural paraphrase scoring system \\(p(q_i|q)\\). The answers are scored \\(p(a|q) = \sum\_{i} p(a|q_i) p(q_i | q )\\)

Rather neatly this framework doesn’t actually require paraphrase data to train, only a system that can build paraphrases. They test it on various small, KB based QA datasets (webquestions, GraphQuestions, wikiqa) but I haven’t seen this approach applied as-is to newer, larger datasets.

**Ask the right questions : active question reformulation with RL (Buck et al. 2018)** take a similar approach, but use reinforcement learning to train an agent.

1. An agent learns to paraphrase a question to maximise the probability of a black-box QA model giving the right answer 
2. The black-box QA system generates an answer for each paraphrase written by the agent
3. The agent aggregates the all the answers into a final answer. 

An interesting facet of this work is that you can inspect the paraphrases that the agent learns to write. The authors note that they get less natural and even learn to mimic keyword search and other IR techniques

These models seek to fix a problem with QA models, that paraphrased questions can give different answers. But the solution isn’t very efficient, as at test time, paraphrases must be generated and the QA system must be queried many times for one question. In essence, they are trying to reduce the impact of errors from a bad QA model rather than fixing the QA model itself.


## Adversaries for data augmentation

An alternative approach would be to identify when the system gives inconsistent results, and then to augment the training so that this behaviour is corrected. This adversarial paraphrase identification / generation is the focus of several works. Again, we can identify a 3 step high level process:

1. Generate paraphrases of a question
2. Generate answers to the paraphrases, collect those that have different (wrong answers)
3. Augment the training data with these adversarial examples to increase robustness

This pattern can be seen in **Semantically Equivalent Adversarial Rules for Debugging NLP Models (Ribeiro et al. 2018).** The authors introduce the concept of a Semantically Equivalent Adversary (SEA) which, for our purposes, is a question paraphrase that causes a QA system to give a different answer. They define a semantic equivalence metric based on the probability of generating a paraphrase using back-translation and require a SEA to have a semantic equivalence score to be above a certain threshold and to “fool” a system under test. They use these SEAs to identify rules (SEARs) that can be interpreted by a human as systematic ways that the QA system fails. These SEARs are then used as data augmentation, i.e. For an existing datapoint \\((q, c, a)\\), a rule can be used to create a new training datapoint \\((q’=r(q), c, a)\\). 

The authors of **Adversarial Example Generation with Syntactically Controlled Paraphrase Networks (Iyyer et al. 2018)** use large paraphrase datasets to train a system that can translate a (sentence, target parse tree structure) pair into paraphrase of the sentence with the specified grammatical structure. They use a similar approach to Ribeiro et al. to create adversarial examples, this time for NLI models, by taking a sentence, and generating syntactically-controlled paraphrases until the NLI model’s prediction changes. The authors needed vast numbers of paraphrases to train their paraphrasing system. They used the automatically generated ParaNMT-50M dataset. They note that only about 65% of the dataset contained true paraphrase pairs (compared to 55% for WikAnswers. It seems that there may be a need for cleaner paraphrase datasets)


## Inconsistency losses

An alternative (but not mutually exclusive) approach would be to to directly train a model to give sensible answers when it encounters paraphrases, rather than just augment the training data. This would involve directly optimising some kind of auxiliary loss to make the model robust. 

This approach has been explored in RTE tasks in **Adversarially Regularising Neural NLI Models to Integrate Logical Background Knowledge (Minervini and Riedel 2018)**. Here, the  authors define an inconsistency loss that measures the extent to which a model violates a logical constraint. A simple example in textual entailment is if statement 1 is predicted to contradict statement 2, then statement 2 should contradict statement 1. This can be quantified and the extent of violation can be directly minimised alongside the main task of minimising the RTE classification loss. The authors go on to perturb inputs by mutating words and parse subtrees, in effect generating “paraphrases” that cause the model to fail. The difference with the two papers above is that the violation can be directly minimised, rather than needing to generate examples that fail and adding them to the training data of the main task.


## Don’t just paraphrase questions

So far we have talked exclusively about paraphrasing questions, but a lot of the techniques could be used to paraphrase different parts of the QA data, as we identified at the start.

A recent highlight in data-augmentation in the QA literature is **QANET: Combining Local Convolution with Global Self-Attention For Reading Comprehension (Adams et al. 2018)**. This paper introduces a number of different techniques, including fast-training, non-recurrent QA architectures, but they also explore data augmentation. They do not paraphrase questions as they are concerned about semantic drift, but instead paraphrase the context document.

They use back-translation to translate each sentence of the context into 25 paraphrases, that are then randomly combined into a new document. If the original sentence contained the answer to the question, the answer span needs to be “relocated” in the paraphrase. This is done by simply choosing the word in the paraphrase with the highest bi-chargram overlap with the first word of the original answer as the new answer span start, with an equivalent process for the span end. This creates new data points \\((q,c,a) \rightarrow (q, c’, a’)\\), which are added to the training set. They do not explore adversaries, but there is potential to do so. 

They find their process to be quite effective, adding 1.5 EM score on Squad (significant given the fierce competition) and claim it makes it more robust on adversarial squad datasets, although they do not publish results for their model without data augmentation for these datasets. The improvements they observe are probably down to the model being able to experience a larger variation of language during training.


## Multi-task learning with paraphrases

A final set of techniques to briefly explore are multitask learning. This approach is related to the concepts of inconsistency losses, but is more general. The strategy is to define a second task that is complementary to the first, generally to encourage a neural encoder to encode information in a more preferable way. This approach has been used in **Question Answering with Subgraph Embeddings (Bordes et al. 2014)**, where the main task of learning a scoring function for a question and answer tuple \\(S_a(q,a)\\) was coupled with an auxiliary task of paraphrase detection \\(S_p(q,q’)\\) where both tasks used the same question encoder \\(f(q)\\). The paraphrase detection task encourages the encoding \\(f(q)\\) and \\(f(q’)\\) to be similar, capturing the “meaning” of the question more effectively.

This approach is also used in **Question Answering over Freebase with Multi-Column Convolutional Neural Networks, (Dong et al. 2015)**, where question encoders are co-trained with a paraphrase ranking task, where a model is trained such that the dot product of a question and a paraphrase representation is larger than for a random non-paraphrase.


## Trends and suggestions:

Summing up, we have identified 3 main techniques for question answering using paraphrases.

1. Generating many paraphrases of inputted questions, generating an answer from each, and aggregating the answers together
2. Data augmentation either from paraphrasing documents, or (Adversarial) generation of paraphrased questions.
3. Multi-task learning, either to encourage higher quality representation learning, or to directly improve the task at hand by introducing an inconsistency loss


As alluded to earlier, many of the techniques have not been fully explored. Most have focused on question paraphrasing due to the availability of large question paraphrase datasets. It would be possible, however to create adversarial documents using paraphrasing, similar to the work, **Adversarial Examples for Evaluating Reading Comprehension Systems (Jia and Liang 2017)** where adversarial sentences are added to squad contexts to confuse models.

For models that learn to rank candidate answers, answers could also be paraphrased, such that \\(f(q, a_1) = f(q, a_2)\\). This avenue appears not to have been explored much at all.

It seems that current QA models fall short of encoding language in a paraphrase-resistant way (that is to say, they are over-reliant on learning simple patterns and word matching that is brittle to rephrasing). That Iyyer et al. 2018 observe their agent’s paraphrases to become less grammatical lends weight to this interpretation. Also, Adams et al. 2018 data augmentation by paraphrasing gives them meaningful performance boosts suggests that models could benefit from exposure to more diverse language and domains. Inspired by work we've reviewed here, I'm going to be experimenting with some new methods to improve the resilience of machine readers. I hope to report back with progress here!

## Citations:

M. Iyyer, J. Wieting, K. Gimpel, and L. Zettlemoyer, “Adversarial Example Generation with Syntactically Controlled Paraphrase Networks,” [arXiv:1804.06059](https://arxiv.org/abs/arXiv:1804.06059) [cs], Apr. 2018.

R. Jia and P. Liang, [“Adversarial Examples for Evaluating Reading Comprehension Systems”](https://arxiv.org/abs/1707.07328) in Proceedings of the 2017 Conference on Empirical Methods in Natural Language Processing, Copenhagen, Denmark, 2017, pp. 2021–2031.

P. Minervini and S. Riedel, “Adversarially Regularising Neural NLI Models to Integrate Logical Background Knowledge,” [arXiv:1808.08609](https://arxiv.org/abs/arXiv:1808.08609) [cs, stat], Aug. 2018.

C. Buck et al., “Ask the Right Questions: Active Question Reformulation with Reinforcement Learning,” [arXiv:1705.07830](https://arxiv.org/abs/arXiv:1705.07830) [cs], May 2017.

R. Nogueira and K. Cho, “End-to-End Goal-Driven Web Navigation,” [arXiv:1602.02261](https://arxiv.org/abs/arXiv:1602.02261) [cs], Feb. 2016.

L. Dong, J. Mallinson, S. Reddy, and M. Lapata, “Learning to Paraphrase for Question Answering,” [arXiv:1708.06022](https://arxiv.org/abs/arXiv:1708.06022) [cs], Aug. 2017.

A. Fader, L. Zettlemoyer, and O. Etzioni, [“Paraphrase-Driven Learning for Open Question Answering,”](http://www.aclweb.org/anthology/P13-1158) in Proceedings of the 51st Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), Sofia, Bulgaria, 2013, pp. 1608–1618.

A. W. Yu et al., “QANet: Combining Local Convolution with Global Self-Attention for Reading Comprehension,” [arXiv:1804.09541](https://arxiv.org/abs/arXiv:1804.09541) [cs], Apr. 2018.

L. Dong, F. Wei, M. Zhou, and K. Xu, [“Question Answering over Freebase with Multi-Column Convolutional Neural Networks,”](http://www.aclweb.org/anthology/P15-1026) in Proceedings of the 53rd 
Annual Meeting of the Association for Computational Linguistics and the 7th International Joint Conference on Natural Language Processing (Volume 1: Long Papers), Beijing, China, 2015, pp. 260–269.

A. Bordes, S. Chopra, and J. Weston, [“Question Answering with Subgraph Embeddings,”](http://www.aclweb.org/anthology/D14-1067) in Proceedings of the 2014 Conference on Empirical Methods in Natural Language Processing (EMNLP), Doha, Qatar, 2014, pp. 615–620.

M. T. Ribeiro, S. Singh, and C. Guestrin, [“Semantically Equivalent Adversarial Rules for Debugging NLP models,”](http://aclweb.org/anthology/P18-1079) in Proceedings of the 56th Annual Meeting of the Association for Computational Linguistics (Long Papers) Melbourne, Australia, 2018 pp856-865.


