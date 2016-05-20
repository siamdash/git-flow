# git-flow
A simple repository for exploring workflow.

###Creating a feature branch
When starting work on a new feature, branch off from the develop branch.

$ git checkout -b myfeature develop
> Switched to a new branch "myfeature"

###Incorporating a finished feature on develop
Finished features may be merged into the develop branch to definitely add them to the upcoming release:

$ git checkout develop
> Switched to branch 'develop'

$ git merge --no-ff myfeature
> Updating ea1b82a..05e9557
> (Summary of changes)

 git branch -d myfeature
 > Deleted branch myfeature (was 05e9557).

 $ git push origin develop

 The --no-ff flag causes the merge to always create a new commit object, even if the merge could be performed with a fast-forward. This avoids losing information about the historical existence of a feature branch and groups together all commits that together added the feature.

 In the latter case, it is impossible to see from the Git history which of the commit objects together have implemented a feature—you would have to manually read all the log messages. Reverting a whole feature (i.e. a group of commits), is a true headache in the latter situation, whereas it is easily done if the --no-ff flag was used.

Yes, it will create a few more (empty) commit objects, but the gain is much bigger than the cost.
