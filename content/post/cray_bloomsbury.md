---
title: "Breaking the TriviaQA SoTA with ELMo and Horovod"
date: 2018-10-06T20:30:29+01:00
draft: true
tags: ['QA']
---

The post below is a (slightly edited) [repost](https://www.migarage.ai/intelligence/bloomsbury-ai-cape/) from a blog post I wrote for the collaboration between Cray Supercomputers, Digital Catapult and Bloomsbury AI. Its an informal report of how we used Cray's supercomputing resources to both increase the accuracy and massively accelerate the speed of training machine readers.


## Bloomsbury AI's Cape, in partnership Cray and Digital Catapult, breaks the state of the art in Open Domain Machine Reading

### Key Points:

* Bloomsbury AI develops Natural Language Processing Technology to help humans to scale their expertise.
* Bloomsbury AI's product [Cape](https://github.com/bloomsburyai/cape-webservices) answers natural questions on free text documents. Cape has diverse applications that range from customer service and regulatory compliance, to powering digital assistants.  
* Working in partnership with Digital Catapult’s Machine Intelligence Garage, Cray provided Bloomsbury AI access to their Accel AI lab and a Cray CS-Storm, a powerful GPU cluster optimized for AI and Deep Learning.
* The goal was to improve the accuracy of the existing model architecture, to coincide with the open-sourcing of the Cape Project.
* The focus was to demonstrate how a Cray CS-Storm could be used to accelerate training speed, and how by taking advantage of the Cray CS-Storm's power, adjust the training dynamics
* Using the Cray CS-Storm and Horovod enabled a 10x speed up in training time, from over 200 hours on a GCP server with a k80 GPU to about 20 hours on the CS-Storm.
* Increased training speed enabled more efficient experimentation, and when coupled with high quality training signals from many parallel workers, lead to an increase in model accuracy, allowing Cape to become the state of the art in [Wikipedia open-domain question answering](https://competitions.codalab.org/competitions/17208#results) at the time of writing.


### Bloomsbury AI and Cape

[Cape](https://github.com/bloomsburyai/cape-webservices), from Bloomsbury AI, is a complete end-to-end solution for a question answering system, including models, back-end servers and front-end admin interfaces.
Cape's core functionality allows users to upload a free text documents to the system. A user can then pose questions (in natural language) to a document or collection of documents, and Cape will extract strings from the document(s) that best answer the inputted question.
Cape can also be used as a python library for developers and hobbyists to integrate natural language question answering components into their systems.
Cape can run in inference mode on a variety of different hardware, including locally and on cheap cpu-only servers on the cloud. Cape is designed to be both accurate and scalable.

A demo is shown in the image below. The document can be as short as a sentence or as long as several hundred paragraphs.

{{< figure library="1" src="cray_bloomsbury/demo.png" title="Figure 1: The above image shows a demo of the solution. The document can be as short as a sentence or as long as several hundred paragraphs." >}}

At the heart of Cape is a deep neural network that predicts the probability that a short span of text is the answer to an inputted question and document pair. The task is known as machine comprehension, machine reading, or extractive question answering in the literature. Cape's model is based on the document-qa model from [Clark et al. 2017](https://arxiv.org/abs/1710.10723), augmented with an ELMo language model ([Peters et al. 2018](https://arxiv.org/abs/1802.05365)), and several other architectural tweaks. Cape is multipurpose, and specifically trained in order to perform well on several diverse datasets simultaneously.

The inclusion of the ELMo language model is _highly beneficial in terms of accuracy_, especially on datasets such as SQUAD ([Rajpurkar et al. 2016](https://arxiv.org/abs/1606.05250)), but _at the cost of significantly increasing the model's training time_, especially for longer documents.
Cape uses an innovative caching strategy that allows it to answer quickly on documents it has seen before at inference time, but the team were experiencing training times on typical cloud computing hardware of over 200 hours (an NVIDIA Tesla k80 GPU on a system with 40GB of CPU RAM).
This training time severely limited the experiments that Bloomsbury AI's Machine learning engineers could perform, whilst also incurring significant cost to the business for server time.

### Partnership with Digital Catapult, Cray AI Lab and CS-Storm

Digital Catapult has worked with Bloomsbury AI before. Through Machine Intelligence Garage, Digital Catapult's programme to provide UK based machine learning startups with access to cutting edge compute resources, Bloomsbury AI received access to a powerful NVIDIA DGX-1 deep learning machine, which had been used to develop the model that was being used by the team. As a partner of Machine Intelligence Garage, the world leading manufacturer of supercomputers Cray is working with UK deep tech startups to help them access the compute resources they need to realise their business goals.
Cray’s Accel AI lab was able to provide Bloomsbury AI with access to a CS-Storm system, and the support of one of their in-house experts. The Cray CS-Storm 500NX system is specifically built for AI workloads. The cluster used for this collaboration consists of 4 nodes, each with two 18 core Intel Xeon “Broadwell” CPUs with 512GB RAM and 8 NVIDIA Tesla V100 GPUs. The nodes are connected by InfiniBand connections, and the system is supported by a 55TB Cray ClusterStor Lustre filesystem.

The partnership's goals were to demonstrate how Cray's AI machines could be used to provide real business value, and showcasing the raw power of CS-Storm systems.
The models that were to be trained on CS-Storm would be required to work with the rest of Cape's technology. This provided the team with two constraints:

1. The model architecture couldn't significantly change
2. The model's size (parameters, number of layers etc) had to be constrained so that it could still run on regular hardware.

As a result, the team decided to focus on how the training dynamics could be modified to improve the model, and on accelerating the training process.


### Experiments

Cape is trained on a several different datasets, some in-house, and others publicly available. Of particular interest to the Bloomsbury AI team is the TriviaQA dataset from [Joshi et al, 2017](https://arxiv.org/abs/1705.03551).

TriviaQA tests a Machine reading system's ability to answer factual questions on full web and Wikipedia articles. The dataset itself is used in a _distantly supervised_ paradigm. It consists of natural language questions, their answers, and a list of several Wikipedia or web articles that _should_ contain the answers. These articles are split into _chunks_ of a predefined number of words, and trained independently.[^1]
 Chunks that happen to contain the answer to the inputted question may not contain sufficient information to infer that the answer is indeed the correct answer. This is why the training regime for TriviaQA is said to be distantly supervised, as in practice, although most of these datapoints are high quality, there is a significant amount of noise.

 We shall focus on the results on TriviaQA Wiki, the subset of TriviaQA concerned with answering questions on Wikipedia articles. This subset contains 77582 questions and 48822 Wikipedia articles, with an average of 1.79 articles per question.

There were two main avenues open for experimentation that would make good use of the CS-Storm whilst staying within the design constraints:

1. **More powerful hardware (in particular larger GPU memory) allows for the size of text chunks to be increased**: This should improve performance by allowing the model to learn longer-distance dependencies, and reduce the amount of the noise in the dataset.
2. **Take advantage of multiple GPUs to train in a data-parallel paradigm**: Using the [Horovod](https://github.com/uber/horovod) framework from [Sergeev and Del Balso, 2018](https://arxiv.org/abs/1802.05799), We can distribute training on to many GPUs and nodes.[^2] The benefit of this is twofold. Firstly, it increases the training speed, and secondly, by increasing the effective mini-batch size, the average update is _less noisy_, which is especially important in our distant supervision training regime.

### Results - Convergence speed

{{< figure library="1" src="cray_bloomsbury/training_speeds.png" title="Figure 2: The graph above shows the learning curves for several different experimental setups, with the number of hours of training vs the accuracy relative to that model’s peak accuracy." >}}

The figure shows the learning curves for several different experimental setups, with number of hours of training vs the accuracy relative that model's peak accuracy.
The orange curve is for the control model, trained  on short chunk lengths on a normal cloud server. The time taken to reach the optimal development accuracy was over 200 hours.
The green curve shows an equivalent model trained using only one of the CS-Storm's V100 GPUs. The time to peak performance is reduced to about 40 hours, a 5x speedup over the cloud server.
The blue curve shows the final model's learning curve, trained using 16 GPUs distributed across two of CS-Storm's cores. This system not only breaks the state of the art for this dataset, it also takes only about 20 hours to train.

It is also worth noting that the final model achieved an accuracy within 5% of its peak accuracy in less than 4 hours of training, whereas this milestone took over 42 hours for the k80.

### Results - Accuracy

Two metric used to assess model performance are Exact Match (EM) and F1 score. The EM score the percentage of extracted answers that exactly match the gold reference answer, and the F1 metric is the mean word overlap F1 between the extracted answer and gold reference answer.


Using two the CS-Storm nodes and 16 workers allowed for effective mini-batch sizes of 960, compared to only 30 on the cloud server. Not only did this enable very fast training, it also compensated for the noise in the dataset considerably. When using data parallelism, the research community recommends _linearly scaling_ the learning rate with the number of workers, following the example of [Goyal et al. 2017](https://arxiv.org/abs/1706.02677). However, the focus of their research was a supervised learning task, with very little noise in the dataset. Bloomsbury AI found that _linearly scaling the learning rate for their task led to instability_ and poor performance. However, _not modifying the learning rate did not lead to accelerated training_. A good compromise was to _scale the learning rate by a factor of the square root of the number of workers_, which empirically provided excellent performance and training speed.

The team also experimented with very long document chunk sizes. Because of the extreme length of these chunks, the training was considerably slower. This type of model looked very promising from an accuracy perspective. but had two major drawbacks - they took over 120 hours to train, even on the CS-Storm, and the performance degraded considerably when using these models in a production environment with shorter document chunk sizes.

{{< figure library="1" src="cray_bloomsbury/accuracies.png" title="Figure 3: The graph shows the performance attained by different models on the development set of TriviaQA Wikipedia." >}}

The figure shows the performance attained by different models on the development set of TriviaQA Wikipedia. It is apparent that hyperparameter tuning enabled by fast training has allowed significant performance improvement. Furthermore, training on longer document chunks looks promising, but requires further experimentation.[^3] The effect of large effective batch sizes can also be seen here, with increases of 0.72% EM and 1.12% F1 against an equivalent model trained on a single worker.

The best model was submitted to the official TriviaQA model leader-board, which can be viewed [here](https://competitions.codalab.org/competitions/17208#results). The test set answers are withheld from the public release, allowing for fair testing of model performances. Cape's model received scores of 67.32% EM and 72.35% F1, setting a new state of the art in Wikipedia open-domain question answering. The test set also contains a verified subset, which has been curated by human annotators to be noise-free. On this subset the model scores 75.68% EM and 79.26% F1 indicating it answers an impressive ~80% of TriviaQA's questions correctly.


### Summing up

Since this project concluded, Bloomsbury AI has joined Facebook, and has released Cape Open-source to the community, including the best model from this project. They hope that it will be of benefit to many people, empowering them to create great, intelligent products. To try Cape out, please check out the project [here](https://github.com/bloomsburyai/cape-webservices). Pull Requests for functionality, demos, tutorials and documentation are encouraged! The team would like to thank Cray and Digital Catapult's Machine Intelligence Garage for their support, expertise and hardware resources, without which, this work would not have been possible.

### Footnotes

[^1]: for further details on training regime, see [Clark et al. 2017](https://arxiv.org/abs/1710.10723)

[^2]: Each worker maintains a copy of the model being trained and computes parameter updates for a mini-batch. The parameter updates for all the workers are then averaged together using a highly efficient ring-allreduce method. All the workers' models are then updated with the averaged parameter update. The overall effect is to increase the effective mini-batch size by a factor of the number of workers, i.e. 2 workers each computing updates with a mini-batch size of 64 training examples is equivalent to 1 worker with a mini-batch size of 128 training examples.

[^3]: Training runs were terminated after 120 hours. Learning curves for the Long chunk models indicated that training had not yet converged after this time, suggesting the long context model could have ended up performing best. The drawbacks of this model have already been mentioned however, and it could not be put into production, so further experiments with it were stopped.


