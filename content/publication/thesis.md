+++
title = "PhD Thesis: Improving Neural Question Answering with Retrieval and Generation"
date = 2021-12-17T00:00:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["**Patrick Lewis**"]

# Publication type.
# Legend:
# 0 = Uncategorized
# 1 = Conference paper
# 2 = Journal article
# 3 = Manuscript
# 4 = Report
# 5 = Book
# 6 = Book section
publication_types = ["5"]

# Publication name and optional abbreviated version.
publication = ""
publication_short = ""

# Abstract and optional shortened version.
abstract = "Text-based Question Answering (QA) is a subject of interest both for its practical applications, and as a test-bed to measure the key Artificial Intelligence competencies of Natural Language Processing (NLP) and the representation and application of knowledge. QA has progressed a great deal in recent years by adopting neural networks, the construction of large training datasets, and unsupervised pretraining. Despite these successes, QA models require large amounts of hand-annotated data, struggle to apply supplied knowledge effectively, and can be computationally ex- pensive to operate. In this thesis, we employ natural language generation and information retrieval techniques in order to explore and address these three issues. We first approach the task of Reading Comprehension (RC), with the aim of lifting the requirement for in-domain hand-annotated training data. We describe a method for inducing RC capabilities without requiring hand-annotated RC instances, and demonstrate performance on par with early supervised approaches. We then explore multi-lingual RC, and develop a dataset to evaluate methods which enable training RC models in one language, and testing them in another. Second, we explore open-domain QA (ODQA), and consider how to build mod- els which best leverage the knowledge contained in a Wikipedia text corpus. We demonstrate that retrieval-augmentation greatly improves the factual predictions of large pretrained language models in unsupervised settings. We then introduce a class of retrieval-augmented generator model, and demonstrate its strength and flexibility across a range of knowledge-intensive NLP tasks, including ODQA. Lastly, we study the relationship between memorisation and generalisation in ODQA, developing a behavioural framework based on memorisation to contextualise the performance of ODQA models. Based on these insights, we introduce a class of ODQA model based on the concept of representing knowledge as question- answer pairs, and demonstrate how, by using question generation, such models can achieve high accuracy, fast inference, and well-calibrated predictions."
abstract_short = "" 
# Featured image thumbnail (optional)
image_preview = ""

# Is this a selected publication? (true/false)
selected = false

# Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's filename without extension.
#   E.g. `projects = ["deep-learning"]` references `content/project/deep-learning.md`.
#   Otherwise, set `projects = []`.
projects = []

# Tags (optional).
#   Set `tags = []` for no tags, or use the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = []

# Links (optional).
url_pdf = "https://discovery.ucl.ac.uk/id/eprint/10151750/"
#url_preprint = "https://arxiv.org/abs/1906.04980"
#url_code = "https://github.com/facebookresearch/UnsupervisedQA"
#url_dataset = "https://github.com/facebookresearch/UnsupervisedQA"
#url_project = "https://github.com/facebookresearch/UnsupervisedQA"
# url_slides = "#"
# url_video = "#"
#url_poster = "#"
# url_source = "#"

# Custom links (optional).
#   Uncomment line below to enable. For multiple links, use the form `[{...}, {...}, {...}]`.
#url_custom = [{name = "Website", url = "https://github.com/facebookresearch/UnsupervisedQA"}]

# Digital Object Identifier (DOI)
doi = ""

# Does this page contain LaTeX math? (true/false)
math = true

# Does this page require source code highlighting? (true/false)
highlight = true

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
[header]
image = ""
caption = ""

+++
