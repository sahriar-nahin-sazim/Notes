### Sazim Resources


so if we are aware of the form field names from pdf, this approach becomes really easy and less error-prone. 
the alternative approach is to use an array.

[source](https://youtube.com/playlist?list=PL9lx0DXCC4BNUby5H58y6s2TQVLadV8v7&si=vsdA18smsE9FS9ng) 
- **Git objects** *(Like Data store/Table)*
	- **Blob** - contents of file (files - metadata)
		- identified by sha1 hash
	- **Tree** - directory/folder listing (blob + trees)
	- **commit** - complete snapshot of working tree
		- point to main tree
		- metadata
		- parent commit
	- **index/staging area** 
		- before commit happens
		- either the content in last commit or the prep for new commit

- **branch** - named reference to commit
		 - **Head** pointer points to a branch
- **repository**
		- collection of commits
		- branches
		- head

Syncing Repository is like distributed DB and multi-repo organization is like shrading [🔗](https://youtu.be/YdstUWcg5j4?si=Z3SViQrdAiR2S_tj)





### branch and movement between
`git checkout <id>`(of commit) 
- temporarily move to older version of commit

`git branch`
- to view all branches + one active
 - `<branch_name>` - creates new branch
 - `--merged` - list of merged branch
 - `--no-merged`
`git merge <name>`
- merge to main

`git checkout -b branch_name`
- create and checkout branch
`git checkout -D branch_name`
- Delete branch
`git switch branch_name`
`git switch -C branch_name`

`git cherry-pick commit_id`

##### stash
`git stash`
	`push -m"message"`
	`list` 
	`show <name/sequence>`
	`apply <name>`
	`drop <name>`
	`clear` - remove all
- does not include new untracked ones `-a/--all`
##### merge
- fast forward 
		- take master forward to branch (no additional branch in play, linear path)
	`git merge branch_name` (has to be at master)
- 3 way merge
		- branch diverge from a previous master commit (master gets ahead with branch)
		- common ancestor, top of master and branch
	`git merge --no-ff branch_name`
		- easier to revert
	- **conflict** 
			- current change - at master
			- incoming - from branch
			- both - both branch lines
			- manual 
					- evil commit - adds new thing
	- `--abort` - to resolve conflict later
	- `git reset --hard HEAD~n`
	- `git revert -m 1 HEAD`
	- `--squash branchname` 
		- remove branch and commit on top of head (listed as not merged)
	- `rebase master`
		- given the master branch moved fo rward, we change the ancestor of branch to top of master
			- basically copies the commits from branch and puts them on top of master branch
		- perform from branch
### undo
`git revert <id>` (commit id)
- adds a new commit undoing specified
`git reset --hard <id>` - kill everything after  this
	- soft - only head changes in commit history
	- mixed(default) - change in staging area as well
	- hard - working area as well

 
### remote stuff (github)

```bash
git remote add origin <github_link> # origin is the name
git remote set-url origin <http://gitUserName@github_link> # add username
git push origin main # origin branch
# prompt of password, provide PAT saved by OS
git push --set-upstream origin main # set connection to main branch

git pull # download updated code
```
`git remote -v`

`git pull` 
	`--rebase` - make history linear
`git fetch origin`
`git merge origin/master`

`git branch -vv` - local and commit difference

`git push origin tagname`
	`--delete` - to delete tag

settings
	-> developer settings
	-> personal access token -> generate new token (chose repo)

`git merge from-where`
 get git repo link
- `git remote add origin link` origin = link
- `git push -u` `origin`(where) `master` (branch) - upload
 
`git diff branch_name`

- `git config --global user.name "name"`
- `git config --global user.email "name"`
- `git branch  --set-upstream`(aka u)`-to-origin/master  ` 

`sshkeygen` `-t rsa`(type of encryption)`-b 4096`(strength of encryption)`-C email`
`ls| grep keyname` ->.`pub` result is going to be uploaded 
`cat keyname.pub` -> github settings---> ssh keys

wget --mirror --convert-links --wait=2 url


## configuration
`git config --global user.name "Nahin"`
`git config --global user.email notoldbutbald@gmai.com`

`git config --global core.editor "code --wait"`
`git config --global -e` 
- edit global config in editor

`git config --global core.autocrlf input`
- this removes carriage return
- set `true` for windows (`/r, /n`)


## basics
`git init` 
- make it a git repository (.git folder)


`git commit -am "-m for message"` 
	`git add . `  (-A) - stage files (ready for next commit) 
		`git reset .` to unstage
	`git commit - m "-m for message"` 
	- commit enables us to get back to snapshot of previous version
	- 

### branch and movement between
`git checkout <id>`(of commit) 
- temporarily move to older version of commit

`git branch`
- to view all branches + one active
`git branch <branch_  name>` 
- creates new branch
`git merge <name>`
- merge to main

`git checkout -b branch_name`
- create and checkout branch
`git checkout -D branch_name`
- Delete branch
 
### undo
`git revert <id>` (commit id)
- adds a new commit undoing specified
`git reset --hard <id>` - kill everything after this


### remote stuff (github)

```bash
git remote add origin <github_link> # origin is the name
git remote set-url origin <http://gitUserName@github_link> # add username
git push origin main # origin branch
# prompt of password, provide PAT saved by OS
git push --set-upstream origin main # set connection to main branch

git pull # download updated code
```
 
settings
	-> developer settings
	-> personal access token -> generate new token (chose repo)

`git merge from-where`
 get git repo link
- `git remote add origin link` origin = link
- `git push -u` `origin`(where) `master` (branch) - upload
 
`git diff branch_name`

- `git config --global user.name "name"`
- `git config --global user.email "name"`
- `git branch  --set-upstream`(aka u)`-to-origin/master  ` 

`sshkeygen` `-t rsa`(type of encryption)`-b 4096`(strength of encryption)`-C email`
`ls| grep keyname` ->.`pub` result is going to be uploaded 
`cat keyname.pub` -> github settings---> ssh keys

wget --mirror --convert-links --wait=2 url


### merge conflict



## configuration
`git config --global user.name "Nahin"`
`git config --global user.email notoldbutbald@gmai.com`

`git config --global core.editor "code --wait"`
`git config --global diff.tool code`
`git config --global difftool.code.cmd "code --wait --diff $LOCAL $REMOTE"`
`git config --global merge.tool mold`
`git config --global mergetool.mold.path "link-to-path"`

`git config --global -e` 
- edit global config in editor

`git config --global core.autocrlf input`
- this removes carriage return
- set `true` for windows (`/r, /n`)


## basics
`git init` 
- make it a git repository (.git folder)

`git status` `-s`
- check to be committed - staging area (stage vs already committed)
- untracked (not yet staged) ones 
`git log`
- see all commits (from **head** to older)
	- **head** points commit loaded currently (`git checkout` to move around commits)
	- to get to top of branch,  `git checkout branch_name`
	- `--oneline ` - to view commits in one line
	- `--reverse`  - reverse order
	- `--stat` - changed files update info
	- `--patch` - actual changes in file
	- `-n` - n number of commits
	- `--author="name"` - filter by author
	- `--before/after="YYYY-MM-DD/yesterday/one week ago"`
	- `--grep="keyword"` - filter by keyword (case sensitive) from commit message
	- `-S"Keyword"`
	- `first_commit..end_commit`
	- `filename` `--filename`
	- `--pretty=format:"%an %h %H %cd"`
	- `branch1..branch2`
	- `--graph` - for branching stuff




`git diff`
`git diff --staged`
	`branch_name` - current and specified branch difference
	
`git show commit_id` 
- `HEAD~n` works as well
- add `:filename` to view content of the file
- `--name-only` 
- `--name-status`

`git ls-tree commit_id`
- all the files in a commit
`git show commit_id` - view object

`git ls-files`
	- view the staging area (-> `git add filename`)
	- `git rm filename` - removes file from staging area + working dir
	- `git rm --cached filename` - staging area delete

`git mv oldname newname`

`git commit -am "-m for message"` 
	`git add . `  (-A) - stage files (ready for next commit) 
		`git reset .` to unstage
	`git commit - m "-m for message"` - commit enables us to get back to snapshot of previous version

#### Revert
`git restore --staged filename`
- take last commit and move to staging area
`git restore .`
- staging to working dir
`git clean` `-fd`
- remove untracked files
`git restore --source=HEAD~1 filename`


#### Branch
`git checkout commit_id`
- that is detached head state
	- Don't make any commit- else it will become dead code
`git checkout commit_id filename`
#### Bisect
`git bisect start`
`git bisect bad`
`git bisect good commit_id`
	`git bisect good/bad`
`git bisect reset`

#### Author stuff
`git shortlog` - author, summary of commit messages
	`-n` - sorted based on author commits
	`-s` - suppress commit messages
	`-e` - author email 
	`--before/after`

`git blame -L n1,n2 filename`
`git tag` - view all tags
	`v1.0 commit_id` create new tag (lightweight tag)
	`-a v1.1 -m"custom_message"` - annotated tag
	`-d v1.1 `


#### Git
`git --version`

`git help`

`git config --list`

`git config --global user.name "name"`
`git config user.name`

`git config --global user.email "name@email.domain"`

`git init` - create new git repository
- Repository
- Working Directory
- Staging Area/Index (for next commit)

**Tracked Files**
- Commit
- Modified
- Staged

`echo "text" >> README.md`

`git add filename` - to add to staging area
`git commit -m "some message"` - create a snapshot from staging area

`git branch -m main` - rename branch to main

`git checkout commit_id` - go back to earlier version

`git remote add origin github_link`

`git push -u origin master`



`git status` 
	- branch info, 
	- comparison with remote branch
	- file changes
	- `-s` | `--short` - just filenames and status

`git diff` 
	- `firstcommit secondcommit` 
	- see difference between two commits (more than status)
	- `--staged`
		- file comparison
		- metadata
		- marked changes
		- chunk/change header
		- actual changes
	- `-- no-renames` 

`git log` 
	- reverse chronological order or git commits
	- `-n` n commits
	- `--oneline`
	- `--stat`

`git rm filename` - removes file and untracks
`git rm --cached filename` - only untracks
`git mv oldname newname` - rename file

`git checkout -b new_branch` - create branch and switch
	- `git branch branchname`
	- `git switch branchname`
`git merge other_branch`

`git branch` - list of branches

`git stash` - it makes another working dir so we do not have to commit
	- `show` - show all
	- `pop` - remove last one
`git reset`
	- `soft` - moves to staging area
	- default - moves to working dir
	- `hard` - delete all changes

#### Git internals
- Persistent Map
	- table with keys and values (sequence of bytes)
	- `echo "something" | git hash-object --stdin -w` - adds to `.git/objects/nn`
	- `git cat-file hash` `-t` for type `-p` to print content
- Content Tracker (from man-page, versioned file system)
	- **Commit**, Stage, Working Dir
		- commits have parent (leaving first one)
		- if some object exists, git just points to old hash
		- reference between commit is used to track history, others are for content
	- **blob** - content of a file (file content)
	- **tree** - directory stored in git (blob, filename + permission + tree)
- Revision Control System (auto update working dir based on branch)
	- **branches** (hash of commit) are stored in `.git/refs/heads/branchname` - `.git/HEAD` points to active branch
			- current branch track new commits
		- **merge** - merge is a commit with 2 parents
			- if there is a merge from branch1, branch2 merge will just point to that merge (fast forward merge)
		- **rebase branch2** (changes history)
			- get the common parent, detach branch1 from that
				- detach = copy commit (with parent/conflict data differing)
			- put the detached commits on top of branch2
	- `git checkout commit` => ditached HEAD
			- WARNING- unreachable commits get garbage collected
	- **Tag** - name for commit (`.git/refs/tags`)
- Distributed Control System
