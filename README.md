# Learning Git

Alright guys. So this is a tutoral to learn git. While I have learnt it many times, I want to this time also document it properly, so that I don't end up forgetting it once more.

Now I don't use it often so it's OK if I do forget some of the syntax. But the idea here is to have a *self created* ready reference of the things I should know and why I should do it. Because the thing with git is that while learning the basic commands is easy, I end up forgetting the underlying philosphy or reason to do something. That then just doesn't work with me.

Alright, enough said. Let's get started.

OK so the way I plan to learn it is to use **Git Immersion** site. And probably this document itself can be used to just learn the basics of the git syncing.

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

```git
mkdir learngit
cd learngit
touch learngit.md hello.py
```

## Create Repository

- To create a repository out of a directory run the following command:

```git
git init
```

## Add a file to repository

- This basically adds the file to the 'staging area' and starts 'monitoring it' but doesn't commit it. This allows to commit separate files at different times.

We can also use `git add .`  to add all the files in current directory to the staging area. But we should ensure to run `git status` first to ensure nothing gets unintentionally.

```git
git add hello.py
```

## Check Repository status

```git
git status
```

## Commiting the Changes

- We commit the changes using commit command . We should provide -m flag to provide meaningful message along with commit. If -m flag is forgotten then it will automatically open an editor for you to add the commit message.

```git
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

## Checkout the older version

- We can go back to any older version (snapshot) of a file by using `checkout` command. Here the `<hash>` is determined by the git log command (first 7 characters of hash are enough to uniquely determine.)

```git
git checkout <hash>
cat hello.py
```

- To go back to the original version use below command. `master` is the default branch, but we can create multiple branches as required. By typing the name of the branch during checkout, we go to the latest versin of that branch.

```git
git checkout master
cat hello.py
```

## Tagging the versions

- We can call the current version of the file as v1.

```git
git tag v1
```

- To tag the version immediately prior to current one, we need to first checkout the version 1 step prior to v1 by writing v1~1

```git
git checkout v1~1
cat hello.py  # Verify that we are at the correct version
git tag v1-beta
```

- Now we can go back and forth between the two tagged versions easily.

```git
git checkout v1
git checkout v1-beta
```

- To view the currently available tags.

```git
git tag
```

- Tags are also viewable in the logs.

```git
git hist master --all
```

## Undo Changes

- To undo changes *before staging* the changes, we can simply checkout the last correct version (usually master).

```git
git checkout master
```

- To undo the changes *after staging* we can use the `reset` command. The `reset` command resets the staging area to be whatever is in HEAD . This clears the staging area of the change we just staged. But we should still do `git checkout master` to get the last commited version and remove the unwanted changes from the file.

```git
git add hello.py # Add modified file in staging area
git status

git reset HEAD hello.py # To remove file from staging area

git checkout master   # To actually revert the changes back to last version.
```

- To undo the changes *after commit* we should ideally create a new commit to revert the unwated changes. Instead of HEAD (which we revert the most recent version) we can use the hash of any commit to revert to that particular version.

```git
git add hello.py # Add modified file in staging area
git commit -m "Unwated changes commited"

git revert HEAD # This will open the editor to ask for revert comments.

git log   # We can now see both the wanted commited as well as the revert commit.
```

- We can also undo the changes *after commit* `permanently` by using the `reset` command. What this will do is that it will remove (hide) the incorrect commit from the log history. `--hard` parameter ensures that the working directory also gets updated with the new branch head.

```git
git tag oops   # Tag the current commit as oops
git hist

git reset --hard v1 # v1 is the tag of the version which we want to commit but we can also provide the hash value instead.

git hist  # Now the unwanted commits will not be visible.
git hist --all ## This parameter will show the unwanted commits also.

git tag -d oops  # This removes an existing tag.
git hist --all   # Now the unwated commits are not seen anymore because of removed tag
```

However, care should be taken to use the reset command on remote repositories as it can cause confusion with the other users. Using reset we can modify the branch pointer to point to anywhere in the commit tree.

## Amending Commits

- To commit an additional change with the latest commit we can do the following.

```git
git add hello.py    # Partial change
git commit -m "Add an author name"


git add hello.py    # After making addtional change here.
git commit --amend -m "Add author name and email"

git hist  # Review the changes
```

## Moving Files

- Two ways to move the files and inform git about it.

  - Directly move the file through git. Advantage is that git immediately knows about the move and keeps the changes in the staging area.

  ```git
  mk dir lib
  git mv hello.py lib
  git status
  ```

  - Fir move the file through OS commands and later inform git about it.

  ```git
  mk dir lib
  mv hello.py lib
  git add lib/hello.py
  git rm hello.py  # Removes file from staging area, and from working directory if available there.
  git status
  ```

## Git Directory structure

```git
ls .git  # Shows the structure of folder where all git "stuff" is stored.
ls .git/objects # Shows a bunch of directories with names correspnonding to first 2 letters of hash of the objects stored in git
ls .git/objetcs/<dir> # Shows the file which contain the objects stored in git. These files are encoded but are used to identify the object version.

cat .git/config # This is a project specific config file and will override .gitconfig in $HOME

