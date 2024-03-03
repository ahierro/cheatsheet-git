# GIT Cheatsheet

## Initial Configuration

#### Lists all configurations
```bash
git config -l 
```
#### Configure user name
```bash
git config --global user.name "ahierro"
```
#### Configure user email
```bash
git config --global user.email "alejandrohierro123@gmail.com"
```
#### Edit Configs as a file
```bash
git config --global --edit
```
#### Change color current branch on status
```bash
git config color.status.branch magenta
```
#### Change color untracked files on status
```bash
git config color.status.untracked blue
```
#### Autocrlf
- On Windows, you simply pass true to the configuration. For example:
    ```bash
    git config --global core.autocrlf true
    ```
    - Configure Git to ensure line endings in files you checkout are correct for Windows.
    - For compatibility, line endings are converted to Unix style when you commit files.

- On Linux, you simply pass input to the configuration. For example:
    ```bash
    git config --global core.autocrlf input
    ```
    - Configure Git to ensure line endings in files you checkout are correct for Linux

#### Set VS Code as commit editor and diff tool:
```bash
git config --global core.editor 'code --wait --new-window'
```
#### Unset commit editor
```bash
git config --global --unset core.editor
```
#### Set VS Code as difftool
```bash
git config --global diff.tool vscode
```
```bash
git config --global diff.tool 'code --wait --new-window'
```
```bash
git config --global difftool.vscode.cmd 'code --wait --diff $LOCAL $REMOTE'
```
```bash
git config --global difftool.prompt false
```

#### Set VS Code as Merge tool
```bash
git config --global merge.tool vscode
```
```bash
git config --global mergetool.vscode.cmd 'code --wait $MERGED'
```
```bash
git config --global mergetool.prompt false
```
```bash
git config --global mergetool.keepBackup false
```
#### Superlog
```bash
git config --global alias.superlog "log --graph --abbrev-commit --decorate --date=relative --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all"
```
#### Other log
```bash
git config --global alias.lol "log --graph --oneline --decorate"
```
#### Full log
```bash
git config --global alias.flog "log --graph --format=full"
```

### Configure SSH key
#### Generate SSH key
```bash
ssh-keygen -t ed25519 -C "alejandrohierro123@gmail.com"
```
#### Start the SSH agent allowing the SSH client to utilize the agent
```bash
eval "$(ssh-agent -s)"
```

#### Add key to agent
```bash
ssh-add ~/.ssh/id_ed25519
```

#### Copy key

- linux
    ```bash
    cat ~/.ssh/id_ed25519.pub
    ```

- windows
    ```bash
    clip < ~/.ssh/id_ed25519.pub
    ```

- mac
    ```bash
    pbcopy < ~/.ssh/id_ed25519.pub
    ```

#### Copy ssh key into github (settings -> SSH keys -> Authentication keys)

#### Verify ssh config is ok
```bash
ssh -T git@github.com
```

## Initialize Remote Repo
#### create a new repository on the command line
```bash
echo "# cheatsheet-git" >> README.md # Create a file with some content on it
git init # Initializes local repo
git add README.md # Adds a file
git commit -m "first commit" # Commits the file with message 'first commit'
git branch -M main # the -M option stands for "move" and has the effect of forcefully renaming the branch, even if the new branch name already exists. It essentially replaces the existing branch with the new one without requiring a separate deletion step.
git remote add origin git@github.com:ahierro/cheatsheet-git.git # Connects remote repo to local repo
git push -u origin main
```
#### push an existing repository from the command line
```bash
git remote add origin git@github.com:ahierro/cheatsheet-git.git # Connects remote repo to local repo
git branch -M main # the -M option stands for "move" and has the effect of forcefully renaming the branch, even if the new branch name already exists. It essentially replaces the existing branch with the new one without requiring a separate deletion step.
git push -u origin main # Push to remote repo and also sets the remote branch as the upstream for your local branch. This means that, in the future, you can pull from or push to the remote branch without specifying the remote name and branch name.
```

## Basic Flow
#### Show files in staging area / working dir
```bash
git status
git status -s # Shorter version
```
#### Add file to staging area
```bash
git add [file]
git add . # Stage all changes new files, modifications, deletions in the current directory
git add -A # Stage all changes new files, modifications, deletions independently of what the current directory is
git add -u # Adds only modified or deleted files, not new files
```

