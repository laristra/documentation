## Create a Personal Access Token
A personal access token allows for remote access to a repo. A more secure alternative is a [SSH Deploy Key](https://github.com/laurelmcintyre/documentation/blob/gh-pages/deploy_key.md) because a personal access token, when hacked, can be used to access all of an organization's repos, whereas a deploy key can only access one repo.

Go to GitHub settings, generate a personal access token, and click public_repo. Copy it under Travis environmental variables and call it GH_REPO_TOKEN.
