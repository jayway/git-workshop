# Instructions

## Happy flow.
Start by running `gitk origin/base learn-merge`. This will open a view of `gitk` with the branches `origin/base` and `learn-merge`. Looking at the graph we can see that the branches `origin/base` and `learn-merge` diverge after the third commit. Here we can also see that our branch has 3 commits after the divergin point and the `base` branch has 2 new commits.

Now run `git merge origin/base` this pulls in the changes in `origin/base` and merges with our branch. A prompt with a auto-generated commit message, just exit the editor. To visualize what happend run `gitk origin/base learn-merge` again to see how the graph differs. Compared to earlier we see a new commit has merged the two branches together. Comparing this with 
`rebase` it might be a bit harder to understand the graph now. 

Now lets reset the branch to its remote by running `git reset --hard origin/learn-merge`.

## Merge conflict.

To demonstrate a merge conflict we will run the following command `git merge origin/base-conflict`. The only difference between here is that we need to create our own merge commit since we had a conflict. Resolve the merge conflict by removing the line *Oh uh! this will case a conflict!* in `file.txt` and then add the file and do a `git commit`. Looking at `gitk origin/base-conflict learn-merge` and we see that it looks exactly the same as the happy flow case.

Now lets reset the branch to its remote by running `git reset --hard origin/learn-merge`.
## Revert a merge.
Reverting a merge can be done the same way as with reverting a rebase. Example how my reflog looks after running `git merge origin/base`:
```
810ae0e HEAD@{0}: merge origin/base: Merge made by the 'recursive' strategy.
7ad4025 HEAD@{1}: reset: moving to origin/learn-merge
```
What we are looking for is the entry before the merge. In my example it is `HEAD@{1}`. So now just run `git reset --hard HEAD@{1}` and it should be reset to the state before the merge.

# Next lession
Now that we know a little bit more about the difference between merge and rebase let us look a bit closer on rebase. Do this by checking out the `rebase-interactive` branch and looking at the `instructions.md`.

`git checkout rebase-interactive`
