---
theme: default
title: MLOps Open Source Contributions
class: text-center
highlighter: shiki
fonts:
  sans: Roboto
  serif: Roboto Slab
  mono: Fira Code
  local: Fira Code, Roboto, Roboto Slab
transition: slide-left
favicon: https://raw.githubusercontent.com/zeyaddeeb/zeyaddeeb.github.io/master/favicon.ico
mdc: false
---

## Hitchhiker's Guide  to MLOps <br/> Open Source Contributions

<p class="mt-10 op30">
by Zeyad Deeb
</p>
---
transition: fade-out
---

<div class="h-full w-full px-20">
<h1> Pick your project </h1>

  <img src="https://landscape.lfai.foundation/images/landscape.png" alt=""/>

[Source](https://landscape.lfai.foundation/)

</div>

---
transition: fade-out
layout: image
image: https://polyaxon.com/static/efd3132199fc1b888048b743dda8e6d0/a3af8/polyaxon-logo-cover.png
---


---
transition: fade-out
---

# Polyaxon

Reproduce, automate, and scale your data science workflows with production-grade MLOps tools.

<div class="mt-10" v-click>

### All major libraries supported

`Pytorch, Tensorflow, Keras, Scikit Learn`
</div>

<br>

<v-click>

### Provider agnostic

Deploy Polyaxon on-premise or on your favorite  <span v-mark.red="2">cloud provider</span>
and especially <span v-mark.circle.orange="2">Kubernetes!</span>

</v-click>


<div class="mt-30" v-click="4">

[Learn More](https://polyaxon.com/)

</div>


---
transition: fade-out
layout: image-right
image: /tags.png
backgroundSize: 35em
---

# Just... Start

You must have [docker](https://www.docker.com/), [homebrew](https://brew.sh/) and [kubectl](https://kubernetes.io/docs/reference/kubectl/) installed first

<v-click>

```bash
pip install -U polyaxon
```
</v-click>


<div class="mt-5" v-click>

```bash
polyaxon sandbox
```
</div>


<div class="mt-5" v-click>

Go to your <span v-mark.red> [http://localhost:8080](http://localhost:8080) </span> in your favorite browser

</div>


<div class="mt-10 op40" v-click>

If you want to run your own [minikube](https://minikube.sigs.k8s.io/docs/) cluster. Follow the docs [here](https://polyaxon.com/docs/intro/quick-start/)

</div>

---
layout: two-cols
---

# Spacy

<div class="w-100">

```python
import random
import spacy
from spacy.util import minibatch, compounding

from polyaxon import tracking

TRAIN_DATA = [
    ("Polyaxon is a Data Science Platform", {"entities": [(0, 8, "ORG")]}),
]


def train_model(n_iter: int = 100) -> None:

...

if __name__ == "__main__":

    tracking.init()

    _ = train_model(n_iter=50)

    custom_model = spacy.load("custom_spacy_model")

    for text, _ in TRAIN_DATA:
        doc = custom_model(text)
        print("Entities", [(ent.text, ent.label_) for ent in doc.ents])
        print("Tokens", [(t.text, t.ent_type_, t.ent_iob) for t in doc])

    tracking.log_dir_ref("custom_spacy_model")
```
</div>

::right::

# Deployment


```yaml
version: 1.1
kind: component
tags: [examples, spacy]

run:
  kind: job
  init:
  - git: {"url": "https://github.com/polyaxon/polyaxon-examples"}
  container:
    image: polyaxon/polyaxon-spacy-examples
    command: ["python", "-u", "{{ globals.artifacts_path }}/polyaxon-examples/in_cluster/spacy/train/model.py"]
```

<br class="mt-15"/>

You must use docker to package all your code, see referece [`Dockerfile`](https://github.com/polyaxon/polyaxon-examples/blob/master/in_cluster/spacy/train/Dockerfile)

---
transition: fade-out
layout: image
image: /filter-runs.png
---

---
layout: center
class: text-center
---

# Thank you