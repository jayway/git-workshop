# Instructions

## Happy flow.
Start by running `gitk origin/base learn-rebase`. This will open a view of `gitk` with the branches `origin/base` and `learn-rebase`. Looking at the graph we can see that the branches `origin/base` and `learn-rebase` diverge after the third commit. Here we can also see that our branch has 3 commits after the divergin point and the `base` branch has 2 new commits.

Now run `git rebase origin/base` this comamand rebases the current branch on `origin/base`. To visualize what happend run `gitk origin/base learn-rebase` again to see how the graph differs. Now everything looks like a neat line and the `origin/base` branch looks like an earlier version of our branch. Remember this as we will do the same operations on the `merge` branch after this. 

Run `git status` and notice how it says *Your branch and 'origin/learn-rebase' have diverged, and have **5** and **3** different commits each, respectively.* It says that our local branch has 5 commits while the remote has 3 commits. Since we did a rebase we rewrote the history of the branch and thus it looks like our local branch has 5 new commits, the 3 commits that was there and the 2 commits from `origin/base`, while the remote has 3 "new" commits. This is the reason for using force push to update the remote. 

Now lets reset the branch to its remote by running `git reset --hard origin/learn-rebase`.

## Merge conflict.

To demonstrate a merge conflict with rebase we should run the following command `git rebase origin/base-conflict`. This will stop us from completing the rebase since we have a merge conflict. The conflict appears in the middle of a rebase so if we run `git status` it will say something like this *You are currently rebasing branch 'learn-rebase' on 'fc59455'.* 

The nice part is that git actually gives us all the commands we can run here to exit this state. 

* `git rebase --abort` will stop the rebase and revert everything to how the branch was.
* `git rebase --skip` will skip trying to apply the current commit.
* `git rebase --continue` will continue the rebase as usual.

To continue we should open the `file.txt` file and resolve our conflict. Essentially remove the line which says *Oh uh! this will case a conflict!*. The `git add file.txt` and finally run `git rebase --continue`. And everything will work out fine.

Now lets reset the branch to its remote by running `git reset --hard origin/learn-rebase`.
## Revert a rebase.
What shall we do if we by accident do a rebase against a branch but we didn't want to do it? To reset we can always revert the branch to its origin by running `git reset --hard origin/learn-rebase`. However this will lose all the commits (we potentially) have not pushed up yet. To demonstrate this, add a line to `file.txt` and do a commit on that. Then run `git rebase origin/base`.

To revert this we can make use of the reflog. Now run `git reflog` this is a history of the operations git has run.  My history looks like this: 
```
5d1dbbf HEAD@{0}: rebase finished: returning to refs/heads/learn-rebase
5d1dbbf HEAD@{1}: rebase: added a line to file.txt
57464a0 HEAD@{2}: rebase: Second commit on rebase for file.txt
5b9674c HEAD@{3}: rebase: first commit on rebase branch
801804a HEAD@{4}: rebase: added instructions for rebase
01a34eb HEAD@{5}: rebase: checkout origin/base
bc8e313 HEAD@{6}: commit: added a line to file.txt
7ad4025 HEAD@{7}: reset: moving to origin/learn-rebase
```
What we are looking for is when the rebase started. In my example it is `HEAD@{6}`. So now just run `git reset --hard HEAD@{6}` and it should be reset to the commit we just made.

# Next lession
Now lets go through merge do this by checking out the learn-merge branch and looking at the `instructions.md`. So lets run:

`git checkout learn-merge`

If you feel like you alreadyd know how merge works and think you can skip that lesson you can go to the next step which is a deeper dive into rebase, with rebase-interactive. Run:

`git checkout rebase-interactive`
