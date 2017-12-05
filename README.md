### LaTeX + Git + Travis &rightarrow; release pdf

[![Build Status](https://api.travis-ci.org/PHPirates/travis-ci-latex-pdf.svg)](https://travis-ci.org/PHPirates/travis-ci-latex-pdf)

Write LaTeX, push to git, let Travis automatically try to build your file and release a pdf automatically to GitHub releases when the commit was tagged.

Installs a minimal TeX Live installation on Travis, and compiles with pdflatex.

# Features

* Add the packages you use to the install script.
* Same for other document classes.
* File inclusion.
* Caches TeX Live and packages, also speeds up build time.
* Works with BiBTeX.

# To do
* Table of contents
* Index

# Setup

* Go to [Travis CI](https://travis-ci.org) and enable the repository which contains a LaTeX file that you want to build.
* Copy the files `.travis.yml`, `texlive_install.sh` and `texlive/texlive.profile` and specify the right tex file in the `.travis.yml`.
* Optional: commit and push to check that the file builds.

## To automatically deploy pdfs to GitHub release
### First time setup
* We will generate a GitHub OAuth key so Travis can push to your releases, with the important difference (compared to just gettting it via GitHub settings) that it's encryped so you can push it safely.
* (Windows) [Download ruby](https://rubyinstaller.org/downloads/) and at at end of the installation make sure to install MSYS including development kit.
* Run `gem install travis --no-rdoc --no-ri` to install the Travis Command-line Tool.
### For every new project
* Remove the `deploy` section in the `.travis.yml` or use `--force` in the next command.
* Go to the directory of your repository and run `travis setup releases`. Specify your GitHub credentials, and fill in anything for File to Upload. If it hangs in Git Bash, try to use Command Prompt.
* Replace everything below your encryped api key with
```yml
  file:
  - ./_build/nameofmytexfile.pdf
  skip_cleanup: true
  on:
    tags: true
    branch: master
```
* If you are ready to release, just tag and push.
* If you want the badge in your readme, just copy the code below to your readme and change the links.
```markdown
[![Build Status](https://api.travis-ci.org/username/reponame.svg)](https://travis-ci.org/username/reponame)
```
* Probably you want to edit settings on Travis to not build both on pull request and branch updates, and cancel running jobs if new ones are pushed.

#### Notes
* If you want to build a private project, if you are a student you can use [travis-ci.com](https://travis-ci.com). Beware that you need a token to include the build status image in your readme, get the correct url by clicking on the build status on travis-ci.com.
* Otherwise you could try SemaphoreCI, currently they give 100 private builds per month for free. If you do, it would be great if you could report back!

Adapted from [harshjv's blog](https://harshjv.github.io/blog/setup-latex-pdf-build-using-travis-ci/), and thanks to [jackolney](https://github.com/jackolney/travis-ci-latex-pdf) for all his attempts to put it into practice.
Also see harshjv's original [blog post](https://harshjv.github.io/blog/document-building-versioning-with-tex-document-git-continuous-integration-dropbox/).