ls .git/refs   # Shows the way to reference the commits
ls .git/refs/heads  # Shows the ways to reference branches
ls .git/refs/tags # Shows all the tags created for commits
cat .git/refs/tags/v1   # Tag v1 simply points to the hash code of corresponding commit.

cat .git/HEAD   # HEAD file contains the reference to the current branch. Usually points to master by default.

```

## Working wiht Git Objects

- We can make use of the `git type` and `git dump` aliases we defined earlier to traverse and see the content of the files and structure inside git object.

```git
git cat-file -t <hash>    # Shows the type of the commit object
git cat-file -p <hash>    # Shows the corresponding dump of the commit object

git cat-file -p <treehash>    # We can use the hash of the tree to further traverse inside (folder/ git structure)

git cat-file -p <blobhash>  # Blobhash will probably show the content of the corresponding file

```

## Creating a Branch

- A Branch can be defined to isolate any changes from the master (branch).

```git
git branch <branchname>
git checkout <branchname>

git checkout -b <branchname> # Short cut for above 2 commands

```

## Switch Between Branches

- To Simply switch between branches we can use 'git checkout` :

```git
git checkout master
```

## To see diversion between branches

- Use `git log` along with `--graph` parameter.  All use `--all` flag to ensure all branches are shown in the log.

## Merging the Branches

- If master branch got changed while working on another branch (say 'greet') then we can make use of `git merge` to get the master changes into the greet branch. But this cause ugle graphs in the log. So probably  `git rebase` is better to use.

```git
git checkout greet
git merge master

git hist --all
```

## Rebasing the Branches

- To rebase the branch :

```git
git checkout greet  # greet is another branch name
git rebase master

git hist
```

- The advantage of rebase is that greet branch history gets 'overwritten' by master so that it becomes part of greet branchh. this leaves the chian of commits linear and much easier to read.

## Merge Vs Rebase

- We shouldn't use rebase :
  - If the branch is public and shared with others. Rewriting publicly shared branches tend to screw work of other members of the team.
  - When the exact history of the commit branch is important to know.

- So usually we should use Rebase for short-lived, local branches and merge for branches in the public repository.

## Fast Forward Merge

- If the head of one branch is already a direct ancestor of another, the merge simply moves the head in the commit tree. This is called fast forwarding and there is no chance of conflict here.

```git
git checkout master
git merge greet # To merge greet branch changes into master

git hist
```

## Cloning Repositories

### Local Cloning

- To do a local clone we use below command. This creates an exact copy of the original repo with all the files and folder. The commit history is also copied as is, only difference being the name of the repo will be different in the cloned repo.

```git
git clone hello cloned_hello  # Here hello is existing repo
```

### Remote Cloning

- A repostiory on github exists is called a `remote` respository. We may create a local copy of the remote repository on our local computer by first determining the full url of the repository on github. This can be found by using the 'clone' button on the repository page. Then we use the following command:

```git
git clone https://github.com/codeRSH/learngit.git  # Full URL of your remote repo here.
```

## Remote Repository

- To check the name of the remote repository use below :

```git
git remote
```

- Conventionally a Remote repository (Primary Central repo) is called as 'origin'. To get more information about origin:

```git
git remote show origin
```

## Check list of branches

- Git has all the commits from the original repository, but branches in remore repository are not treated as local branches. To see the list of branches

```git
git branch    # To see list of local branches
git branch -a # To see list of all branches (local + remote )
```

## Fetch Changes from Remote Repo

- `git fetch` command will fetch new commits from the remote repository, but it will not merge these commits in the local branches. Once data is fetched we can then amnually merge the changes using `git merge`.

```git
git fetch

git merge origin/master
```

- We can combine the effect of `git fetch` and `git merge origin/master` using simply `git pull`

```git
git pull
```

## Tracking Branch

- We can add a local branch to track a remote branch (other than remote/master).

```git
git branch --track greet origin/greet
git brach -a
git hist # greet local branch will now show along with origin/greet
```

## Bare Repositories

- Bare Repositories are without working directory. They are usually used for sharing.
- Conventionally repositories ending in '.git' are bare repositories. Essentially it is nothing but the .git directory of a non-bare repo.

```git
cd ..
git clone --bare learngit learngit.git
ls learngit.git
```

## Adding a Remote Repository

- We can add the bare repository as a remote to any existing repository.

```git
cd learngit
git remote add shared ../learngit.git
```

## Pushing change to remote repository

- To push the change to remote repository we use:

```git
git push shared master
# Here shared is the remote repo name and master is the branch of local repo receiving the changes.
```

- Usually we have to provide both the remote repo and remote branch name while pushing the changes.

## Pull changes from Remote Repository

```git
git remote add shared ../learngit.git # Add learngit repository as a remote repo.
git branch --track shared master
git pull shared master
```

## Advanced Topics

- Some of the topics to research by own:
  - Reverting Committed Changes
  - Cross OS Line Endings
  - Remote Servers
  - Protocols
  - SSH Setup
  - Remote Branch Management
  - Finding Buggy Commits (git bisect)
  - Workflows
  - Non-command line tools (gitx, gitk, magit)
  - Working with GitHub

