+++
title = "EMNLP 2018"
date = 2018-11-06T10:13:14Z
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = []

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = ['PhD Blog', 'Conference', 'Review']
categories = []

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
# Use `caption` to display an image caption.
#   Markdown linking is allowed, e.g. `caption = "[Image credit](http://example.org)"`.
# Set `preview` to `false` to disable the thumbnail in listings.
[header]
image = "posts/emnlp2018.jpg"
caption = ""
preview = true

+++

I just got back from EMNLP in Brussels. We were presenting our dataset paper ShARC (a blog post about ShARC will be coming soon). The scale and breadth of the conference was really something, with so many smart people doing amazing things. It was also great to meet, network and talk research with all kinds of academics in NLP. We’ve got some exciting projects planned already, and I’m really just starting out. it was great to meet you all, lets stay in contact 😀


I’ve attempted to distill some of my experiences into a post. There was so much going on, and I’m already starting to forget things! So here’s a *monster* post on work that particularly resonated with me. I saw over over 60 orals and loads of posters, but that was still only the tip of the iceberg at a conference with 549 accepted papers. I’ll start off with some high level trends I see, and then drill down into a day-by-day account of highlights. 

[Edit 1:] Added links to papers

[Edit 2:] Added links to other blogs 


# Summary

I think this year’s EMNLP had a few broad trends that should be noted. There was a broad focus on pushing more difficult tasks, and generally critical analysis of models, datasets and tasks. This is a good thing, we’re seeing less sensationalism and fewer “crazy architecture papers” that are poorly motivated.

We’re also seeing the growth in what I’ll call “socially-responsible NLP”, from investigations on biases within the research community, to work on fact verification and fake news. In particular, I thought the FEVER workshop was a great success and would encourage people get involved.

We’re also seeing growing interest in adversarial methods in NLP, from the odd GAN, discriminator networks, and generally the generation of natural language adversarial exaples continues to grow. It’s still relatively small, but I think next year, adversaries will be everywhere.

We’re also seeing a wide adoption of contextual embeddings, particularly ELMo. Attendees were given ELMo stickers and plush elmos in a good PR move from AI2. ELMo was used as an input ablation in a fair number of works, and indeed led to pretty much universal gains in loads of different applications

