## Doxygen 
[Doxygen](http://www.stack.nl/~dimitri/doxygen/) is a tool for generating documentation for code in several different languages, mainly c or c++. The documentation can be displayed on a webpage browser. Download Doxygen to your computer, navigate to a directory, and run `doxygen -g`. This will create a Doxyfile. Then `open Doxyfile` to get the template and standard settings for a Doxyfile. Put this in your GitHub repo as DOXYFILE. Most of the configurations do not need to be changed, but make sure to change the project name to your repo name. Also, make sure `INPUT = ../..` (two directories up) and `GENERATE_HTML = YES`. Next, create a gh-pages branch of the repo by going to the repo settings, and under "GitHub Pages" it should say "Source" -- click on it and switch the branch to master.

The first step to run Doxygen is to create a shell file called generateDocumentationAndDeploy.sh. The .travis.yml file will reference this file, but having a separate shell file means that all of this source code does not have to be in .travis.yml. The ${TRAVIS_REPO_SLUG} variable refers to username/repo_name, so it does not have to be changed for every project.

    echo 'Setting up the script...'

    set -e
    
    #make a directory called code_docs and navigaate to it
    mkdir code_docs
    cd code_docs

    #clone the gh-pages branch of the repo and navigate there
    git clone -b gh-pages https://github.com/${TRAVIS_REPO_SLUG}
    cd ${TRAVIS_REPO_SLUG##*/}

    git config --global push.default simple
    git config user.name "Travis CI"
    git config user.email "travis@travis-ci.org"

    rm -rf *

    echo "" > .nojekyll

    echo 'Generating Doxygen code documentation...'

    doxygen $DOXYFILE 2>&1 | tee doxygen.log

    #if the files are generated in html, add them to GitHub
    if [ -d "html" ] && [ -f "html/index.html" ]; then

        echo 'Uploading documentation to the gh-pages branch...'

        git add --all

        git commit -m "Deploy code docs to GitHub Pages Travis build: ${TRAVIS_BUILD_NUMBER}" -m "Commit: ${TRAVIS_COMMIT}"

    #force push the documentation to GitHub -- the gh-pages branch gets rewritten with every commit
    #if one of the above conditions is not met, the documentation will not push to GitHub
        git push --force "git@github.com:${TRAVIS_REPO_SLUG}" > /dev/null 2>&1
    else
        echo '' >&2
        echo 'Warning: No documentation (html) files have been found!' >&2
        echo 'Warning: Not going to push the documentation to GitHub!' >&2
        exit 1
    fi
      
The .travis.yml requires changes as well:

    #travis will ignore the gh-pages branch because the auto-generated Doxygen files are pushed there
    branches:
      except:
        - gh-pages

    env:
      global:
        - DOXYFILE: $TRAVIS_BUILD_DIR/DOXYFILE

    #install doxygen packages
    addons:
      apt:
        packages:
          - doxygen
          - doxygen-doc
          - doxygen-latex
          - doxygen-gui
          - graphviz

    #refers to shell file that was just made
    #only run the shell file if it is the first build, aka the first compiler
    after_success:
      - chmod +x generateDocumentationAndDeploy.sh
      - if [[ ${TRAVIS_JOB_NUMBER} = *.1 ]]; then ./generateDocumentationAndDeploy.sh; fi

         
To display some form of documentation, add comments to the top of the file being documented. For example, for a simple hello world function, the comment could look like this (must start with `brief`), which would be published as "a helloworld program in c":

      /// \file

      /** \brief a helloworld program in c
       */

Doxygen documentation should publish on the gh-pages branch. To find the documentation displayed on a web browser, go into repo settings and click the link under "Your site is published at https://<username>.github.io/<repo_name>/". Add html after <repo_name>/ to see the published documentation.