#### Remove file from staging (undo git add)
- Unstage a file from the staging area. Restore files in the working directory or staging area to a previous state.
```bash
git restore -S [file]
```
- Unstage a file from the staging area. Git reset primarily operates on the commit level, making it broader in scope compared to git restore
```bash
git reset [file]
```
## Stop tracking the file (useful when you have to add it to gitignore).
- Stages the deletion of the file but adds it as untracked files at the same time.
```bash
git rm --cached [file]
```

#### Commits with message
- Opens default editor to add commit message
```bash
git commit 
```
- Commits with message
```bash
git commit -m "message"
```
- Concatenates new changes to previous commit (warning: this rewrites the log, so, like rebase, it shouldn't be used on shared branches if the las commit was pushed)
```bash
git commit --amend
```
## TAGS
- git tag: adds a tag
    - -a (annotation)
    - -m (message)
    - -l (lists)
    - -f (rename)
    - -d (delete)
```bash
git tag -a 0.5.1 -m 'stable version'
```
- Push a single tag
```bash
git push origin [tag_name]
```
- Delete remote tag
```bash
git push --delete origin [tagname]
```

## REBASE (Don't use it on shared branches)
- It takes the commits from a given branch and replays them on top of the HEAD of another branch. If you are working on a featureBranch and you want to put all your commits from featureBranch on top of the latest commit of main branch then switch to main and do a 'git pull' to update it with latests changes and then switch back to featureBranch and do:

```bash
git rebase [main]
```

#### Abort current rebase process discarding changes
```bash
git rebase --abort
```
#### Proceed with the rebase process.
```bash
git rebase --continue
```
## SQUASH COMMITS
- It shows you the list of commits sorted from the oldest at the top to the newest at the bottom. If you put squash or fixup over a commit, it merges with the commit above. To edit in vim, press Esc, and to save press Esc followed by :wq, to exit without saving type :qa!, to cancel type git rebase --abort.
#### Squash last 3 commits
- Leave the first commit on the list as 'pick' and the rest put 'squash'
```bash
git rebase -i HEAD~3
```

## LOGS
 - Git log shows the current HEAD and its ancestry. That is, it prints the commit HEAD points to, then its parent, its parent, and so on. It traverses back through the repo's ancestry, by recursively looking up each commit's parent.
 ```bash
git log # Show the commit history for the currently active branch
git log branchB..branchA # Show the commits on branchA that are not on branchB
git log --oneline # Git formats the commit history to show each commit on a single line
git log --graph # Git formats the commit history in a graphical representation alongside the traditional textual commit information
git log -n [number] # Show latests n commit
git log --stat -M # Show all commit logs with indication of any paths that moved
git reflog # Provides a record of the updates to the tips of branches and other references within your local repository. Unlike the commit history, which tracks the changes made to the files in your repository, the reflog tracks the movements of the HEAD and branch tips, offering a chronological list of Git operations that updated these references. This includes actions such as commits, merges, rebases, and even when you switch branches.
```

## BLAME
- Trace the changes in a file back to the individual commits and authors that made those changes
```bash
git blame [file]
```
- From line 1 to 2
```bash
git blame [file] -L 1,2
```
## HISTORY
 ```bash
gitk --follow --all -p [file]
```
- gitk is a graphical user interface for Git that provides a visual representation of your repository's history. It can display the commit graph, show information about commits, and present the changes made over time. The command gitk --follow --all -p [file] combines several options to tailor the output specifically for tracking the history of a single file across different branches and through renames.

    --follow: This option tells gitk to track the history of a file beyond renames. Normally, Git's history tracking stops at the point where a file was renamed; --follow allows you to see the history of the file before it was renamed, effectively following the file's entire history across its different names.

    --all: By default, gitk shows the commit history of the current branch. The --all option extends this to include all branches and tags in the repository, providing a comprehensive view of the file's history across the entire project.

    -p: This stands for "patch" and instructs gitk to include the diff of the changes made in each commit. This allows you to see not just when and by whom the file was modified, but also what the specific changes were.

## DIFFS 
#### Compare the content of two branches
 ```bash
git diff # Show difference between working area and repository
git diff --staged # diff of what is staged but not yet commited
git diff [main]...[featureBranch] # Show changes introduced in featureBranch from where it started
git diff [sha] # Show changes in a commit
git diff [main] [featureBranch] # Direct Comparison: shows all the changes that are in featureBranch that are not in main.
git diff [main]..[featureBranch] # Commit Range Comparison: shows differences introduced in featureBranch compared to main 
git diff [main]...[featureBranch] # Commit Range Comparison with Merge Base (Three Dots): It gives you a sense of what is new in featureBranch relative to a common starting point shared with main.
git diff [branch_name1] [branch_name2] [file_name] # Show difference on specific file
git diff [commit_hash] [commit_hash] [file_name] # Commit hash can be used instead ob branch_name
git difftool [main] [featureBranch] # Direct Comparison: Like diff be with default tool instead of text based comparison
git difftool [main]..[featureBranch] # Commit Range Comparison
git difftool [main]...[featureBranch] # Commit Range Comparison with Merge Base
git difftool featureBranch $(git merge-base main featureBranch) # Show changes introduced in featureBranch from where it started like three dots syntax
git difftool -t kompare [branch1]..[branch2] # -t to use another tool for example kompare
```

## CHERRY PICK
- Adds a commit from a different branch
 ```bash
git cherry-pick [sha]
```
- Apply changes in a commit to working area
 ```bash
git cherry-pick [sha] --no-commit
```
## BRANCHES
 ```bash
git branch [name-branch] # Creates a new branch
git branch -l # Lists local branches
git branch -a # Lists local and remote branches
git branch -d [name-branch] # Delete branch
git branch -m [old-name-branch] [new-name-branch] # Rename branch
git checkout -b [name-branch]: # Creates a new branch and position in that branch
git switch -c [name-branch]: # Creates a new branch and position in that branch
git checkout [name-branch] # Position in that branch
cat .git/HEAD # Find out HEAD sha
cat .git/refs/heads/main # Find out main sha
```

## DISCARD CHANGES

#### Discard unstaged changes of file
```bash
git restore [file]
```

#### Discard (Discard uncommited changes)
```bash
git checkout . # Leaves untracked files and staged changes and discards updates and deletions
git restore . # Leaves untracked files and staged changes and discards updates and deletions
git checkout [file] # Discard unstaged changes in file
git checkout -p # Discard unstaged hunks
```
#### Discard commited changes With revert
 ```bash
git revert [sha] # Adds a commit with the opposite changes to commit sha
git revert [sha] --no-commit # Add the opposite changes to commit sha to working area
```
#### Reset
 ```bash
git reset . # Unstage all files that are on stage
git reset --soft [sha] # Moves the HEAD (the pointer to the current branch) to the specified commit, but it does not alter the working directory or the staging area (index). This means your working directory remains unchanged, and all the differences between the commit you reset to and the previous HEAD are staged for commit.
git reset --mixed [sha] # Moves the HEAD to the specified commit, similar to --soft. However, it goes a step further by resetting the staging area to match the specified commit. Changes that were staged for commit are demoted to the working directory. Your files are not reverted to their state at the specified commit; rather, the changes are kept in your working directory but are no longer staged.
git reset --hard [sha] # Resets HEAD to commit sha and discard all changes

```
#### Discard everything 
 ```bash
git reset . # Unstage all files that are on stage
git restore . # Leaves untracked files and staged changes and discards updates and deletions
git clean -f # Delete not ignored untracked files
git clean -f -X # Delete untracked files even the ones ignored
 ```
#### STASH (Discard uncommited changes)
```bash
git stash # Saves changes in working area and stage area
git stash --include-untracked # Saves changes in working area and stage area including untracked files
git stash list # Display a list of all your stashed changes.
git stash save "Stash Name" # Saves the stash with a name
git stash drop stash@{number} # Git removes the stash entry at the position specified by {number} from the stash list. This operation permanently deletes the stashed changes associated with that entry, freeing up space and tidying up your stash list.
git stash apply # Applies last stash without deleting it
git stash pop # Applies latest stash and deleting it
git stash pop stash@{number} # Pops specific stash
git stash apply stash@{number} # Apply specific stash
git stash branch testchanges # In case of conflict this creates a branch from the stash and applies the stash to that branch
```
## Git Show
- Gives information about latest commit
```bash
git show 
```
- Gives information about specific commit
```bash
git show [sha]
```
## CLONE
- Download the source code from a remote repository to your local machine, allowing you to work on the project locally. 
```bash
git clone [http_url] # With user and pass
git clone [ssh_url] # Requires ssh key configured
git clone -b [branch-name] --single-branch [repository-url] # This command clones only the history of the specified branch, reducing the amount of data transferred and storage space used on your local machine.
```
## REMOTES
```bash
git remote add [origin] [SSH/HTTPS] # Connects remote repo to local repo
git remote -v # List existing connections
git remote remove [origin] # Deletes connection to remote repo
```

## Bring remote changes
```bash
git fetch origin # Download objects and refs from a remote repository into your local repository with no impact on Working Directory
git merge origin/main # Merge the changes from the main branch of the remote repository (typically referred to as origin) into your current local branch. 
git pull origin main # Combines two actions: fetching updates from a remote repository and merging those updates into your current local branch
git pull --rebase # Combines two actions: fetching updates from a remote repository and rebase 
git pull --rebase --autostash # Before starting rebase, stash local modifications away (see git-stash[1]) if needed, and apply the stash entry when done. --no-autostash is useful to override the rebase.autoStash configuration variable (see git-config[1]).
```
## Upload changes to repository
- Upload the content of your local main branch to the remote repository named origin
```bash
git push origin main
```
- --force overrides remote repo (should not be used if branch is shared with someone else)
```bash
git push origin main --force
```
- -u option also sets the remote branch as the upstream for your local branch. This means that, in the future, you can pull from or push to the remote branch without specifying the remote name and branch name.
```bash
git push origin main -u
```
## MERGE changes from featureBranch into main
#### Go to main branch
- Switch your working directory to the main branch
```bash
git switch [main]
```
#### Merge feature brach changes into main
- Integrate the changes from one branch (in this case, featureBranch) into another branch (typically the current branch you're on, in this case main branch)
```bash
git merge [featureBranch]
```
#### Deal with conflicts
- Keep our changes
```bash
git checkout --ours [file]
```
- Keep remote changes
```bash
git checkout --theirs [file]
```
#### Abort Merge
- Reverts all changes made to the working directory and the index (staging area) to the state they were in before the merge command was run. This includes discarding any conflict resolutions or file modifications made during the merge process.
```bash
git merge --abort
```
#### Continue Merge
- This command essentially tells Git, "I have resolved all conflicts, and now I want to complete the merge operation that was previously paused." If there are no conflicts remaining or if there are unresolved conflicts, Git will not allow the merge to continue and will output an appropriate message guiding you on what needs to be done next.
```bash
git merge --continue
```
#### Unrelated Histories
- By default, git merge command refuses to merge histories that do not share a common ancestor. This option can be used to override this safety when merging histories of two projects that started their lives independently. As that is a very rare occasion, no configuration variable to enable this by default exists and will not be added.
```bash
git merge --allow-unrelated-histories 
```
#### Create a merge commit even when the merge resolves as a fast-forward. This is the default behaviour when merging an annotated (and possibly signed) tag.
```bash
git merge --no-ff [featureBranch]
```
#### Instead of preserving the entire history of commits from the feature branch, you want to combine all those changes into a single commit. The command does not commit the changes by itself. After running git merge --squash, you need to manually commit the changes.
```bash
git merge --squash [featureBranch]
```
#### In case of conflicts
```bash
git mergetool
```
#### After conflict resolution if you don't have this 'git config --global mergetool.keepBackup false' then you should remove .orig files
```bash
git clean -i
```
#### As a final step of the merge commit changes
```bash
git commit
```

## PATCHES
#### Generate a patch
```bash
git format-patch -1 [sha]
```
#### Apply patch
```bash
git am [patchfile]
```

## Refs
#### Special refs
- HEAD – The currently checked-out commit/branch.
- FETCH_HEAD – The most recently fetched branch from a remote repo.
- ORIG_HEAD – A backup reference to HEAD before drastic changes to it.
- MERGE_HEAD – The commit(s) that you’re merging into the current branch with git merge.
- CHERRY_PICK_HEAD – The commit that you’re cherry-picking.

#### Relative refs
- HEAD\~n (The ~ character lets you reach parent commits. For example, HEAD\~2 is the grandparent of HEAD, HEAD~ is the parent)
- HEAD^n (The ~ character will always follow the first parent of a merge commit. If you want to follow a different parent, you need to specify which one with the ^ character. For example, if HEAD is a merge commit, the following returns the second parent of HEAD. HEAD^ is the parent commit, HEAD^2 is the grandparent commit)
- HEAD@{n} (The HEAD{} syntax lets you reference commits stored in the reflog. It works a lot like the HEAD~ references from the previous section, but the  refers to an entry in the reflog instead of the commit history.)
git reset --hard HEAD@{n} # Moves HEAD pointer to n commits before.
