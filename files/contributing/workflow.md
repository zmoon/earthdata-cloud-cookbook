---
title: Contributing
execute:
  eval: false
---

## How to contribute to our book

We'll make our e-book using Quarto ([https://quarto.org](https://quarto.org/docs/getting-started/installation.html)). Your Quarto workflow can be from the Command Line (bash), Python, or R. Its book chapters can be many file types, including `.md` , `.ipynb`, `.Rmd`, and `.qmd`. In all cases narrative and prose can be written in markdown, and chapters without code to execute can be written in `.md`. This workflow can streamline collaboration for scientific & technical writing across programming languages. 

What is Quarto? Quarto builds from what RStudio learned from RMarkdown but enables different engines (Jupyter and knitr). It is both a Command Line Tool and R package. `.qmd` is a new filetype like `.Rmd` --- meaning it's a text file but coupled with an engine can execute code and be rendered as html, pdf, word, and beyond --- but for other languages like Python. Quarto can convert `.ipynb` files to and from `.md` and `.qmd` easily so you can develop and publish with collaborators that have different workflows. Once the book is "served" locally, `.md` files auto-update as you edit, and files with executable code can be rendered individually, and the behavior of different code chunks can be controlled and cached.

(Note: with Quarto, e-books and websites are very similarly structured, with e-books being set up for numbered chapters and references and websites set up for higher number of pages and organization. We can talk about our book as a book even as we explore whether book/website better suits our needs. This is assigned in `_quarto.yml`.)

## Setup

### Clone our repository locally

Clone `nasa-openscapes/quartobook-test` using your method of preference. Here is how to do this in bash:

```{bash}
git clone https://github.com/NASA-Openscapes/earthdata-cloud-cookbook 
cd earthdata-cloud-cookbook
```

### Load environment

Our repo is set up with libraries (currently Python but we can also add R) that's just for this project (creation using the [`renv`](https://rstudio.github.io/renv/) R package below). Loading this environment will give you what you need to contribute to the book, as well as build and publish it. But it will stay contained just here in this project so it won't interact with your other projects.

More details about environments, from [A Guide to Python's Virtual Environments](https://towardsdatascience.com/virtual-environments-104c62d48c54) (Sarmiento 2019):

> A virtual environment is a Python tool for **dependency management** and **project** **isolation**. They allow Python **site packages** (third party libraries) to be installed locally in an isolateddirectory for a particular project, as opposed to being installed globally (i.e. as part of a system-wide Python).

#### Command Line/Python

Depending on whether your python install is via `pip` or `conda` you will run the following code in the command line:

```{bash}
## pip approach:
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

## conda approach:
conda env create --name openscapes --file environment.yml  
conda activate openscapes
```

#### R

In R, we'll first need the `remotes` package to install the `renv` package from GitHub. Then we can restore the environment using `renv::restore()`.

```{r}
if (!requireNamespace("remotes")) install.packages("remotes") 
remotes::install_github("rstudio/renv") 
renv::restore()
```

### Install Quarto

Install the `quarto` library so you can interact with it via the command line or R. I've set mine up with both.

#### Command Line/Python:

In the command line, type:

```{bash}
pip install quarto 
```

#### R

In R you'll also need the [`reticulate`](https://rstudio.github.io/reticulate/) package. In In the R Console, type:

```{r}
install.packages("quarto") 
install.packages("reticulate")
```

## GitHub Workflow

First let's talk about the GitHub part of the workflow. The main steps of working with GitHub are to pull, (work), stage, commit, (pull), and push. A great resource on GitHub setup and collaboration: <https://happygitwithr.com/> (R-focused but fantastic philosophy and bash commands for setup, workflows, and collaboration).

We're going to use branches and follow a shared workflow: create a branch, work in your branch and commit regularly, and push to github often. When you're ready, create a pull request and we'll merge it into the main branch. 

### Branch setup and workflow

Create a new branch, then switch to that branch to work in. Then, connect it to github.com by pushing it "upstream" to the "origin repository". `git checkout -b branch-name` is a 1-step approach for `git branch branch-name` `git checkout branch-name` (read [more](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)). 

```{bash}
## create and switch to new branch
git checkout -b branch-name
```

Check which branch you're on anytime by not specifying a branch name:

```{bash}
git branch
```

Time for your Quarto workflow -- see below. Then when you're ready after you render the whole book, push back to GitHub. First you have to connect your branch to github.com (the "upstream origin") (`-u` below is short for `--set-upstream`)

```{bash}
## commit regularly as you work
git add --all 
git commit -m "my commit message here" 

## connect your branch to github.com and push
git push -u origin branch-name
```

Now it's on github, in a separate branch from main. You can go to <https://github.com/nasa-openscapes/quartobook-test> and do a pull request, and tag someone to review (depending on what you've done and what we've talked about).

TODO: Let's discuss this:

-   When the pull request is merged, delete the branch on github.com

-   Then also delete the branch locally:

```{bash}
git checkout main # switch to the main branch
git branch -d branch-hame
```

## Quarto Workflow

OK now we are setup and ready to work! The thing to do first is to "serve" (build) the book to make sure everything's working. (It's called "serve" because it's really a website that looks like a book).

The overall workflow will be to serve the book at the beginning, make edits and render your `.Rmd`/`.qmd` pages to view your edits as you go (`.md` are automatic) and then when you're ready to publish, you render the book with an additional command. Learn more about rendering here: <https://quarto.org/docs/computations/running-code.html#rendering>. From J.J. at RStudio:

> For `.Rmd` and `.qmd` files you need to render them (`.md` updates show on save because there is no render step). The reason Quarto doesn't render `.Rmd` and `.qmd` on save is that render could (potentially) be very long running and that cost shouldn't be imposed on you whenever you save.
>
> Here we are talking about the age old debate of whether computational markdown should be rendered on save when running a development server. Quarto currently doesn't do this to give the user a choice between an expensive render and a cheap save. See: <https://quarto.org/docs/websites/website-basics.html#workflow>.

The structure of the book is written in `_quarto.yml.` More description on this upcoming.

### Command Line/Python

To serve the book, run the following:

```{bash}
quarto serve
```

Paste the url from the console into your browser to see your updates.

Continue working, the `.md` files will refresh live! To refresh files with executable code, type:

```{bash}
quarto render jupyter-document.ipynb
```

To render and the whole book before publishing:

```{bash}
quarto render
```

### R

To the serve the book from R:

```{r}
quarto::quarto_serve()
```

Continue working, the `.md` files will refresh live! To refresh files with executable code, type:

```{r}
quarto::quarto_render("filename.ipynb")
```

To render and the whole book before publishing:

```{r}
quarto::quarto_render()
```

## Updating the environment

TODO!

From R: As we develop and add more package dependencies, re-run `renv::snapshot()` to update the environment. 

