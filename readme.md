# Introduction

This is a small git tutorial repository for learning `git-rebase` and what it can do. This tutorial will be very command line heavy so I recommend to set ones git editor to something that you are used to.

This repository has read only capacity to not accidentily destroy the tutorial if someone tries to push to it. If you want to test out how to do this on the remote i suggest you fork this repository and work from there.

# Requirements

Some sort of git command line tool. I would also recommend `gitk` which can view a nicer graph of the branches to make it easier to understand what is going on.

## Ubuntu

Run: 
```
sudo apt install git
sudo apt install gitk
```

## Mac

git can be installed either through `brew` which is what i would recommend. To get `gitk` one must update the version of `git` installed on the machine by running:

```
brew update 
brew install git
```

one can also update git through a .pkg file, although i will leave that to the person who wants to explore that path.

## Windows

For Windows you can download and install [git-bash](https://git-scm.com/downloads) which will give you all you need. 


# Lets get started!


Start with cloning down the repository and set the default editor to something more comfortable.
By default (I think)  it is set to be `vim`.  To change the editor just run `git config core.editor {editor}` where editor is whatever you want it to be, if you are not used to `vim` I think `nano` is a pretty good alternative. For Windows users you might need to do a bit more work hopefully running `git config core.editor notepad` will suffice. 

To start this workshop simply checkout the branch `learn-rebase` and look at the `instructions.md` file for what to do. Let run: 

`git checkout learn-rebase`
