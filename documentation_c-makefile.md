# How to
How to use GitHub, Travis CI, CodeCov, SonarQube, Doxygen, and Make using c with [c-makefile repository](https://github.com/laurelmcintyre/c-makefile) as example

## GitHub
[How to set up GitHub account, create SSH key for GitHub, and clone repository to local computer](https://github.com/laurelmcintyre/documentation/blob/gh-pages/github_instructions.md)

### The Shell and Terminal
[Shell/Terminal description and Terminal commands](https://github.com/laurelmcintyre/documentation/blob/gh-pages/shell_and_terminal.md)

### Markdown
[Markdown formatting description](https://github.com/laurelmcintyre/documentation/blob/gh-pages/markdown.md)

## Travis CI
[Continuous Integration description, Travis CI setup, build passing badge](https://github.com/laurelmcintyre/documentation/blob/gh-pages/travis_ci.md)

### Example of a .travis.yml file
[Link to example c-makefile repository](https://github.com/laurelmcintyre/c-makefile)
* `language: c` means that the project is written in c.
* `script: ${CC} <file-name>.c -o <file-name>` runs the compiler gcc or clang on a file
*  Specify the compiler which the script runs on

        compiler:
          - gcc
          - clang 

## Create a Personal Access Token OR Create a Deploy Key for the Repo
[Create personal access token](https://github.com/laurelmcintyre/documentation/blob/gh-pages/gh_personal_access_token.md) OR [Create an SSH deploy key](https://github.com/laurelmcintyre/documentation/blob/gh-pages/deploy_key.md) -- SSH deploy key is more secure

## Cache
[Cache description, additions to .travis.yml file](https://github.com/laurelmcintyre/documentation/blob/gh-pages/cache.md)
      
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
    
    #if the build passes, it uploads reports to CodeCov
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
          organization: "<username>-github"

      script:
        - sonar-scanner

      cache:
        ccache: true
        directories:
          - $HOME/.sonar
          
## Doxygen
[How to set up Doxygen, generateDocumentationAndDeploy.sh file, additions to .travis.yml file](https://github.com/laurelmcintyre/documentation/blob/gh-pages/doxygen.md)

## Makefile
The Makefile contains shell commands that direct make on how to recompile a program. Some commands previously in the .travis.yml file will be redirected into the Makefile, so some lines in the .travis.yml file will need to be deleted and replaced. First, make a Makefile with the following content:

      #name of the program
      NAME=<file_name>

      #other c file to compile
      EXTRA_SRC=

      #what should be done by default - 1st target
      all: $(NAME)

      #generate a list of object (.o) files
      OBJS=$(patsubst %.c,%.o,$(NAME).c $(EXTRA_SRC))

      #main program depend on all objects, rest is done by implicit rules
      $(NAME): $(OBJS)
         ${CC} ${LDFLAGS} -o ${NAME} ${OBJS} ${LIBS}

      #clean up rule
      clean:
         rm -f $(NAME) $(OBJS)

      test: $(NAME)
         ./$(NAME)

      #all, clean, test are phony rules and should always run
      .PHONY: all clean test

Next, in the .travis.yml file, delete these existing script lines:

    script:
      - ${CC} --coverage -c <file_name>.c -o <file_name>.o
      - ${CC} --coverage <file_name>.o -o <file_name>
      - ./<file_name>
        
 Because these commands are now part of the MakeFile, change the script lines to:
 
    script:
      - make CFLAGS="--coverage" LDFLAGS="--coverage"
      - make test
