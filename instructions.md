# Instructions

We will learn a bit about interactive rebase, how to use it to edit history of the branch etc.

## Rebase Interactive.
You don't have to rebase against another branch, you can also rebase against this branch itself. This can be done with referencing an earlier commit on the branch by its hash or by specifying `HEAD~2` where the number after the tilde is how many steps backwards you can go. 
We are gonna use the following command `git rebase -i --root` . To break this down:
* `git rebase`: just the rebase command
* `-i` or `--interactive` will open the default editor to choose what we want to do. (will be explained more after this breakdown).
* `--root` we are rebaseing onto the root commit of this branch, this essentially means that we can change the first commit of this branch. This will in turn make it look like all the commits are changed when comparing to the remote. Note: this is not recommened to use when you have a remote since it will change the root commit it will look very weird on a PR or such, however for this workshop this is sufficient. Usually you rebase against another branch or just a couple of commits earlier.

Now run the `git rebase -i --root`, this will open the default editor with the following:
```
pick 01ec0f7 added gitignore
pick 7cc7014 added a line to file.txt
pick 3e271c7 added a divider line to avoid conflicts
pick ef48bb3 added instructions
pick efe9324 tihs is the first commit on rebase-interactive
pick 269c1e9 second commit on file.txt
pick fc7335f Reorder me!
pick 6c55cdf This should come before the reorder commit
pick 94e98e9 A huge commit
pick 9088bfd a smaller commit
pick 7457a67 another huge commit

# Rebase 7457a67 onto 61d6824 (11 command(s))
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```
We see all the commits made on this branch, if we compare with `gitk` the commits start with the first one at the top and the last one on the bottom. Git also provides this handy notation at the bottom of the file with all the commands that you can run during the rebase. Exit this file and then run `git reset --hard origin/rebase-interactive`.

## Editing history

### Fixing an eariler commit
There are a couple of ways of editing history, the easiest it `git commit --amend` this changes the latest commit on your branch, however quite often we want to go further back and fix something, for instance a spelling misstake. For this use case we can use the interactive rebase approach!
Run `git rebase -i --root` if we look at the commits we notice that the 5th commit from the top has a spelling misstake, to fix that change from using `pick` to `edit`. Before closing the editor it should look like this:
```
pick 01ec0f7 added gitignore
pick 7cc7014 added a line to file.txt
pick 3e271c7 added a divider line to avoid conflicts
pick ef48bb3 added instructions
edit efe9324 tihs is the first commit on rebase-interactive
pick 269c1e9 second commit on file.txt
pick fc7335f Reorder me!
pick 6c55cdf This should come before the reorder commit
pick 94e98e9 A huge commit
pick 9088bfd a smaller commit
pick 7457a67 another huge commit
```
This will stop the rebase after the 5th commit has been applied for editing purposes.  Closing the editor and opening `gitk` shows us that we have thus far applied 5 commits. Now we can make use of the `git commit --amend`. To add to the previous commit just run `git add`  on a specific file and then run `git commit --amend`. To fix our spelling misstake open the `file.txt` and change `Tihs` to `This` on the second to last line. Then run `git add file.txt` and finally run `git commit --amend`. When this is done the editor will open with the commit message, change the `tihs` to `this` and then save and close the editor. Finally to continue the rebase simply run `git rebase --continue`.

When opening `gitk` now we can see that the 5th commit is fixed!

Now just reset the branch by running `git reset --hard origin/rebase-interactive`.

There is another way of editing history which I think is preferred since it won't edit the history until you rebase the branch. That is with the `git commit --fixup` 