There were also a LOT of new datasets introduced. The datasets proposed were for ever more specialised tasks (including our work [ShARC](https://sharc-data.github.io/)) and often emphasised difficulty. The concept of a hardness filter (adversarial filtering and related techniques) seems to be catching on, to guarantee the dataset isn’t solvable by current methods. In theory this is great, I’m slightly concerned that subtle, model-based biases might be introduced into these datasets if we’re not careful.
Related to the above, there are several works that take existing datasets and perform a close, critical analysis of what types of modelling are actually required to do well on the task, particularly in question answering. 

We’re also seeing the continued momentum of common-sense and word-knowledge investigations, and some progress is being made, but there is still a long way to go here.

Some other points are the re-introduction of syntax and linguistic structure to modelling, which is a growth area, a strong emphasis on the interpretability of models, and methods to structurally constrain model outputs (with structured prediction, ILPs and the like)

There is more I could write here, and you may see very different trends to me (it’s a big conference, with 5 simultaneous tracks)

What follows is a session-by-session account of work that I thought was particularly interesting or worth discussing.

[Sebastian Ruder](http://blog.aylien.com/emnlp-2018-highlights-inductive-bias-cross-lingual-learning-and-more/) and [Claudia Hauff](https://chauff.github.io/2018-11-04-emnlp/) also also posted about EMNLP, check them out for a different perspective.

## Contents

* [Tutorial 1: Joint Models for NLP. Yue Zhang.](#tutorial-1-joint-models-for-nlp-yue-zhang)
* [Tutorial 3: Writing Code for NLP Research. Matt Gardner, Mark Neumann, Joel Grus, and Nicholas Lourie.](#tutorial-3-writing-code-for-nlp-research-matt-gardner-mark-neumann-joel-grus-and-nicholas-lourie)
* [FEVER Workshop](#fever-workshop)
* [Black box NLP](#black-box-nlp)
* [CoNLL - second day afternoon](#conll-second-day-afternoon)
* [Keynote I: "Truth or Lie? Spoken Indicators of Deception in Speech" Julia Hirschberg, Columbia University](#keynote-i-truth-or-lie-spoken-indicators-of-deception-in-speech-julia-hirschberg-columbia-university)
* [EMNLP Session 1](#emnlp-session-1)
* [EMNLP Session 2](#emnlp-session-2)
* [EMNLP Session 3](#emnlp-session-3)
* [EMNLP Session 4](#emnlp-session-4)
* [EMNLP Session 5](#emnlp-session-5)
* [EMNLP Session 6](#emnlp-session-6)
* [EMNLP Session 7](#emnlp-session-7)
* [Keynote II: "Understanding the News that Moves Markets" Gideon Mann, Bloomberg, L.P.](#keynote-ii-understanding-the-news-that-moves-markets-gideon-mann-bloomberg-l-p")
* [EMNLP Session 8](#emnlp-session-8)
* [EMNLP Session 9](#emnlp-session-9)
* [EMNLP Session 11](#emnlp-session-11)
* [Best Paper Awards](#best-paper-awards)

## Tutorial 1: Joint Models for NLP. Yue Zhang.

We kicked off the conference by attending the joint models for NLP Tutorial. Professor Yue Zhang explored how joint modelling of different linguistic tasks can benefit one-another, especially syntactic tasks like segmentation, POS taking and parsing. 
Some observations:

* I find it interesting that Chinese words have internal syntactic structure
* Shift/append/reduce type models seem to have been intensively studied in this area, using various stacks, queues and buffers to perform labelling and linking for lots of different tasks. It seems quite closely related to some ideas in program synthesis, where a few simple operations are defined, and models must use the ops to write programs to correctly solve a task
* Joint NER, entity linking and relation extraction seem a very natural joint modelling setup

The second half of the session focussed on neural joint modelling. The speaker identified four tasks, based on joint learning, but separate search (i.e. correlated tasks, but not codependent):

* Cross Task
* Cross Lingual
* Cross Domain
* Cross Standard (?)

It seems that NER  and language modelling are an effective combination, but which layers to share seem an important hyperparameter

There is also distinction between multi-view and multi-task models. Multi-view is where data points have different feature sets and they can all be exploited for a task, and multi-task is where you have several multi-view tasks on the same input.
After touching on pre-training, (ULMFit, ELMO, BERT) The author highlights some interesting work exploring the correlation between multitask learning and pretraining. I.E. pretraining is multitask learning, but where one task is fully fit before the other is trained. There has been some work to queue the tasks, to explore whether there is an optimal “interpolation” between fully pretraining and fully multi-task learning.


Finally the speaker touches on cross domain training, where the domain is fed to the model with the input, which looks to work pretty well. 
Unfortunately I can’t find the slides yet, so can’t cite the papers, If i find the slides, I will 😀


## Tutorial 3: Writing Code for NLP Research. Matt Gardner, Mark Neumann, Joel Grus, and Nicholas Lourie. 


This tutorial from AllenNLP was excellent, delivering practical advice that you know you should be doing, but somehow never end up doing properly in your experiments. In order to move fast, you have to build well, and pay particular attention to version control and reproducibility. Its 2018 , your results should be reproducible! The slides can be found [here](https://t.co/GykkyReuCO) and other (better?) bloggers than me have discussed it up [here](https://medium.com/@hadyelsahar/writing-code-for-natural-language-processing-research-emnlp2018-nlproc-a87367cc5146) (nice one Hady Elsahar!)

The speakers identify two VERY different modes of 2 modes of writing code:

1. Prototyping (doing research)
2. Writing components (preparing for future research)

The first half of the talk focuses on research prototyping. It should be:
* Quick
* Run experiments and keep track of what you’ve done
* Analyse model behaviour easily

To go fast, you. need to:

* Use a Framework
* Don’t start from scratch
* Don’t get boxed in by an overly restrictive library
* Get a good starting place” as soon as possible:
	* get a BASELINE running 
	* Use someone else’s code
	* Hopefully modularized

They make a few observations about how to do experiments fast and not create a hacky mess, namely to
* copy first, refactor later
* DON'T overengineer
* DO write readable code
* Comment the shape of tensors and Comment to explain non obvious logic
* Minimal testing but not NO testing


But what to test? They suggest that tests checking experimental behaviour is a waste of time, instead the non-experimental code should be tested.

When running experiments, each experiment should be associated with a git hash, so it can be exactly replicated. They also touch on beaker, an allennlp tool for reproducible experiments, that allows you to keep results.

They stress the importance of configs over code, i.e. switch behaviour on with a config, don't hardcode changes into the code. The same code should be used for all experiments that you publish. At the same time, visualise everything you can think of, on tensorboard or something similar.

You should also build demos. Models should be able to run without labels, and ideally you should build a web app to play around with the outputs to get an intuitive understanding of the results.

Finally, they discuss how to best share research, which is what we should all be aiming for,
You need to simply workflow for installation and data as much as possible.
The code should also run anywhere, so ideally, you should dockerize it, and/or use good package managers / environments like anaconda. You should also use filecaches, they’re great for data, a very clean coding construction.

## FEVER Workshop
The FEVER workshop was excellent. It would be great to see the momentum gained here continue and grow. The organisers did a great job.

#### Tim Rocktaschel: Invited speaker
Tim talked about his work on NTP, ShARC and some other projects. I’m familiar with Tim’s work so I haven’t made detailed notes. There was an insightful question from the audience asking whether NTP captures relatedness or semantic similarity. The questioner noted that rule induction shouldn’t work well with relatedness

#### Towards Automated Factchecking: Developing an Annotation Schema and Benchmark for Consistent Automated Claim Detection: Lev Konstantinovskiy, Oliver Price, Mevan Babakar and Arkaitz Zubiaga <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1809.08193" target="_blank" rel="noopener">Paper</a>

Lev works at FullFact, a charity fighting disinformation. FullFact defines the following fact checking workflow:

* monitor - more automatable
* spot claims that need checking - more automatable
* check claims
* Publish
* Intervention

This project involved working towards automatic claim detection, by first building a dataset for 7 classes:

* Not a claim
* Quantity
* Prediction
* Personal experience
* correlation/causation
* laws/rules of operation

They performed annotation using the excellent “prodigy” software from ExplosionAI. Unfortunately the inter annotator agreement wasn't great, but was much better when defining a binary classification task. The dataset consisted of 5,571 sentences, of which 1,570 are claims. The modelling allows the system to get P: 0.88 R: 0.80, F: 0.83

#### Shared Task Flash Talks - The Fact Extraction and VERification (FEVER) Shared Task


The fever task had 87 submissions, 23 teams, and f1 boosted from 0.18 to 0.53

Most Teams approached the task usually using a three step pipeline:

* Document selection: NEs, NPs, capitals, page viewerships, search apis
* Sentence selection: several different methods
* Classification using supervised training

The authors of *Combining Fact Extraction and Claim Verification in an NLI Model 
Yixin Nie, Haonan Chen and Mohit Bansal*, used a NSMN for all three steps, and come first in the task

*UCL Machine Reading Group: Four Factor Framework For Fact Finding (HexaF)
Takuma Yoneda, Jeff Mitchell, Johannes Welbl, Pontus Stenetorp and Sebastian Riedel*  <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="http://aclweb.org/anthology/W18-5515"  target="_blank" rel="noopener">Paper</a> use a pipeline method that starts with document retrieval, followed by sentence retrieval, then an NLI step and finally a label aggregation. They note that capitalisation and whether evidence is near the beginning of the article are important features and use entity co-ref by adding title of the article to the sentence NLI model

*Multi-Sentence Textual Entailment for Claim Verification
Andreas Hanselowski, Hao Zhang, Zile Li, Daniil Sorokin, Benjamin Schiller, Claudia Schulz and Iryna Gurevych*  <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="http://aclweb.org/anthology/W18-5516"  target="_blank" rel="noopener">Paper</a>also find it useful to do entity linking for the entities in the document, finding wikipedia articles for additional evidence

*Team Papelo: Transformer Networks at FEVER
Christopher Malon* <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="http://aclweb.org/anthology/W18-5517"  target="_blank" rel="noopener">Paper</a>This team uses GPT transformers for the NLI step, and their submission has quite different P/R tradeoff behaviour


#### The Data Challenge in Misinformation Detection: Source Reputation vs. Content Veracity Fatemeh Torabi Asr and Maite Taboada.

Fatimeh Joined on a Video Call. They have been building systems by examining whether it is possible to use the reputation of a publisher to train a system to detect true or fake content. On the whole, the reputation-based data is useful, but a correlation analysis shows that the there are some issues, as legitimate articles often labelled as satire, and buzzfeed data often labelled as a hoax. The discourse processing lab have an interesting website [here](https://fakenews.ngrok.io).


#### Invited Talk: Call for Help: Putting Computation in Computational Fact Checking Delip Rao

Delip talked passionately about the need to tackle Fake news now and how it is our duty as practitioners to help solve the problem. He detailed several different fake news solutions:

* Policy
* Journalism
* Education
* Technology (AI, UX and more)
* Research

He also names different “actors” and “spreaders” of fake news

* Actors: Celebrities, criminals/terrorists, Activists, Government
* spreaders: Bots, “Useful idiots”, Conspiracy theorists, journalists

And highlighted the importance of different actions for different fake news actors


#### Announcement from James Thorne, University of Sheffield
James introduced the plans for FEVER 2. It will follow the “build it, break it, fix it” methodology of [Ettinger et al.](http://aclweb.org/anthology/W17-5401)

* Build it:
	* Existing models will be used to build baselines using existing fever data
	* An API will be developed for the models
* Break it:
	* Adversaries will be tasked to generate new data that breaks the baselines
	Systems will be tested live using the APIs
	Breakers will be able to submit their 1000 best examples for competition
	Breakers will be scored on the number of systems they are able to break
* Fix it:
	* Half the breaker data will be released for training new models, the rest will be retained for testing

This plan is really good, I’m excited to see how progress improves here


## Black box NLP

#### Context-Free Transductions with Neural Stacks. Yiding Hao, William Merrill, Dana Angluin, Robert Frank, Noah Amsel, Andrew Benz and Simon Mendelsohn. <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1809.02836"  target="_blank" rel="noopener">Paper</a>

The authors investigate whether NNs augmented with a neural stack really use the stack data structure. They defined a couple of different tasks:

1. reverse a string - the authors find that LSTM controllers don’t exploit a stack well, instead using it as unstructured memory.
2. balanced parenthesis language modelling, an easy task, naive networks do quite well, but lstm controller networks are very good. Stack gets used as unstructured memory, rather than as a stack
3. Parity evaluation - compute the bitwise parity of a binary string at each time step equivalent to taking xor of previous parity with a new bit. Buffered architectures can solve this well, but un-buffered can’t do it (random guess)

They conclude that Stack RNNs learned intuitive and interpretable solutions to simple algorithmic tasks, but generally don’t use stacks in an appropriate way. Further inductive biases might be needed to get controller networks to use stacks correctly.

## CoNLL - Second Day Afternoon


#### Comparing Models of Associative Meaning: An Empirical Investigation of Reference in Simple Language Games. Judy Hanwen Shen, Matthias Hofer, Bjarke Felbo and Roger Levy <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1810.03717"  target="_blank" rel="noopener">Paper</a>

The authors investigate a simplified version of CodeNames.  one player needs to pick one of three words that allow the other player to “pick an odd one out” from a list of three other words.
They are interested in finding what semantic resources humans use to form word associations, and investigate 5 resources:

* Noun, adjective Bigram associations
* Conceptnet5 similarity
* word vector cosine distance
* LDA topic modelling - here the euclidean distance between the word’s topic distribution was used

They find that the bigram system best predicts how human players played the game (especially for player 2) which suggests “that direct co-occurrence statistics are particularly salient in associative settings”. They also note that there was a discrepancy between the player one strategy and the player two strategy, suggesting that the information each player uses is different.


#### Sequence Classification with Human Attention (special paper award) Maria Barrett, Joachim Bingel, Nora Hollenstein, Marek Rei and Anders Søgaard <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="http://aclweb.org/anthology/K18-1030"  target="_blank" rel="noopener">Paper</a>

This paper won a special prize for psycho-linguistics, and is great! The authors try to encourage neural attention in sequence classification to work more like humans by using eye-tracking measurements. They do not directly supervise the attention signal, but instead regularize attention by using the gaze time measured from eye tracking software on humans reading news articles. This work is really cool, and is one of very few that directly use signals from humans doing instinctive tasks.

They test their system on sentiment classification, grammar detection and hate speech detection. They found that using “mean fixed duration” gaze from the ZUCO and Dundee corpora improved scores by ~ 0.5% - 2% F1


## Keynote I: "Truth or Lie? Spoken Indicators of Deception in Speech" Julia Hirschberg, Columbia University

Julia was the first keynote of the conference, detailing approaches to improve deception detection. She described the process to build a clean dataset containing people speaking the truth and lying. Both humans and ML algorithms were given the audio of people either speaking the truth or lying, often about quite sensitive topics. 

The ML systems were better at detecting lies than humans, but the humans and machines tended to make different mistakes
They found that detection of deception was easier for males, and the best people at detecting lies were those who scored highly in a personality test on openness and agreeableness.

Interestingly, the personality type of the lier was an important feature for the ML models to detect lies. They also found that entrainment (copying someone’s mannerisms when speaking with them was an important factor too, as well as pitch and “inter-pausal units”.

In their next work, they plan to obtain more human judgements by crowdsourcing with the lying game, in order to understand human deception detection better.
Note from me: stress and context are super important here. When a human is stressed, tired or annoyed, the way they lie is likely to be very different? We have problems with domain transfer here


## EMNLP Session 1

#### Reasoning about Actions and State Changes by Injecting Commonsense Knowledge. Niket Tandon, Bhavana Dalvi, Joel Grus, Wen-tau Yih, Antoine Bosselut and Peter Clark <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1808.10012"  target="_blank" rel="noopener">Paper</a>

The authors introduce the "ProPara dataset”. A dataset to predict the actions and state changes during a piece of procedural natural language. The task is somewhat related to one of the BABI tasks but is actually written in natural language, and also the recently released RecipeQA, but has a richer vocabulary and more diverse domains.

The task itself provides the model with a sequence of sentences and a set of entities within the sentence. The model must read each sentence, and predict whether the state of any of the entities has changed.
They consider the following actions: consumed, produced, what coversions have occurred and what movements have occurred. 
Interesting point: they notice that greedy decoding may result in nonsense predictions

For example, if previously destroyed, a entity cannot go through any further state changes
They achieve this by using structured prediction
This system needs common sense to do well.
They find that existing entity tracking systems such as the recurrent entity network do not work that well here, and their system improves the score by over 13 F1 points, 
the most common errors come from implicit reference, coreference and knowledge retrieval 


#### Collecting Diverse Natural Language Inference Problems for Sentence Representation Evaluation. Adam Poliak, Aparajita Haldar, Rachel Rudinger, J. Edward Hu, Ellie Pavlick, Aaron Steven White and Benjamin Van Durme <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1804.08207"  target="_blank" rel="noopener">Paper</a>

This work collects together NLI type datasets for various different semantic phenomena from 13 existing NLI datasets, creating the Diverse Natural Language Inference collection (DNC) which can be found at https://www.decomp.io. DNC is large and diverse, covering proto-roles, factuality, generics, common sense inference, wordsense inference, and others. Together there are over 500k examples. During modeling, they find that it is necessary to pre training on MNLI data to get good performance on puns and sentiment (fine tuning was necessary), and that only providing the hypothesis still allows good results on NER. 

#### Textual Analogy Parsing: What's Shared and What's Compared among Analogous Facts. Matthew Lamm, Arun Chaganty, Christopher D. Manning, Dan Jurafsky and Percy Liang <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1809.02700"  target="_blank" rel="noopener">Paper</a>

Motivated by the setting Automatic visualisation of summary language, this work proposes a new task called Textual Analogy Parsing. The task requires the extraction of facts and the higher order relation between these facts. The paper introduces a dataset for the task, and provides a model that uses ILP to enforce extracted analogy graphs to fit the task settings constraints. This is another paper that needs to constrain the outputs of NNs to conform to logical constraints. The model first identities entities and their semantic types and quantities. Then an analogy graph is built and then the graph is used to fill an analogy frame. Models are evaluated using P/R/F on labelled vertex-edge-vertex triples.


#### SWAG: A Large-Scale Adversarial Dataset for Grounded Commonsense Inference. Rowan Zellers, Yonatan Bisk, Roy Schwartz and Yejin Choi <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1808.05326"  target="_blank" rel="noopener">Paper</a>

SWAG is new dataset designed to be difficult, created using a process called adversarial filtering. The task requires a model to correctly choose 1 of 4 follow-on sentences, given an initial setup question, and can be think of ranking the most plausible/commonsense continuation of a piece of natural language. The false examples were chosen to challenging to identify.
The motivation for this paper comes stems from the observation that most NLI tasks require only linguistics, but NLI itself should also be about (situational) common sense. The task is also very related to story cloze. The adversarial filtering is a dataset construction theme that we’ve seen a couple of times now too. The dataset is large 113k putting it over the arbitrarily chosen 100k target for big nlp datasets.

The dataset construction method is interesting:

* Two sequential sentences are sourced from audio descriptions from videos on activitynet    
* The second sentence becomes the gold answer
* A language model is fed the first sentence, plus the first few tokens of the second sentence (the first noun phrase). The LM then produces many negative endings to the second sentence
* These are fed through the adversarial filtering approach (see paper for full description)
	* Randomly split into train and test, and fit a model on train
	* On the test set, find “easily answered examples” and replace them with harder ones
	* Repeat until convergence
* Finally candidates are manually annotated by humans to ensure they are real negatives


## EMNLP Session 2

#### Adaptive Document Retrieval for Deep Question Answering. Bernhard Kratzwald and Stefan Feuerriegel <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1808.06528"  target="_blank" rel="noopener">Paper</a>

I found this interesting as it was similar to experiments that we had run in-house at Bloomsbury AI. Their findings are similar to (but not the same as) ours. Commonly QA  systems consist of a IR step to collect relevant documents, then a reading step to extract answers from the candidate documents. The authors investigate the possibility of an adaptive number of documents to retrieve, conditioned on the corpus size and the query type. They motivate this by running experiments that show that, as corpus size increases, recall@1 becomes volatile and larger numbers of retrieved documents should be used, but for small corpora, larger retrieval sizes led to decreases in accuracy as their model was distracted by spurious answers.
We found at bloomsbury, that our readers were always better than IR at finding answer paragraphs, and the distraction effect wasn't a problem for us

## EMNLP Session 3

#### Generating Syntactic Paraphrases. Emilie Colin and Claire Gardent <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="http://aclweb.org/anthology/D18-1113"  target="_blank" rel="noopener">Paper</a>

The authors here study the generation of syntactic paraphrases. They show that the conditioning generation on syntax constraints gives syntactically distinct paraphrases, and they are able to generate from data, text or a combination of the two.
They forumate the task as structured prediction conditioned on input and syntactic constraint. The same input can be mapped to several outputs, each fulfilling a different syntactic constraint
They have 4 tasks: 

1. Take RDF triples and generate text.
2. Take a sentence and a constraint, and generate text
3. Take text and an RDF triple, and generate text that contains the RFD triple
4. Take text and an RDF triple, and generate text such that the RDF triple is removed from the text

They can perform all the tasks quite well, and syntax constraints very much improve performance on bleu score evaluation. 

## EMNLP Session 4

I went to the VQA session 4, knowing little. RecipeQA was included here due to multimodality QA, which I think is interesting, but up-to-now fairly understudied.

#### RecipeQA: A Challenge Dataset for Multimodal Comprehension of Cooking Recipes. Semih Yagcioglu, Aykut Erdem, Erkut Erdem and Nazli Ikizler-Cinbis <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1809.00812"  target="_blank" rel="noopener">Paper</a>

The authors note the trend that more challenging QA datasets are needed. They construct the multimodal procedural dataset RecipeQA from a recipe website. The recipe steps are accompanied with images, making this a multimodal question answering dataset.
It is of moderate size (36K q,a pairs) and questions are (mostly) clozes with multiple choice answers. Both visual and textual QA are represented. Comparisons to ProPara, a similar entity-state tracking dataset is interesting here. This dataset contains many more instances, but there is less rich labelling per document.

## EMNLP Session 5

We were presenting our poster on ShARC during this session. Despite the early start (after really fun industry hospitality events the night before!) and the less than optimal positioning of our poster board, we had positive reception from people who came. We’d love for people to attempt our task, and a blog post will be coming out soon with an introduction to the ShARC dataset and task !


## EMNLP Session 6


#### emrQA: A Large Corpus for Question Answering on Electronic Medical Records. Anusri Pampari, Preethi Raghavan, Jennifer Liang and Jian Peng <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1809.00732"  target="_blank" rel="noopener">Paper</a>

The authors leverage expert annotations on clinical notes from the i2b2 task. They partially automate the QA dataset creation task to create a huge 400,000 question,evidence pair dataset for medical QA on electronic medical records. They additionally release 1 million question-logical form pairs. Furthermore they add some extra task phenomena that are not present in the popular machine comprehension datasets like SQuad. These are arithmetic and temporal reasoning, both vital for medical question answering.
They build a dataset as follows:

* Collect questions on the domain, and templating the questions
* Associate these templates with expert annotated logical forms
* Use existing dataset annotations to populate Question and logical form templates and get answers

They use 680 question templates, which may not be enough to generalize to natural language questions gracefully

#### HotpotQA: A Dataset for Diverse, Explainable Multi-hop Question Answering. Zhilin Yang, Peng Qi, Saizheng Zhang, Yoshua Bengio, William Cohen, Ruslan Salakhutdinov and Christopher D. Manning <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1809.09600"  target="_blank" rel="noopener">Paper</a>

HotPotQA is another new QA dataset, attempting to cover a broad range of different phenomena: Multihop reading, text-based reasoning, diverse domains, explainability, and 
Comparison questions. A neat part is that HotPotQA annotates which sentences contain facts that are needed to answer questions, and models are required to not Just answer the question, but also “cite” the facts that were used to answer the question. These facts at training time can be used as supervision. They seperate multihop questions into two broad categories:

* Type 1: inferring a bridge entity to complete 2nd hop
* Type 2: locating the answer by checking several qualifier properties 

The comparison questions are also new, (but I feel are somewhat arbitrarily thrown in, but they are “multihop” in the sense that the model must answer a question about both things being compared and then compare the answer). 

Modelling reveals that supporting facts are important during training, and that their BiDAF++ type baseline performs quite poorly compared to humans.


#### Can a Suit of Armor Conduct Electricity? A New Dataset for Open Book Question Answering. Todor Mihaylov, Peter Clark, Tushar Khot and Ashish Sabharwal <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1809.02789"  target="_blank" rel="noopener">Paper</a>

Another challenging, small-scale AI2 dataset was introduced here, called the OpenBook dataset. The task focus is to do multi-hop reasoning with partial context. Facts are provided (an open book test) and a question that requires the application of a fact and the use of common(sense) knowledge to correctly answer a multiple choice question. The task sits somewhere in between reading comprehension and open-ended QA. The dataset is quite small, so successful (5900 questions an an openbook of 1,326 facts) so some transfer learning will be required. Similarly to Swag, they apply a check that existing systems do not perform well at dataset construction time, to make the dataset more challenging. Whilst this is great in theory, I do wonder if it introduces difficult to detect biases.


#### Evaluating Theory of Mind in Question Answering. Aida Nematzadeh, Kaylee Burns, Erin Grant, Alison Gopnik and Tom Griffiths <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1808.09352"  target="_blank" rel="noopener">Paper</a>

This paper was an interesting change of pace.  QA requires reasoning, and should not be able to just look up info. The authors wonder if models answer a question using the right information? or just cheat.
The BABI tasks don’t examine reasoning about beliefs. So the authors design set of tasks for evaluating the capacity to reason about beliefs, known as “Sally Anne Tasks”. A simple situation is described, where Sally and Anne interact with objects, sometimes without each other’s knowledge. For instance:

* Sally puts a ball in the box, 
* Sally leaves the room, 
* Anne takes the ball out of the box and puts it in a bag. 
* Then Sally returns to the room.

The model is asked “where will sally look for the ball” and the answer requires the model to understand that Sally believes the ball is where she left it, the box, not the bag.

They test several different belief tasks:

* First order true belief: e.g. sally’s belief about an object she observed being moved
* First order false belief: e.g. sally’s belief about a object she did not observe being moved
* Second false order belief: e.g. anne’s belief about sally's belief

They use a memn2n, multiple observer model (with separate memories for sally and anne and the observer), an Entnet and a relation network (relnet) They find that first order belief is harder for models, but not (adult) humans, that models with explicit memory fail at belief questions, while Entnet and relnet fail at memory questions. It seems models with recursive nature are required for higher order belief modelling

## EMNLP Session 7


I went to short posters. There were several interesting ones, three to particularly highlight

* Generating Natural Language Adversarial Examples. Moustafa Alzantot, Yash Sharma, Ahmed Elgohary, Bo-Jhang Ho, Mani Srivastava and Kai-Wei Chang 
* Loss in Translation: Learning Bilingual Word Mapping with a Retrieval Criterion. Armand Joulin, Piotr Bojanowski, Tomas Mikolov, Hervé Jégou and Edouard Grave 
* Bayesian Compression for Natural Language Processing. Nadezhda Chirkova, Ekaterina Lobacheva and Dmitry Vetrov


## Keynote II: "Understanding the News that Moves Markets" Gideon Mann (Bloomberg, L.P.) 

A generally interesting talk, also reminding us of the responsibility to create NLP that is robust, given the increased adoption and integration into systems critical for our society. How do we build appropriate checks and balances that mean that flaws in nlp systems don’t trigger huge selloffs? The extreme speed at which the market responds to financial news is simultaneously awe-inspiring and terrifying

## EMNLP Session 8

Generation session: This was actually a great session. I know little about generation, but the work was inspiring here, with applications in work that I have planned.

#### Integrating Transformer and Paraphrase Rules for Sentence Simplification. Sanqiang Zhao, Rui Meng, Daqing He, Andi Saptono and Bambang Parmanto <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="http://aclweb.org/anthology/D18-1355"  target="_blank" rel="noopener">Paper</a>

The task here is to simplify language so it is easier to understand for children or non-native speakers, whilst retaining the original meaning. They use Transformers and integrate rules from the simple PPDB KB to improve simplification and select more accurate simplification rules. 

They integrate rules by introducing a loss that maximises the likelihood of applying simplification rules, which is minimised in addition to a sequence generation loss. They also augment their architecture with an architecture to memorize simplification rules

#### Learning Neural Templates for Text Generation. Sam Wiseman, Stuart Shieber and Alexander Rush <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1808.10122"  target="_blank" rel="noopener">Paper</a>

This work involved learning templates for text generation using a a conditional neural semi-hidden markov model. They (correctly) argue that templating for Natural Language generation is both more interpretable and controllable than direct neural sequence generation. 
They use the wikiBio dataset for generation, where a wikidata infobox is used to generate a natural language description of the box. This is cool, original work, using some nice applications of various old favourite dynamic programming algorithms

#### Multi-Reference Training with Pseudo-References for Neural Translation and Text Generation. Renjie Zheng, Mingbo Ma and Liang Huang <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1808.09564"  target="_blank" rel="noopener">Paper</a>

This work focussed on the taks on generating more references for translation and generation tasks, which is essentially the same task as paraphrase generation. They state the (often avoided, awkward) obvious truth that there are exponentially many valid, meaning preserving paraphrases/reference translations. They propose to generate more, using a lattice construction. They first show how to do lattice construction with “hard alignments”:
* Compress existing references by merging identical words (recursively?)
* Traverse the lattice, creating a pseudo-reference for each possible path from the star to the end of the lattice

They then extend the hard alignment using semantic similarities from a language model so the alignment can work for synonyms. A drawback of the method is that the sentence structures produced won’t greatly differ from the original references.

## EMNLP Session 9

Here I jumped betweem a few different tracks and the poster sessions.

#### Noise Contrastive Estimation and Negative Sampling for Conditional Models: Consistency and Statistical Efficiency. Zhuang Ma and Michael Collins <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1809.01812"  target="_blank" rel="noopener">Paper</a> 

This (mostly theoretical) work looks at Noise contrastive estimation ranking and classification losses and how consistent they are with a full MLE. They found that ranking based losses are more consistent for more tasks than classification losses (which are consistent only under the assumption that the partition function is constant) , but both are approach the MSE as K increases. 

#### Pathologies of Neural Models Make Interpretations Difficult. Shi Feng, Eric Wallace, Alvin Grissom II, Mohit Iyyer, Pedro Rodriguez and Jordan Boyd-Graber <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1804.07781"  target="_blank" rel="noopener">Paper</a>

This work investigates what happens when reading comprehension questions are “reduced” when dropping words. They show for some examples that they can reduce the question down to a single token and still keep the answer the same. The method is kind of the opposite of the usual method of creating adversaries (perturb the input until the output changes). They remove a word that is the “least important” in determining the answer. They argue that as the input becomes less well defined, the output confidence should decrease, and probability mass spread out.  They have some examples: 

* SQUAD: "what did tesla spend anstors money on?"> 'Did' (0.78 -> 0.91)
* VQA: “what color is the flower” > "flower" (0.83 -> 0.82)

They propose a way to fix this, by generating rubbish examples, and then training the model to maximize output entropy on the reduced examples

#### Adversarial Deep Averaging Networks for Cross-Lingual Sentiment Classification. Xilun Chen, Yu Sun, Ben Athiwaratkun, Claire Cardie, Kilian Weinberg <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1606.01614"  target="_blank" rel="noopener">Paper</a>

There is not a lot of data for sentiment in languages other than english. The idea is to use a resource rich language for sentiment tasks in other languages, The authors attempt to learn learn language invariant features, using only monolingual data. They use bilingual/multilingual word embeddings and a language discriminator network on the embedded language


## EMNLP Session 10

I spent this session in the question answering track

#### Joint Multitask Learning for Community Question Answering Using Task-Specific Embeddings. Shafiq Joty, Lluís Màrquez and Preslav Nakov <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1809.08928"  target="_blank" rel="noopener">Paper</a>

These authors tackle community question answering by combining three task in community question answering using a graphical model (nice to see in QA). The three defined tasks in community question answering were:

* Finding similar questions to a new question
* Finding relevant answers to the new question
* Finding whether the answer to a question in a thread is a good answer

The three tasks can benefit from each other, so they jointly model them using a CRF with joint norm, training with rmsprop, and performing inference with LoopyBP.

#### What Makes Reading Comprehension Questions Easier?. Saku Sugawara, Kentaro Inui, Satoshi Sekine and Akiko Aizawa <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1808.09384"  target="_blank" rel="noopener">Paper</a>

This paper is on trend with some others I saw during EMNLP, including the related, but less interesting best short paper award. The authors here seek to evaluate how difficult different QA datasets are. They define some heuristics to answer questions, and assert that questions are easy if they can be answered by the simple heuristics.

In some of the  datasets, the most similar sentence to the question was an effective method. 
Triviaqa, race, mctest, arc-e, arc-c were found to be quite challenging datasets, and Qangaroo was found to be quite variable, the easy questions were very easy, and the hard questions were very hard. 
The authors also consider whether the hard questions are hard, or just unanswerable.

TriviaQA, Quangaroo, and ARC are found to have quite high numbers of “unsolvable” questions


## EMNLP Session 11

#### The Importance of Being Recurrent for Modeling Hierarchical Structure. Ke Tran, Arianna Bisazza and Christof Monz <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1803.03585"  target="_blank" rel="noopener">Paper</a>

The authors here specifically investigate the properties of LSTMS and Transformers for modelling in tasks that specifically require hierarchical structure. They look at subject verb agreement and logical inference on artificial language. Interestingly, going against most people’s intuition and the momentum in the research community , they find that the LSTM based model consistently outperforms the transformers by a small but noticeable margin. This is despite the powerful pairwise connections between tokens in transformers. This result is hard to interpret but should be more investigated further, and the results reproduced.

## Best Paper Awards

#### How Much Reading Does Reading Comprehension Require? A Critical Investigation of Popular Benchmarks. Divyansh Kaushik and Zachary C. Lipton <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1808.04926"  target="_blank" rel="noopener">Paper</a>  

This work is on-trend and is simple and self contained. The authors train QA systems with only the context or question, deliberately depriving it of the information in-theory required to solve the task. This work is in the same spirit as several other papers in this year’s emnlp, and the authors rightly cite some similar work in NLI and other settings from previous years

#### Linguistically-Informed Self-Attention for Semantic Role Labeling. Emma Strubell, Patrick Verga, Daniel Andor, David Weiss and Andrew McCallum <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1804.08199"  target="_blank" rel="noopener">Paper</a>

This was a great piece of work and an excellent presentation. The authors begin by stating the 
fast, accurate, robust nlp is vital for web-scale deployment.
The authors get a boost in SRL tasks both in- and out-of-domain by applying multitask learning techniques for linguistic phenomena. They augment transformer architectures by introducing a new form of self attention, the syntactically informed self attention. One attention head is used to attend to the syntact heads of words. In addition, different layers of the transformer are used to predict other linguistic items, ie.  pos tagging,  predicate detection, semantic role spans and labels

#### Phrase-Based & Neural Unsupervised Machine Translation. Guillaume Lample, Myle Ott, Alexis Conneau, Ludovic Denoyer and Marc'Aurelio Ranzato <a class="btn btn-outline-primary my-1 mr-1 btn-sm" href="https://arxiv.org/abs/1804.07755"  target="_blank" rel="noopener">Paper</a>

Finally Guillaume presented the (now famous) work on unsupervised machine translation. The process can be summarised as 3 steps:

1. Initialization: the two distributions can be roughly aligned by learning a phrase or word-to-word translation in an unsupervised way
2. Language modelling: a language emodel is learnt independently in each domain, which can be used to denoise sentences
3. Back translation: starting from an observed source sentence, we translate to the target language using the current model, then reconstruct the sentence by using the source-target translation. The discrepancy is used to train the target->source model

They also shared a preview of using their framework to perform style transfer, which is super exciting work


Phew we're done. What a lot of papers.
