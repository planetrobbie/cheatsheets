{
    "id": "auto",
    "style": "css/h4dark.css",
    "layout": "default"
}
%%%END

# Git

#### Getting and creating projects

command | description | comments
-- | -- | --
git init | initialize a directory as a Git Repository | there is now a .git subdirectory in your project, this is your Git repository where all the data of your project snapshots are stored
git clone git://github.com/schacon/simplegit.git | clone a git repository | this will copy the entire history of that project so you have it locally, you'll also have a .git subdirectory that will store all data

#### Snapshotting

command | description | comments
-- | -- | --
git add ... | adds file contents to the staging area | you run git add on a file when you want to include whatever changes you've made to it in your next commit snapshot. Gives more granularity then other RCS
git status -s | you run git status to see if anything has been modified and/or staged since your last commit so you can decide if you want to commit a new snapshot and what will be recorded in it. `-s` gives you short output of the status, 1st column for staging area, second for working area
git diff | show diff of unstaged changes | so where git status will show you what files have changed and/or bstaged since your last commit, git diff will show you what those chanactually are, line by line. It's generally a good follow-up command of git status
git diff --cached | show diff of staged changes | this will show you the changes that will currently go into the commit snapshot.
git diff HEAD | show diff of all staged or unstaged changes | this basically means you want to see the difference between your working directory and the last commit, ignoring the staging area with Tag instead, will show what has changed since this tag
git diff --stat | show summary of changes instead of a full diff | if we don't want the full diff output, but we want more than the status output
git diff branchA...branchB | compare two divergent branches |   Git will automatically figure out what the common commit (otherwise known as the "merge base") of the two commit is and do the diff off of that.
git config --global user.name 'Your Name' | configure name | will be recorded with every commit done
git config --global user.email you@somedomain.com | configure email | will be recorded with every commit done
git add hello.rb; git status -s; M hello.rb; git commit -m hola mundo changes' | records a snapshot of the staging area
git commit -am 'new commit' | stage all tracked, modified files before the commit | to avoid to use git add for each modified file
git reset HEAD -- hello.rb | unstage changes that you have staged | this command unstage hello.rb, `--` starts file path argument
git rm file | remove files from the staging area | kicks the file off the stage entirely, so that it's not included in next commit snapshot, thereby effectively deleting it. Add --cached to leave file on disk.

#### Branching and Merging

command | description | comments
-- | -- | --
git branch | list your available branches |
git branch (branchname) | create a new branch | you can think of it like a bookmark for where you currently are. When you start on work it is very useful to always start it in a branch and then merge it in and delete the branch when you're done. It's easy to rollback then.
git checkout (branch) | switch to a new branch context | `-b` to create and immediatly switch to a new branch instead of using `git branch newbranch; git checkout newbranch`
git branch -d (branchname) | delete a branch | Already merged branch can be deleted
git merge (branchname) | merge a branch context into your current one | branchname will be merged in the current context
git log (branchname) | show commit history of a branch | `--oneline` for a compact output <br/>`--graph` for a topology view `^otherbranch` to exclude otherbranch from report `--decorate` to display tags
git tag -a v1.0 | tag a point in history as important | tag last commit (HEAD) as v1.0 by adding a permanent bookmark at a specific commit. commit SHA at the end to tag another commit

#### Sharing and updating project

command | description | comments
-- | -- | --
git remote | list, add and delete remote repository aliases | Git repositories are all equal and you simply synchronize between them. This command help manage alias or nickname for each remote repository URL to avoid using full URL every time `-v` to display full URLs
git remote add [alias] [url] | create a new alias for a remote repository | alias names are arbitrary 
git remote rm [alias] | removing an existing remote alias |
git fetch | download new branches and data from a remote repository | will synchronize you with another repo, pulling down any data that you do not have locally and giving you bookmarks to where each branch on that remote was when you synchronized.
git pull | fetch from a remote repo and try to merge into the current branch `--all` synchronize all of your remotes | run a git fetch immediately followed by a git merge of the branch on that remote that is tracked by whatever branch you are currently in. Prefer running it separatly instead.
git push [alias] [branch] | push your new branches and data to a remote repository | attempt to make your [branch] the new [branch] on the [alias] remote. If your branch is already on the server, it will try to update it, if it is not, Git will add it. If someone else has pushed since you last fetched and merged, the Git server will deny your push until you are up to date.

#### Inspection and comparison

command | description | comments
-- | -- | --
git log --author | look for only commits from a specific author |
git log --since={2010-04-18} --before={3.weeks.ago} | filter commits by date authored | `--until --after` also available
git log --grep | filter commits by commit message | Git will logically OR all `--grep` and `--author` arguments. Use `--all-match` to match all `--format="%h %an %s"` to modify output format
git log -Sstring | filter by introduced diff | tell Git to look through the diff of each commit for a string.
git log -p | show patch introduced at each commit | `git show [SHA]` do the same with a specific commit SHA
git log --stat | show diffstat of changes introduced at each commit | summary of the changes, less verbose then `-p`
    
#### References

- [Everyday GIT With 20 Commands Or So](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/giteveryday.html)
- [Pro Git book](https://git-scm.com/book/en/v2)
- [one page cheatsheet from x-combinator](http://www.xcombinator.com/2010/09/01/git-cheat-sheet-and-class-notes/)
- [GitHub cheatsheet](http://help.github.com/git-cheat-sheets/ )
- [Jan Krueger cheatsheet](http://jan-krueger.net/development/git-cheat-sheet-extended-edition)
- [Collection of Cheatsheets](http://devcheatsheet.com/tag/git/)

--- 

Created by Planetrobbie
source available on [GitHub](https://github.com/planetrobbie/cheatsheets/git.md)
