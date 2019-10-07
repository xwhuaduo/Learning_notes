### Lesson 1 What is Version Control?
- Secure Hash Algorithm: SHA

### Lesson 2 Create A Git Repo
- `git init` : create brand new repositories (repos) on your computer
- `git clone`: copy existing repos from somewhere to your local computer
- `git status`: check the status of a repo

### Lesson 3 Review a Repo's History
- `git log`: Displays information about the existing commits
  - to scroll **down**, press
    - `j` or `down` to move one line at a time
    - `d` to move by half the page screen
    - `f` to move by a whole page screen
  - to scroll **up**, press
    - `k` or `up` to move up one line at a time
    - `u` to move by half the page screen
    - `b` to move by a whole page screen
  - press `q` to quit out of the log
- Display flag:
  1. `-l` for long
  2. `git log --oneline`
  3. `git log --stat`
  display the files that have been changed in the commit, as well as the number of lines that have been added or deleted.
	4. `git log -p` or `--patch`
  display the actual changes made to a file
- Viewing A Specific Commit:
  - Providing the SHA of the commit you want to see to `git log`
    - `git log -p fdf5493`
    - Keep in mind that it will also show all of the commits that were made **prior** to the supplied SHA.
- `git show`: Displays information about the given commit (by providing ID)
  - Show only one commit

### Lesson 4 Add Commits To A Repo

- `git add`
add files from the working directory to the staging index
  - `git add .` : `.` can be used in place of a list of files to tell Git to add the current directory (and all nested files)
- `git commit` take files from the staging index and save them in the repository
  - Bypass the Editor with the `-m` flag
    - `git commit -m "Initial commit"`
  - About commit message
    - Do
      - Keep short
      - Explain what the commit does
      - Be consistent
    - Do not
      - Explain why and how
        - After the message, leave a blank line, and then type out the body or explanation including details about why the commit is needed 
			- Use "and"
- `git diff` display the difference between the two versions of a file.
  - is used to see changes that have been made but haven't been committed, yet
- To ignore certain files:
	1. Create a .gitignore file under the same directory that contain .git
	2. then add the file names to .gitignore
- Globbing: use special characters to macth patterns/characters.
	- blank lines can be used for spacing
	- `#` - marks line as a comment
	- `*` - matches 0 or more characters
    - e.g. `sampels/*.jpg`
	- `?` - matches 1 character
	- `[abc]` - matches a, b, or c
	- `**` - matches nested directories
  - `a/**/z` matches
  	- a/z
  	- a/b/z
  	- a/b/c/z

### Lesson 5: Tagging, Branching, and Merging
#### `git tag`: add tags to specific commits
- `git tag -a v1.0`: the flag tells Git to create an **annotated** tag to the most recent commit.
  - Annotated tag include a lot of extra information:
    - The person who made the tag
    - The date the tag was made
    - A message for the tag
- `git tag  v1.0`: the flag tells Git to create an **lightweight** tag.
- `git tag` will display all tags that are in the repository.
- `git log --decorate` flag will show us some details that are hidden from the default view. Now used by default

#### Deleting a tag
- `git tag -d v1.0`
- `git tag -a v1.0 af16029`

#### `git branch`: allows multiple lines of development
- List all branch names in the repo
- Craete new branches
- Delete branches
  - `git branch -d sidebar`
    - You can't delete a branch that you're currently on.
    - Git won't let you delete a branch if it has commits on it that aren't on any other branch (meaning the commits are unique to the branch that's about to be deleted).
  - To force the deletion, `git branch -D sidebar`

#### `git checkout`: switch between different branches and tags
- Switch and create branch in one command
  - `git checkout -b new_branch`
- See all branches at once
  - `git log --oneline --decorate --graph --all`
    - `--graph` flag adds the bullets and lines to the leftmost part of the output
		- `--all` flag is what displays all of the branches in the repo

#### `git merge`: combines changes on different branches
- Making a merge makes a commit
- If you make a merge on the wrong branch, use this command to undo the merge: `git reset --hard HEAD^` ???
- When a merge is performed, the other branch's changes are brought into the branch that's currently checked out. That is, when we merge, we're merging some other branch into the **current** (checked-out) branch. We're not merging two branches into a new branch. We're not merging the current branch into the other branch.
1. Regular merge
  - Two divergent branches are combined
2. Fast-forward merge
  - Move the currently checked out branch forward until it points to the same commit that the other branch is pointing to.
3. Merge conflict

### Lession 6 Undoing changes
#### 1. `git commit --amend` Alter the most-recent commit
- with `--amend` flag, you can alter the *most-recent* commit.
1. Change commit message
  - need working tree clean
  - will let you provide a new commit message
2. Add forgotten files to commit
  1. edit the file(s)
  2. save the file(s)
  3. stage the file(s)
  4. and run `git commit --amend`

#### 2. `git revert ac3f78` Reverse given commit
- Create a new commit that revert a specific commit, that is operate the opposite way of a specific commit.
- will undo the changes that were made by the provided commits
- creates a new commit to record the change

#### 3. `git reset` Erases commits
###### Relative Commit References
- `^` indicates the parent commit of the current commit
- `~` indicate the first parent commit of the current commit
- e.g.
  - parent commit: `HEAD^`, `HEAD~`, `HEAD~1`
  - grandparent commit: `HEAD^^`, `HEAD~2`
- A merge commit has two parents.
  - `^` indicates the first parent， which is the branch you were on when you ran `git merge`
  - `^2` indicates the second parent, which is the branch that was merged in
###### The `git reset <--Flag> <reference-to-commit>` command
- It can be used to:
  - move the HEAD and current branch pointer to the referenced commit
  - erase (the commit after the referenced) commits with `--hard` flag
  - move committed changes to the staging index with `--soft` flag
  - unstage committed changes. Move to working directory? with `--mixed` flag, used by default
- Better to create a `backup` branch on the most-recent commit before resetting.
  - you can get back to having the `master` branch point to the same commit as the `backup` branch by
    1. remove the uncommitted changes from the workin directory
    2. merge `backup` into `master`(which will cause a Fast-forward merge and move `master` up to the same points as `backup`)
    -
```
    # restore index.html to the state of the most recent git add or git commit
    git checkout -- index.html
    git merge backup
```
- Git keep track of everything for about 30 days before it completely erases anything.
