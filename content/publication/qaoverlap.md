+++
title = "Question and Answer Test-Train Overlap in Open-Domain Question Answering Datasets"
date = 2020-08-07T00:00:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["**Patrick Lewis**", "Pontus Stenetorp", "Sebastian Riedel"]
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
publication = "EACL 2021"
publication_short = **Best Paper**, EACL 2021"

# Abstract and optional shortened version.
abstract = "Ideally Open-Domain Question Answering models should exhibit a number of competencies, ranging from simply memorizing questions seen at training time, to answering novel question formulations with answers seen during training, to generalizing to completely novel questions with novel answers. However, single aggregated test set scores do not show the full picture of what capabilities models truly have. In this work, we perform a detailed study of the test sets of three popular open-domain benchmark datasets with respect to these competencies. We find that 60-70% of test-time answers are also present somewhere in the training sets. We also find that 30% of test-set questions have a near-duplicate paraphrase in their corresponding training sets. Using these findings, we evaluate a variety of popular open-domain models to obtain greater insight into what extent they can actually generalize, and what drives their overall performance. We find that all models perform dramatically worse on questions that cannot be memorized from training sets, with a mean absolute performance difference of 63% between repeated and non-repeated data. Finally we show that simple nearest-neighbor models out-perform a BART closed-book QA model, further highlighting the role that training set memorization plays in these benchmarks"
abstract_short = "" 
# Featured image thumbnail (optional)
image_preview = ""

# Is this a selected publication? (true/false)
selected = true

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
url_pdf = "https://arxiv.org/abs/2008.02637"
url_preprint = "https://arxiv.org/abs/2008.02637"
#url_code = "https://github.com/facebookresearch/dpr"
url_dataset = "https://github.com/facebookresearch/qa-overlap"
url_project = "https://github.com/facebookresearch/qa-overlap"
# url_slides = "#"
# url_video = "#"
#url_poster = "#"
# url_source = "#"

# Custom links (optional).
#   Uncomment line below to enable. For multiple links, use the form `[{...}, {...}, {...}]`.
url_custom = [{name = "Website", url = "https://github.com/facebookresearch/qa-overlap"}]

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
