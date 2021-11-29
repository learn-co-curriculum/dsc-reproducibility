# Reproducibility

## Introduction

## Objectives

* Identifying the value of reproducibility in data science and science more broadly
* Identifying the components of reproducible data science
* Creating and exporting environment files with `conda`
* Creating and exporting requirements files with `pip`
* Providing data access

## Why Reproducibility Matters

### The Reproducibility Crisis

According to a [*Nature* study](https://www.nature.com/articles/533452a), the overwhelming majority of scientists believe there is a *reproducibility crisis* (also known as the "replication crisis"). Essentially this means that many scientific studies' results cannot be replicated by other researchers.

In some cases this is related to fabricated data or poor statistical practices like p-hacking, but the general consensus is that lack of documentation is the key obstacle. This is linked to the incentive structure in scientific research labs as well as logistical challenges with translating hands-on "wet lab" practices into clear documentation.

### Reproducibility in Data Science

Because the work of a data scientist is captured in a digital format of code and data, there are fewer logistical challenges. But data scientists still need to be careful to document their work!

[According to Professor Bill Howe](https://www.coursera.org/lecture/data-results/reproducibility-and-data-science-v27fG), Director of Research for Scalable Data Analytics at the UW eScience Institute, it's important to remember that *data science is still science*. And in an industry context, we can break down the reasons based on **internal** and **external** stakeholders.

#### Internal Stakeholders

If your data science work is being used for **decision support** and results in a negative outcome, you need to be able to backtrack and figure out what went wrong. This is only possible if you have accurately documented all of the steps you took.

Reproducibility is also important for **communication**. Your current teammates, future trainees, and even *yourself in the future* should be able to understand what was done and how to do it again.

#### External Stakeholders

Your work may be subject to regulations and it's important for **compliance** that you are able to replicate where you got your data and how you got your results.

## Components of Reproducible Data Science

In order to have a reproducible project, you need to have:

1. Published code
2. Instructions to re-create your environment
3. Instructions to access your data

### 1. Published Code

Luckily, one of the most important elements of reproducibility is something you have already been doing: **publishing your code on GitHub**!

(In industry you may use a different source control tool, but GitHub is the standard for open source portfolios so we require you to use it at Flatiron School.)

In addition to helping you back up your work and collaborate with team members, GitHub means that you can be confident that a particular result was actually produced by a particular segment of Python code.

Sometimes it can be tempting to skip this part if you're just working on a "one-off" project, but it's better to be safe than sorry in case you need to re-run an analysis in the future! Saving and committing your code can save you from a lot of headaches down the line.

### 2. Exported Environment

Imagine you are trying to spin up an old project on a new computer. You have the source code cloned from GitHub, but you keep getting errors like this:

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'sklearn'
```

So you go into the terminal and enter `conda install scikit-learn` or `pip install scikit-learn`, then you get errors like this:

```
sklearn/utils/deprecation.py:87: FutureWarning:
Function plot_confusion_matrix is deprecated; Function `plot_confusion_matrix` is deprecated in 1.0 and will be removed in 1.2. Use one of the class methods: ConfusionMatrixDisplay.from_predictions or ConfusionMatrixDisplay.from_estimator.
```

Or like this:

```
/tmp/ipykernel_145/2637478872.py:5: UserWarning: This figure was saved with matplotlib version 3.3.1 and is unlikely to function correctly.
```

In a modern data science stack, it's important to keep track of not only the list of packages used, but also the specific versions of those packages. Python data science packages are constantly updating in ways that often include exciting new features, but also can cause all of your code to break!

The two main ways to export your environment are using `conda` and `pip`. It's helpful to know how to use both tools because they are useful in different contexts.

#### Creating and Exporting Environment Files with `conda`

`conda` should already be somewhat familiar from your local environment setup earlier in the program. You cloned a repository containing an `environment.yml` file, then ran some commands in the terminal so that `learn-env` would be activated by default.

Now it's time to move beyond `learn-env`!

In your career you will likely encounter some situations where the environment has been prepared for you, similar to how `learn-env` was, if it's a larger project with multiple collaborators. But you'll also want to know how to build your own environment from scratch for "one-off" projects.

(Why not just use a mega-environment like `learn-env` every time? Because that involves installing a lot of extra packages that add unnecessary complexity and slow down deployment times.)

**`conda` environments are especially useful if you want to be able to have multiple environments installed on the same computer, since `conda` is both a tool for installing packages and a tool for managing environments.** In other words, if you have one project that uses Matplotlib 3.3.1 and another project that uses Matplotlib 3.5.0, `conda` will allow you to have both installed on your computer and switch between them as needed.

When starting a new project that uses a `conda` environment, you want to begin by creating a new environment for that project. Then as you need additional packages, `conda install` them. Finally, when the project is ready to be published, export the environment to an `environment.yml` file and push that file to GitHub.

An example `conda` workflow for an NLP project might look like:

1. `conda create --name nlp-env notebook scikit-learn matplotlib nltk`
    - This creates an environment named `nlp-env` which has the latest versions of Jupyter Notebook, scikit-learn, Matplotlib, and NLTK
2. `conda activate nlp-env`
    - This activates the newly-created environment. Consider opening up and editing your `~/.bash_profile` so that this is activated every time instead of `learn-env`
3. `conda install seaborn`
    - This is an example of installing a new package after the initial environment creation. It's fine to do this as many times as you need to until you have all of the packages you need
4. `conda env export environment.yml`
    - This creates a file called `environment.yml` in the current directory, which lists the requirements for your environment. Add, commit, and push this file to GitHub when it's ready

You can then repeat steps 3 and 4 as you realize that new packages are needed. Step 4 will overwrite the current `environment.yml` file, and you'll want to sync it with GitHub as appropriate.

Complete documentation for managing `conda` environments can be found [here](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#managing-environments).

#### Creating and Exporting Requirements Files with `pip`

The other main tool for installing packages you will encounter is `pip` (short for Package Installer for Python). Its functionality is more limited than `conda` but is widely used for deploying data projects (APIs, dashboards, web apps, etc.). `pip` requirements files are conventionally called `requirements.txt`.

Using `pip` is a bit more complicated than just `conda` because you need to create an environment first, then you can install things with `pip`. You'll see references online to using [`virtualenv`](https://virtualenv.pypa.io/en/latest/) or [`venv`](https://docs.python.org/3/library/venv.html) (in Python 3.3 an higher) for accomplishing this, but since you already have Anaconda installed we recommend that you just use `conda` for creating the environment.

(See [this article](https://www.anaconda.com/blog/understanding-conda-and-pip) for a longer explanation of the similarities and differences between `conda` and `pip`.)

An example `pip` workflow for an NLP project might look like:

1. `conda create --name nlp-env pip`
    - This creates an environment named `nlp-env` which contains only `pip`
2. `conda activate nlp-env`
    - In order to avoid "polluting" the global environment and to make sure that your `requirements.txt` doesn't have extraneous requirements listed, make sure you have an environment activated
3. `pip install notebook scikit-learn matplotlib nltk`
    - Installs the latest versions of Jupyter Notebook, scikit-learn, Matplotlib, and NLTK using `pip`
4. `pip install seaborn`
    - This is an example of installing a new package after the initial environment creation. It's fine to do this as many times as you need to until you have all of the packages you need
5. `pip freeze > requirements.txt`
    - This creates a file called `requirements.txt` in the current directory, which lists the requirements for your environment. Add, commit, and push this file to GitHub when it's ready

One of the platforms you can use to test out your `requirements.txt` is Binder:

https://mybinder.org/

It should allow you to launch in interactive version of your notebook directly in the browser!

Technically it should work with an `environment.yml` also, but `requirements.txt` seems to work better.

#### Exporting Requirements in Cloud Environments

If you are working in a cloud environment such as [Kaggle Notebooks](https://www.kaggle.com/docs/notebooks), [Google Colaboratory](https://colab.research.google.com/) (requires a Google Account to access), or [Azure Databricks](https://docs.microsoft.com/en-us/azure/databricks/libraries/), you will need to investigate how to install packages as needed.

Often this will simply mean running `! pip install` + the package name in a notebook cell, but there may be platform-specific requirements, such as [Kaggle](https://www.kaggle.com/questions-and-answers/36982) which requires you to adjust your preferences first.

When you are ready to save your work to GitHub, you'll want to use `pip freeze` just like if you were working locally. However it will depend on the file system whether you run `! pip freeze > requirements.txt` then save the file, or if you just run `! pip freeze` then copy the output into a file.

If you are unable to use `pip` or `conda` at all in your cloud environment, make sure you document the exact steps required to install the required packages with whatever system you used.

### 3. Data Access

The final piece needed to reproduce an analysis is having access to the data.

The easiest way to accomplish this is if the data is small enough to be stored in a file (such as a CSV) and pushed directly to GitHub. Then your code will work with your data automatically.

However that can be quite limiting! Some kinds of projects are very unlikely to have data small enough to fit in GitHub because there is a [hard limit of 100MB for files](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github). You might be able to add a subset of your data to GitHub as a workaround, but sometimes that's not possible. If this is the case, you have two options:

1. Scripted data download
2. Data download instructions

#### Scripted Data Download

The more-advanced, more-professional approach is to write code (Python or Bash) that will do the work of downloading the data as well as any unzipping and/or rearranging tasks.

For example, the `requests` library that we use for APIs can also be used to download files using Python ([example here](https://www.geeksforgeeks.org/downloading-files-web-using-python/)). Or the `curl` library can be used with Unix scripting ([documentation here](https://curl.se/docs/manual.html)). You can also use the Python [`shutil`](https://docs.python.org/3/library/shutil.html#module-shutil) or [`os`](https://docs.python.org/3/library/os.html#module-os) modules to move files around, or just use `mv` Unix commands.

If your data is not available via a typical HTTPS call, there is a library called `boto3` ([documentation here](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)) that can work with data stored in AWS S3 buckets. If you're using a different cloud storage approach, try searching online for techniques to download the files directly.

The idea of this script would be that someone could clone your repository and run a single line of Python or Bash code to get the data. This is the gold standard but also a high bar! Don't get tripped up if you're not able to do this right away, and try the "instructions" approach instead if your main audience is a future data scientist. If your deployed project needs to have data available and you aren't able to get a script working, try pushing a subset of data to GitHub instead.

#### Data Download Instructions

The less-advanced, more accessible approach is simply to write instructions for the user to get the necessary data.

Think of the audience for these instructions as yourself 6 months from now -- someone who has a strong general understanding of data science, but doesn't necessarily remember the ins and outs of wherever you got your data.

Here is a minimal example of data download instructions:

> This repository relies on data from the King County Department of Assessments. To download the data, first go to [this website](https://info.kingcounty.gov/assessor/DataDownload/default.aspx) and agree to the acknowledgement. Then click on the links to download **Real Property Sales (.ZIP)** and **Residential Building (.ZIP)**. This will download two `.zip` files to your computer, and might take a couple of minutes depending on your download speed. Unzip both using the relevant utility for your computer ([Mac](https://support.apple.com/guide/terminal/compress-and-uncompress-file-archives-apdc52250ee-4659-4751-9a3a-8b7988150530/mac), [Windows](https://support.microsoft.com/en-us/windows/zip-and-unzip-files-f6dde0a7-0fec-8294-e1d3-703ed85e7ebc), [Linux](https://www.howtogeek.com/414082/how-to-zip-or-unzip-files-from-the-linux-terminal/)). Then after unzipping you should see files named `EXTR_RPSale.csv` and `EXTR_ResBldg.csv`. Move these into the `data/` directory of this repository.

You might find it helpful to show your instructions to a peer to make sure that you haven't forgotten any crucial steps!

## Summary

Reproducibility is key for data science best practices. For your data science projects, make sure you publish your code on GitHub, create a file to help with replicating your environment, and include either a script or instructions for accessing the data.
