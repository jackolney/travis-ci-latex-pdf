### LaTeX + Git + Travis -> .pdf

[![Build Status](https://api.travis-ci.org/PHPirates/travis-ci-latex-pdf.svg)](https://travis-ci.org/PHPirates/travis-ci-latex-pdf)

Write LaTeX, push to git, integrate with Travis, release a pdf.

# Setup

* Go to [Travis CI](https://travis-ci.org) and enable the repository which contains a LaTeX file that you want to build.
* Copy the `.travis.yml` file and specify the right tex file there, as well as the pdf that you want to have.
* Optional: commit and push to check that the file builds.

## To automatically deploy pdfs to GitHub release
* We will generate a GitHub OAuth key so Travis can push to your releases, with the important difference (compared to just gettting it via GitHub settings) that it's encryped so you can push it safely.
* (Windows) [Download ruby](https://rubyinstaller.org/downloads/) and at at end of the installation make sure to install MSYS including development kit.
* Run `gem install travis --no-rdoc --no-ri` to install the Travis Command-line Tool.
* Remove the `deploy` section in the `.travis.yml` or use `--force` in the next command.
* Go to the directory of your repository and run `travis setup releases`. Specify your GitHub credentials.
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
* Currently if you want to include other files, do so with `../src/filetoinclude` if the file is in the same directory. This is because this will go up one directory and down one instead of trying to find the file in the directory from where the `pdflatex` command is executed. I'd consider this buggy behavior of the `-output-directory` option.
* If you want to build private project, you could try SemaphoreCI, currently they give 100 private builds per month for free. If you do, it would be great if you could report back!

Adapted from [harshjv's blog](https://harshjv.github.io/blog/setup-latex-pdf-build-using-travis-ci/), and thanks to [jackolney](https://github.com/jackolney/travis-ci-latex-pdf) for all his attempts to put it into practice.
Also see harshjv's original [blog post](https://harshjv.github.io/blog/document-building-versioning-with-tex-document-git-continuous-integration-dropbox/).