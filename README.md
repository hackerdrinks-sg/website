## Local Development

[Install hugo](https://gohugo.io/getting-started/installing/)

Start hugo server

    hugo server -F

If you are only modifying the venue and date, you just need to update `hackerdrinks/content/events`

## Deployment

Check in and push source branch to origin

Jenkins will build and merge to master

Github pages will serve the static file

## Theming

This site relies on a third-party repo for its theme

https://github.com/luizdepra/hugo-coder.git


Following [this guide](https://www.atlassian.com/blog/git/alternatives-to-git-submodule-git-subtree), we add the them repo as a remote, then merged its changes into our tree using `git subtree` (as an alternative to `git submodule`)


Following the article, we first add the repo as a remote

    git remote add -f theme_coder https://github.com/luizdepra/hugo-coder.git

Then we add the remote as a subtree under `hackerdrinks/themes/coder` by running

    git subtree add --prefix hackerdrinks/themes/coder theme_coder master --squash

at the project root.

_Before this though, we need to sure that the path `hackerdrinks/themes/coder` does not actually exist._

After this, the subtree should be integrated into our project repo.

_If you've pulled after we initially created the subtree, then all you should really have to do is to add the remote._


### Updating

To update the code for the theme (after having setup the remote),

    git fetch theme_coder
    git subtree pull --prefix hackerdrinks/themes/coder theme_coder master --squash

##### Ignore this (github trigger)
