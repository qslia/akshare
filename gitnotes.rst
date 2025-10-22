git remote add upstream https://github.com/akfamily/akshare.git
git switch main
git fetch upstream
git rebase upstream/main
git push upstream main
git branch --set-upstream-to=upstream/main