Instead of rebasing now and changing history we can make use of the fixup commit. To do this we need to find the hash for the commit we want to fix. I think it is easiest to do this by running `git log` and finding the commit message for the commit we want to change, for me the `git log` command gives the following output:
```
commit 7457a67adbd16502d57596cf55719585f249e7c1
Author: Shan Senanayake <shan.senanayake@jayway.com>
Date:   Wed May 30 15:51:50 2018 +0200

    another huge commit

commit 9088bfd8179b72c374bd7a3f56a4ebc009ff140c
Author: Shan Senanayake <shan.senanayake@jayway.com>
Date:   Wed May 30 15:50:55 2018 +0200

    a smaller commit

commit 94e98e9dd6008d07cf0e7661e7f9d311b33c07cd
Author: Shan Senanayake <shan.senanayake@jayway.com>
Date:   Mon May 28 09:10:21 2018 +0200

    A huge commit

commit 6c55cdf2f754379e7fd557c8badc0b7097d0c5d7
Author: Shan Senanayake <shan.senanayake@jayway.com>
Date:   Mon May 28 09:06:28 2018 +0200

    This should come before the reorder commit

commit fc7335f4d2ed3132b341ee55916a5a2241bb2185
Author: Shan Senanayake <shan.senanayake@jayway.com>
Date:   Mon May 28 09:05:29 2018 +0200

    Reorder me!

commit 269c1e947405cdc337e4b9dcd07e6f0a1c6d3b49
Author: Shan Senanayake <shan.senanayake@jayway.com>
Date:   Mon May 28 09:04:31 2018 +0200

    second commit on file.txt

commit efe9324e7c1d3daf26cdc3b704c3df43fce38725
Author: Shan Senanayake <shan.senanayake@jayway.com>
Date:   Mon May 28 09:03:16 2018 +0200

    tihs is the first commit on rebase-interactive

commit ef48bb344f7e286465c3883cbf6f029cadff930e
Author: Shan Senanayake <shan.senanayake@jayway.com>
Date:   Mon May 28 09:02:26 2018 +0200

    added instructions

commit 3e271c781091ce9fc741623440a5a336e6a607b8
Author: Shan Senanayake <shan.senanayake@jayway.com>
Date:   Sat May 26 08:14:17 2018 +0200

    added a divider line to avoid conflicts

commit 7cc701495761a4f01d740ae67684652c470d809c
Author: Shan Senanayake <shan.senanayake@jayway.com>
Date:   Fri May 25 07:35:37 2018 +0200

    added a line to file.txt

commit 01ec0f722a3b58c658040ee08c342c15cb012742
Author: Shan Senanayake <shan.senanayake@jayway.com>
Date:   Wed May 23 15:46:15 2018 +0200

    added gitignore

```
What we want to copy is the SHAID for the commit. In this case it is `efe9324e7c1d3daf26cdc3b704c3df43fce38725`. Then we can do our  fix, that means opening `file.txt` change `Tihs` to `This` in the third line of the file and run `git add file.txt`. Now to do the fixup commit simply run `git commit --fixup efe9324e7c1d3daf26cdc3b704c3df43fce38725`. By opening `gitk` we can now see that a new commit was created with `fixup!` in the commit message. 
To squash the commit simply run `git rebase -i --root --autosquash` this will look like the following: 
```
pick 01ec0f7 added gitignore
pick 7cc7014 added a line to file.txt
pick 3e271c7 added a divider line to avoid conflicts
pick ef48bb3 added instructions
pick efe9324 tihs is the first commit on rebase-interactive
fixup e4fd7eb fixup! tihs is the first commit on rebase-interactive
pick 269c1e9 second commit on file.txt
pick fc7335f Reorder me!
pick 6c55cdf This should come before the reorder commit
pick 94e98e9 A huge commit
pick 9088bfd a smaller commit
pick 7457a67 another huge commit
```
Notice how the commit we just did is moved and has `fixup` instead of `pick` as command. The autosquash command squashes commits with `fixup!` in them. When you exit the editor and open up `gitk` it will now look as if nothing has happend! Ergo the two commits have been squashed. Sadly if we want to change the commit message we still need to use `rebase -i` and change from `pick` to `reword`.

Now lets reset our branch to default with `git reset --hard origin/rebase-interactive`.

## Reordering commits
Sometimes one wants to reorder the commit history, this is fairly easy  with the `rebase -i` command. Let's first check `gitk` to see how our tree looks right now. What we want to achieve is to reorder the commits `This should come before the reorder commit` and the `Reorder me!` commit. To do this run `rebase -i --root` and simply move the line with `Reorder me!` commit below the `This should come before the reorder commit`. When you are done the editor should look like this:
```
pick 01ec0f7 added gitignore
pick 7cc7014 added a line to file.txt
pick 3e271c7 added a divider line to avoid conflicts
pick ef48bb3 added instructions
pick efe9324 tihs is the first commit on rebase-interactive
pick 269c1e9 second commit on file.txt
pick 6c55cdf This should come before the reorder commit
pick fc7335f Reorder me!
pick 94e98e9 A huge commit
pick 9088bfd a smaller commit
pick 7457a67 another huge commit
```

Now simply save and exit the editor and check `gitk` now. We will see that we have sucessfully reordered our commits!

Lets reset the branch with `git reset --hard origin/rebase-interactive`.

## Splitting up a commit to several commits
Lets say you accidentily added a bunch of stuff to a commit and want to revert that however you still want to keep your changes. Well this can be done with a soft reset. If we check `gitk` we see that the latest commit is quite big. Let us fix that by first reverting back one step with `git reset HEAD~1`. `HEAD` is where we currently are standing and `~1` is looking back one commit. Thus we are resetting one commit backwards. If we check our `gitk` now we see that the latest commit is no longer the huge commit. Running `git status` shows us that we have modified changes. Thus we can now commit just like we are used ergo: 
* one commit for file3.txt 
* one commit for file4.txt
* one commit for file.txt

Now lets reset the branch with `git reset --hard origin/rebase-interactive`.

What if we had made a huge commit earlier in the history? How do we fix that? `git rebase -i --root` to the rescue! We simply need to go back to that commit and do the same thing as above. Run `git rebase -i --root` and change the `a huge commit` to edit. When closing the editor it should look like the following: 
```
pick 859cc2e added gitignore
pick 112369c added a line to file.txt
pick 6e41e8b added a divider line to avoid conflicts
pick 1a8100d added instructions
pick 3e42430 tihs is the first commit on rebase-interactive
pick 1b8816c second commit on file.txt
pick 666cbbc Reorder me!
pick b1c2407 This should come before the reorder commit
edit bd08e8f A huge commit
pick 8bba069 a smaller commit
pick 0277a21 another huge commit
```
Exit the editor and then run `git reset HEAD~1`, then do the following commits:
* one commit for file.txt
* one commit for file1.txt
* one commit for file2.txt

When done run `git rebase --continue`. Let's open `gitk` and we can now see that the huge commit is replace by the 3 smaller commits we did when we were rebasing. 

Now lets reset the branch with `git reset --hard origin/rebase-interactive`

# The End
Welp that is all I have to teach about rebase. If you want to build on this or have any questions (or suggestions) do not hesitate to tell me. 

