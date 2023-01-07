+++
title = "Task-aware Retrieval with Instructions"
date = 2022-11-16T00:00:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Akari Asai", "Timo Schick", "**Patrick Lewis**", "Xilun Chen", "Gautier Izacard", "Sebastian Riedel", "Hannaneh Hajishirzi", "Wen-tau Yih"]

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
publication = "Preprint"
publication_short = "Preprint"

# Abstract and optional shortened version.
abstract="We study the problem of retrieval with instructions, where users of a retrieval system explicitly describe their intent along with their queries. We aim to develop a general-purpose task-aware retrieval system using multi-task instruction tuning, which can follow human-written instructions to find the best documents for a given query. We introduce the first large-scale collection of approximately 40 retrieval datasets with instructions, BERRI, and present TART, a multi-task retrieval system trained on BERRI with instructions. TART shows strong capabilities to adapt to a new retrieval task via instructions and advances the state of the art on two zero-shot retrieval benchmarks, BEIR and LOTTE, outperforming models up to three times larger. We further introduce a new evaluation setup, X^2-Retrieval to better reflect real-world scenarios, where diverse domains and tasks are pooled and a system needs to find documents aligning users' intents. In this setup, TART significantly outperforms competitive baselines, further demonstrating the effectiveness of guiding retrieval with instructions."
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
url_pdf = "https://arxiv.org/pdf/2211.09260.pdf"
url_preprint = "https://arxiv.org/abs/2211.09260"
#url_code = "https://github.com/facebookresearch/dpr"
#url_dataset = "https://github.com/facebookresearch/atlas"
#url_project = "https://github.com/facebookresearch/atlas"
# url_slides = "#"
# url_video = "#"
#url_poster = "#"
# url_source = "#"

# Custom links (optional).
#   Uncomment line below to enable. For multiple links, use the form `[{...}, {...}, {...}]`.
#url_custom = [{name = "Website", url = "https://github.com/facebookresearch/atlas"}]

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
