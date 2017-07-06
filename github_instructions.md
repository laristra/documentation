## GitHub
[GitHub](github.com/) is a collection of millions of repositories that offers services to facilitate collaboration on and development of a project. GitHub offers version control, which records who made each change to a repository and when. GitHub is the largest host of source code in the world. README files provide a description of a project.

### Create Github Account
Go to [the Github website](github.com/join) and enter a username, email address, and password. 

### Create SSH Key on GitHub -- Only once per account
SSH provides a secure channel in an unsecure network using encryption. On GitHub, the user creates a pair of public and private keys which allows remote access to a repo using the command line on Terminal. To [create a SSH key for GitHub](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/), go to Terminal. Type `ssh-keygen -t rsa -b 4096 -C "<your_email_for_GitHub>"`. Press enter to save. Do not enter a passphrase. Then, go to GitHub settings. "SSH and GPG keys" is listed under "Personal settings" on the left side of the screen. Click "New SSH key" in the upper right corner. Copy the public version of the SSH key into the window on GitHub. [More instructions](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)

### Make a Github Pages Site and Clone it to your Computer
 * [Github pages site](https://pages.github.com/)
 * Create a new repository and name it `<username>.github.io`, substituting your username
 * Open Terminal and type in the location of where you want to file your repository
   * For example, to put the repository in Desktop, type `cd Desktop` on the command line
 * Type in `git clone` and then the SSH version of the link, which can be found under "clone or download" on the repository page
 * Type `cd <username>.github.io` which will navigate into the local GitHub repository on the computer
 * Type:  
`git add --all`  
`git commit -m "initial commit"`  
`git push -u origin master`  
 Adding updated files from a computer to GitHub requires these three steps every time.

### Clone a GitHub Repo to your computer
[Github's instructions](https://help.github.com/articles/cloning-a-repository/) are fairly straightforward -- first click "Clone or Download" on the main page of a repo and copy the SSH key version. Then go to Terminal and type `git clone <SSH_key>`, which will clone the contents of the git repo to your computer. To navigate into this repo, type `cd <repo_name>`.

### Create a new file on your repo
Create a program in a new file. Travis, CodeCov, SonarQube, etc. will all test this file (or files). To pull the file from GitHub to your local computer, type `cd <repo_name>`, which will navigate into the repo and `git pull` which will pull the updated files on the repo from GitHub to the computer.
