# How to
How to use GitHub, Travis CI, CodeCov, SonarQube, and Doxygen using c with [c repository](https://github.com/laurelmcintyre/c) as example

## GitHub
[How to set up GitHub account, create SSH key for GitHub, and clone repository to local computer](https://github.com/laurelmcintyre/documentation/blob/gh-pages/github_instructions.md)

### The Shell and Terminal
[Shell/Terminal description and Terminal commands](https://github.com/laurelmcintyre/documentation/blob/gh-pages/shell_and_terminal.md)

### Markdown
[Markdown formatting description](https://github.com/laurelmcintyre/documentation/blob/gh-pages/markdown.md)

## Travis CI
[Continuous Integration description, Travis CI setup, build passing badge](https://github.com/laurelmcintyre/documentation/blob/gh-pages/travis_ci.md)

### Example of a .travis.yml file
[Link to example c repository](https://github.com/laurelmcintyre/c)
* `language: c` means that the project is written in c.
* `script: ${CC} <file-name>.c -o <file-name>` runs the compiler gcc or clang on a file
*  Specify the compiler which the script runs on

        compiler:
          - gcc
          - clang 

## Create a Personal Access Token OR Create a Deploy Key for the Repo
[Create personal access token](https://github.com/laurelmcintyre/documentation/blob/gh-pages/gh_personal_access_token.md) OR [Create an SSH deploy key](https://github.com/laurelmcintyre/documentation/blob/gh-pages/deploy_key.md) -- SSH deploy key is more secure

## Cache
(Cache description, additions to .travis.yml file)[https://github.com/laurelmcintyre/documentation/blob/gh-pages/cache.md]
      
## Code Coverage
[CodeCov, .codecov.yml file, and code coverage badge](https://github.com/laurelmcintyre/documentation/blob/gh-pages/codecov.md

### Additions to .travis.yml file needed for Code Coverage
    #sudo:required and dist:trusty both specify a trusty build environment
    sudo: required

    dist: trusty
    
    #${CC} is replaced by gcc and clang cmpilers
    #runs coverage on files
    script:
      - ${CC} --coverage -c <file_name>.c -o <file_name>.o
      - ${CC} --coverage <file_name>.o -o <file_name>
      - ./<file_name>
    
    #if the compiler is clang and the build passes, then it uploads reports to CodeCov
    after_success:
      - if [ ${CC} = clang ]; then
          bash <(curl -s https://codecov.io/bash) -F ${CC} --gcov-exec "llvm-cov gcov";
        else
          bash <(curl -s https://codecov.io/bash) -F ${CC};
        fi
          
## SonarQube
[SonarCloud, sonar-project.properties file, quality gate badge](https://github.com/laurelmcintyre/documentation/blob/gh-pages/sonarcloud.md)

You will need to add these lines to your .travis.yml file:

      addons:
        sonarcloud:
          organization: "<organization_key>"

      script:
        - sonar-scanner

      cache:
        directories:
          - $HOME/.sonar
          
          
## Doxygen
[How to set up Doxygen, generateDocumentationAndDeploy.sh file, additions to .travis.yml file](https://github.com/laurelmcintyre/documentation/blob/gh-pages/doxygen.md)
