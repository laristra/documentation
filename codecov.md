## Code Coverage
[Code coverage](http://codecov.io/) shows what percent of code is being tested by Travis in builds. A high percentage is ideal to guard against bugs. To create a code coverage account, log in through GitHub and click "Add Repository." Codecov provides a token for uploading reports which is unnecessary for Travis CI. Go to account settings on GitHub, install CodeCov, and under "Configure," add the new repo to "Repository Access" and hit save. Then [configure the .travis.yml file](https://docs.codecov.io/docs)(below), [add a .codecov.yml file](https://docs.codecov.io/v4.3.6/docs/codecov-yaml)(below), and the code coverage account page will be working.

### Add .codecov.yml file
The [.codecov.yml](https://docs.codecov.io/v4.3.6/docs/coverage-configuration) file controls the settings for CodeCov.
A .codecov.yml file can look like this:

      coverage:
        precision: 1
        round: down
        range: "70...100"
      
Range specifies the code coverage percentage range corresponding to color. The low number, in this case 70, means that code coverage under or equal to 70% would show a red background. 100% would be green, and there would be a range of colors inbetween. Depending on the project, different ranges of code can be expected, so 100% does not necessarily have to be the top number if it is unattainable. 

Round specifies how the code percentage should be rounded (up, down, or nearest).

Precision specifies how many decimal points CodeCov will round the percent to. `precision: 1` means that the percentage would be rounded to the tenth, `precision: 2` would round to the hundredth, and so on.

These three settings are the minimum configuration, but there are more [optional settings](https://docs.codecov.io/v4.3.6/docs/codecov-yaml) that you can add to specify the configuration of CodeCov for your project.

### How to Display Code Coverage Badge on Github
Under settings on the Code Coverage website, click on "Badge." Copy the markdown version and paste it in the README file on GitHub. 
