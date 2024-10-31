---
title: The Jupyter Notebook
# date: 2017-01-05
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
# categories: [Examples]
# tags: [test, sample, docs]
weight: 130
---

![Jupyter](images/jupyterpreview.png)

The Jupyter Notebook is an open-source web application that allows you to create and share documents that contain live code, equations, visualizations and narrative text. Uses include: data cleaning and transformation, numerical simulation, statistical modeling, data visualization, machine learning, and much more.

## Confirm Installation
If you used the Anaconda Distribution for installing Python in Chapter~\ref{cha:sprint_zero} Jupyter should already be installed.  Confirm installation by running ```jupter --vesion```

```
$ jupyter --version
4.4.0
```

Otherwise, you can still install Jupyter as a Python package via pip.

```
$ pip install jupyter
```

## Running Jupyter
To start a Jupyter session, type ```jupyter notebook``` at the command prompt.

```
$ jupyter notebook
```

This will start a session and open your default browser with a list of all the available notebooks.

Use ***control-c*** to stop the Jupyter session.

## Markdown
Jupyter Notebooks use two main types of cells - markdown and code.

Markdown is a lightweight markup language that you can use to add formatting elements to plaintext text documents. Created by John Gruber in 2004, Markdown is now one of the world’s most popular markup languages.

Markdown can be used in Slack, GitHub comments, and many other areas where coders interact.  **This book is written in Markdown.**

If you are interested in learning more about the Markdown language, you can checkout the [Markdown Guide](https://www.markdownguide.org).

## Code Cells
Jupyter Notebooks contain cells.  As mentioned above, those cells are typically either markdown or code.  Code cells usually contain snippets of code, such as Python, that can be executed sequentially or independently.  This allows the users to make incremental changes and re-run a block of code with ease.
