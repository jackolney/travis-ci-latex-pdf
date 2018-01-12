### LaTeX + Git + Travis &rightarrow; release pdf

[![Build Status](https://api.travis-ci.org/PHPirates/travis-ci-latex-pdf.svg)](https://travis-ci.org/PHPirates/travis-ci-latex-pdf)

Write LaTeX, push to git, let Travis automatically try to build your file and release a pdf automatically to GitHub releases when the commit was tagged.

Installs a minimal TeX Live installation on Travis, and compiles with pdflatex.
This repo contains:
- The TeX Live install script `texlive_install.sh` including profile `texlive/texlive.profile` (specifies for example the TeX Live scheme)
- A Travis configuration file
- Demonstration LaTeX files in `src/`

# Features

* Add the extra packages you use which are not included in the TeX Live basic scheme to the install script.
* The currently used package index is [here](http://ctan.mirrors.hoobly.com/systems/texlive/tlnet/archive/).
* Same for other document classes.
* Supports file inclusion.
* Caches TeX Live and packages, also speeds up build time.
* Works with (at least) BiBTeX.

# How to use continuous integration for your LaTeX?

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
* Besides the list of packages that get installed in `texlive_install.sh`, you can see a list of packages in `main.tex` which you can all use with this install.
* If you want to build a private project, if you are a student you can use [travis-ci.com](https://travis-ci.com). Beware that you need a token to include the build status image in your readme, get the correct url by clicking on the build status on travis-ci.com.
* Otherwise you could try SemaphoreCI, currently they give 100 private builds per month for free. If you do, it would be great if you could report back!

I also put some of these instructions on the [TeX Stackexchange](https://tex.stackexchange.com/questions/398830/how-to-build-my-latex-automatically-with-pdflatex-using-travis-ci/398831#398831).

In the end the install script was completely rewritten based on the [LaTeX3 build file](https://github.com/latex3/latex3/blob/master/support/texlive.sh).

Some original thoughts from [harshjv's blog](https://harshjv.github.io/blog/setup-latex-pdf-build-using-travis-ci/), and thanks to [jackolney](https://github.com/jackolney/travis-ci-latex-pdf) for all his attempts to put it into practice.
Also see harshjv's original [blog post](https://harshjv.github.io/blog/document-building-versioning-with-tex-document-git-continuous-integration-dropbox/).
