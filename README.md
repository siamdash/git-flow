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

###Creating a release branch
$ git checkout -b release-1.2 develop
>Switched to a new branch "release-1.2"

$ ./bump-version.sh 1.2
> Files modified successfully, version bumped to 1.2.

$ git commit -a -m "Bumped version number to 1.2"
> [release-1.2 74d9424] Bumped version number to 1.2
1 files changed, 1 insertions(+), 1 deletions(-)

After creating a new branch and switching to it, we bump the version number. Here, bump-version.sh is a fictional shell script that changes some files in the working copy to reflect the new version. (This can of course be a manual change—the point being that some files change.) Then, the bumped version number is committed.

This new branch may exist there for a while, until the release may be rolled out definitely. During that time, bug fixes may be applied in this branch (rather than on the develop branch). Adding large new features here is strictly prohibited. They must be merged into develop, and therefore, wait for the next big release.

###Finishing a release branch
$ git checkout master
> Switched to branch 'master'

$ git merge --no-ff release-1.2
>Merge made by recursive.
(Summary of changes)

$ git tag -a 1.2

To keep the changes made in the release branch, we need to merge those back into develop, though. In Git:

$ git checkout develop
> Switched to branch 'develop'

$ git merge --no-ff release-1.2
>Merge made by recursive.
(Summary of changes)

This step may well lead to a merge conflict (probably even, since we have changed the version number). If so, fix it and commit.

Now we are really done and the release branch may be removed, since we don’t need it anymore:

$ git branch -d release-1.2
> Deleted branch release-1.2 (was ff452fe).
