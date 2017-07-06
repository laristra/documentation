## SonarQube
SonarQube is meant to improve code quality. It progresses through a series of conditions (the default conditions can be set) which must all be met in order for a project to pass. For example, in order to pass, a default project must have code coverage greater than 80% and a maintainability rating, reliability rating, and security rating all equal to A. This default setting applies to all future projects unless changed. SonarQube checks for bugs, vulnerabilities, code smells (parts of code which indicate bigger, underlying problem with the code), and duplications. To make a SonarQube account, log in through your GitHub account. Then, go to "My Account" in SonarQube, click on "Security," and "Generate Token." Go to Travis project settings and enter the token into Environmental Variables and name it SONARQUBE_TOKEN. Travis has an [instruction page](https://docs.travis-ci.com/user/sonarqube/) on how to configure the .travis.yml file. The file will require the organization key, which can be found under your username on the Account Settings on sonarcloud (it should be username-github). You will need to add lines to your .travis.yml file ([in repo documentation pages](https://github.com/laurelmcintyre/documentation/blob/gh-pages/README.md)):
          
Then, make a sonar-project.properties file:
    
      sonar.projectKey=<project_name>:master

      sonar.projectName=<project_name>

      sonar.projectVersion=1.0

      sonar.sources=.

      sonar.sourceEncoding=UTF-8

### How to Post Quality Gate Badge on GitHub
In Markdown, the format for a Quality Gate Badge is `[![Quality Gate](https://sonarqube.com/api/badges/gate?key=<repo_name>%3Amaster)](https://sonarqube.com/dashboard/id=<repo_name>%3Amaster)`. Type it into the README page.
