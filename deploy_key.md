## Create a Deploy Key for the Repo
In place of a personal access token which can be used to access all of an organization's repos, a deploy key (SSH key) is specific to one repo and therefore is safer. To create a deploy key, go to Terminal. 
* `git pull` makes sure the repo is up to date on the local machine  
* `ssh-keygen -f deploy` to generate a deploy key  
* Do not enter a passphrase, hit enter  
* `cat deploy.pub` .pub indicates the public key and cat displays the content of the public deploy key in terminal  
* Go to GitHub settings and click "Add Deploy Key"
* Copy the content from `cat deploy.pub` into the window on GitHub that "Add Deploy Key" opens and click "Allow write access"  
* `travis encrypt-file ./deploy` tells travis to encrypt the private deploy key  
* type yes when prompted for detected repository name  
* `travis encrypt-file --add after success ./deploy` tells to add in the after_success portion of the .travis.yml file  
* type yes when prompted to override deploy.enc  
* `git add deploy.enc` .enc implies the encrypted version of the deploy key, add this to github  
* `git add .travis.yml` the .travis.yml file now has the encrypted key on it, so add this to github as well  
* `git commit -m "encrypt deploy key"`  
* Edit the .travis.yml file (`nano .travis.yml`) in terminal and add the following in after_success:  
    
      mv deploy ~/.ssh/id-rsa

      chmod 600 ~/.ssh/id-rsa

The first command means the deploy key will be in the ssh folder and the second chmod 600 command means that only you have read/write access and no one else. The chmod command is in the XYZ format, with X pertaining to your access rights, Y meaning your group, and Z meaning everyone else. The numbers range from 0 - 7 and have different meanings. 2 means write and 4 means read, so 6 = 2 + 4 and has both these permissions.

* Save the changes with Ctrl o, hit enter, and then hit Ctrl x to exit nano.  
* `git add .travis.yml` add the travis file again  
* `git commit -m "add .travis.yml`  
* `git push` push changes to github  
