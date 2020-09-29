+++
title = "Unsupervised Question Decomposition for Question Answering"
date = 2020-02-22T00:00:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Ethan Perez", "**Patrick Lewis**", "Wen-tau Yih", " Kyunghyun Cho", " Douwe Kiela"]

# Publication type.
# Legend:
# 0 = Uncategorized
# 1 = Conference paper
# 2 = Journal article
# 3 = Manuscript
# 4 = Report
# 5 = Book
# 6 = Book section
publication_types = ["1"]

# Publication name and optional abbreviated version.
publication = "EMNLP 2020"
publication_short = "EMNLP 2020"

# Abstract and optional shortened version.
abstract = "We aim to improve question answering (QA) by decomposing hard questions into easier sub-questions that existing QA systems can answer. Since collecting labeled decompositions is cumbersome, we propose an unsupervised approach to produce sub-questions. Specifically, by leveraging >10M questions from Common Crawl, we learn to map from the distribution of multi-hop questions to the distribution of single-hop sub-questions. We answer sub-questions with an off-the-shelf QA model and incorporate the resulting answers in a downstream, multi-hop QA system. On a popular multi-hop QA dataset, HotpotQA, we show large improvements over a strong baseline, especially on adversarial and out-of-domain questions. Our method is generally applicable and automatically learns to decompose questions of different classes, while matching the performance of decomposition methods that rely heavily on hand-engineering and annotation."
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
url_pdf = "https://arxiv.org/pdf/2002.09758.pdf"
url_preprint = "https://arxiv.org/abs/2002.09758"
url_code = "https://github.com/facebookresearch/UnsupervisedDecomposition"
#url_dataset = "https://github.com/facebookresearch/UnsupervisedDecomposition"
#url_project = "https://github.com/facebookresearch/UnsupervisedQA"
# url_slides = "#"
# url_video = "#"
#url_poster = "#"
# url_source = "#"

# Custom links (optional).
#   Uncomment line below to enable. For multiple links, use the form `[{...}, {...}, {...}]`.
#url_custom = [{name = "Website", url = "https://github.com/facebookresearch/UnsupervisedDecomposition"}]

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
