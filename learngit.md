# 23/04/2020 - Learning Git

Alright guys. So today I am going to learn git. While I have learnt it many times, I want to this time also document it properly, so that I don't end up forgetting it once more.

Now I don't use it often so it's OK if I do forget some of the syntax. But the idea here is that, to have a *self created* ready reference of the things I should know and why I should do it. Because the thing with git is that while learning the basic commands of git is easy, I end up forgetting the underlying philosphy or reason to do something. That then just doesn't work with me.

Alright, enough said. Let's get started.

OK so the way I plan to learn it is to use **gitimmersion** site. And probably this document itself can be used to just learn the basics of the git syncing.

## Setting up Name and Email and other settings

- We should set up the global name and email the first time we start git. This enables to use this setting throughout the git usage.

``` git
git config --global user.name "Your Name"
git config --global user.email "your_email@whatever.com"
```

- Additionally to take care of the cross-platform development, we use the following :

``` git
git config --global core.autocrlf input  # This makes sure to convert CRLF to LF while commiting but not the other way around.
git config --global core.safecrlf true   # This is used to ensure that the binary files' eol is not touched accidentally.
```

## Create Hello World Program

- Create a folder learngit and add this file `learngit.md` in it. Additionally create a new `hello.py` program.

``` git
mkdir learngit
cd learngit
touch learngit.md hello.py
```

## Create Repository

- To create a repository out of a directory run the following command:

``` git
git init
```

## Add a file to repository

- This basically adds the file to the 'staging area' and starts 'monitoring it' but doesn't commit it. This allows to commit separate files at different times.

We can also use `git add .`  to add all the files in current directory to the staging area. But we should ensure to run `git status` first to ensure nothing gets unintentionally.

``` git
git add hello.py
```

## Check Repository status

``` git
git status
```

## Commiting the Changes

- We commit the changes using commit command . We should provide -m flag to provide meaningful message along with commit. If -m flag is forgotten then it will automatically open an editor for you to add the commit message.

``` git
git commit -m "my meaninful commit message"
```

## Git Tracks the changes inside the file (rather then files themselves)

- When `git add filename` is executed it basically tell git to note the current state of the file to be commited later. Any more changes done in the `git add` will not be captured in the current commit.

## Checking the History of the Project

- We can see what all changes have been made in the repository using:

```git
git log
```

- One line display of changes made:

```git
git log --pretty=oneline
```

- Various options:

```git
git log --pretty=oneline --max-count=2
git log --pretty=oneline --since='5 minutes ago'
git log --pretty=oneline --until='5 minutes ago'
git log --pretty=oneline --author=<my name>
git log --pretty=oneline --all
git log --pretty=format:'%h %cd %s (%an)' --since='7 days ago'
git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short

```

Talking about last option which can be preferred way:
--pretty="..." defines the format of the output.
%h is the abbreviated hash of the commit
%d are any decorations on that commit (e.g. branch heads or tags)
%ad is the author date
%s is the comment
%an is the author name
--graph informs git to display the commit tree in an ASCII graph layout
--date=short keeps the date format nice and short

## Git Aliases

We can set up aliases for some commonly used and long git commands in `.gitconfig` file in $HOME directory. Add the following in there:

```git
[alias]
  co = checkout
  ci = commit
  st = status
  br = branch
  hist = log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short
  type = cat-file -t
  dump = cat-file -p
```

Now we can simply write `git ci` for commit and `git st` to check status.

