### LaTeX + Git + Travis -> .pdf

[![Build Status](https://api.travis-ci.org/PHPirates/travis-ci-latex-pdf.svg)](https://travis-ci.org/PHPirates/travis-ci-latex-pdf)

Write LaTeX, push to git, integrate with Travis, release a pdf.

# Setup

* Go to [Travis CI](https://travis-ci.org) and enable the repository which contains a LaTeX file that you want to build.
* Copy the `.travis.yml` file and specify the right tex file there, as well as the pdf that you want to have.
* Commit and push to check that the file builds.

## To automatically deploy pdfs to GitHub release
* We will generate a GitHub OAuth key so Travis can push to your releases, with the important difference (compared to just gettting it via GitHub settings) that it's encryped so you can push it safely.
* (Windows) [Download ruby](https://rubyinstaller.org/downloads/) and at at end of the installation make sure to install MSYS including development kit.
* Run `gem install travis --no-rdoc --no-ri` to install the Travis Command-line Tool.
* Remove the `deploy` section in the `.travis.yml` or use `--force` in the next command.
* Run `travis setup releases`.
* Replace everything below your encryped api key with
```
  file:
  - ./_build/nameofmytexfile.pdf
  skip_cleanup: true
  on:
    tags: true
    branch: master
```
* If you are ready to release, just tag and push.

Adapted from [harshjv's blog](https://harshjv.github.io/blog/setup-latex-pdf-build-using-travis-ci/), and thanks to [jackolney](https://github.com/jackolney/travis-ci-latex-pdf) for all his attempts to put it into practice.
Also see harshjv's original [blog post](https://harshjv.github.io/blog/document-building-versioning-with-tex-document-git-continuous-integration-dropbox/).