# Experiment 3 -- Git Branching, Pulling Changes and Merge Conflicts

## Aim

Initialize a Git repository, create and push `GIT-EXAM-1`, add a
palindrome program and a squares program, create `GIT-EXAM-2`, bring
changes from `GIT-EXAM-1` into it, make conflicting edits to
`program1.py`, push the branches, compare them, resolve the merge
conflict, and create a Pull Request from `GIT-EXAM-2` to `GIT-EXAM-1`.

## Important notes about the supplied practical

-   The PDF first calls the palindrome file `program2`, then later calls
    it `program2.py`. This guide uses `program2.py` consistently because
    it is a Python file.
-   The PDF says to pull changes from `GIT-EXAM-1` into `GIT-EXAM-2` and
    then demonstrate conflicts. A conflict requires both branches to
    edit the same part of a file differently. Therefore, this guide
    deliberately makes one different edit to `program1.py` on each
    branch after `GIT-EXAM-2` has received the first branch's changes.
-   This guide uses `main`. If your lab uses `master`, replace `main`
    consistently.

## Requirements

-   Git and Git Bash
-   Python
-   GitHub account and internet connection

## Before starting

Create an **empty** GitHub repository named `Experiment-3` at
https://github.com/new. Do not add a README, `.gitignore`, or license.
Replace `YOUR-USERNAME` and the email placeholder with your own details.

## Part A -- Initialize repository and create GIT-EXAM-1

### 1. Check installations

``` bash
git --version
python --version
```

If `python` is not recognized, try `python3 --version` and use that
command to run scripts.

### 2. Create project folder and initialize Git

``` bash
mkdir Experiment-3
cd Experiment-3
git init
git status
```

### 3. Configure Git identity

``` bash
git config --global user.name "Your Name"
git config --global user.email "your-email@gmail.com"
git config --global --list
```

### 4. Create GitHub repository and add remote

Create an empty GitHub repository named `Experiment-3`, then run:

``` bash
git remote add origin https://github.com/YOUR-USERNAME/Experiment-3.git
git remote -v
```

If `origin` already exists:

``` bash
git remote set-url origin https://github.com/YOUR-USERNAME/Experiment-3.git
```

### 5. Create and switch to GIT-EXAM-1

``` bash
git branch -M main
git switch -c GIT-EXAM-1
git branch
```

If Git reports that the branch cannot be created because there is no
first commit yet, create a small initial file and commit it first:

``` bash
echo "# Experiment 3" > README.md
git add README.md
git commit -m "Initial commit"
git switch -c GIT-EXAM-1
```

If you made this initial commit, push `main` later if your instructor
expects it on GitHub.

### 6. Create `program2.py` -- check palindrome

A palindrome reads the same forward and backward (for example, `madam`).

``` bash
cat > program2.py <<'EOF'
text = input("Enter a string: ")

if text == text[::-1]:
    print("Palindrome")
else:
    print("Not a palindrome")
EOF
```

Run:

``` bash
python program2.py
```

Example input `madam` → `Palindrome`; input `hello` →
`Not a palindrome`.

### 7. Create `program1.py` -- squares from 1 to 10

``` bash
cat > program1.py <<'EOF'
for i in range(1, 11):
    print(i, "square =", i * i)
EOF
```

Run:

``` bash
python program1.py
```

Expected values: `1, 4, 9, 16, 25, 36, 49, 64, 81, 100`.

### 8. Commit and push GIT-EXAM-1

``` bash
git add program1.py program2.py
git status
git commit -m "Add squares and palindrome programs"
git push -u origin GIT-EXAM-1
```

Check GitHub and confirm that `GIT-EXAM-1` contains both files.

## Part B -- Create GIT-EXAM-2 and pull changes from GIT-EXAM-1

### 9. Create GIT-EXAM-2 from main

``` bash
git switch main
git switch -c GIT-EXAM-2
```

If you created the optional initial README commit above, this branch
starts from that initial `main` commit.

### 10. Pull changes from remote GIT-EXAM-1 into GIT-EXAM-2

``` bash
git pull origin GIT-EXAM-1
```

This brings the files and commits from `GIT-EXAM-1` into the currently
checked-out branch, `GIT-EXAM-2`.

Verify:

``` bash
ls
git log --oneline --all --decorate -5
```

You should now see `program1.py` and `program2.py`.

### 11. Push GIT-EXAM-2 to GitHub

``` bash
git push -u origin GIT-EXAM-2
```

If Git says the branch has no upstream, this command sets it.

## Part C -- Make different edits to the same file to create a conflict

A merge conflict is not guaranteed merely by creating two branches. Both
branches must change the same lines differently. The next steps
intentionally create that situation.

### 12. On GIT-EXAM-1, modify `program1.py`

``` bash
git switch GIT-EXAM-1
cat > program1.py <<'EOF'
for i in range(1, 11):
    print("Square of", i, "is", i * i)
EOF
git add program1.py
git commit -m "Update squares output on GIT-EXAM-1"
git push
```

### 13. On GIT-EXAM-2, modify the same lines differently

``` bash
git switch GIT-EXAM-2
cat > program1.py <<'EOF'
for i in range(1, 11):
    print("Number:", i, "| Square:", i ** 2)
EOF
git add program1.py
git commit -m "Update squares output on GIT-EXAM-2"
git push
```

Both branches now changed `program1.py` differently. This sets up the
merge conflict required for demonstration.

## Part D -- Compare branches and resolve the conflict

### 14. Compare branch history and changed files

``` bash
git branch
git log --oneline --graph --all --decorate
git diff --name-status GIT-EXAM-1..GIT-EXAM-2
```

To compare the version of `program1.py` in each branch:

``` bash
git diff GIT-EXAM-1..GIT-EXAM-2 -- program1.py
```

### 15. Merge GIT-EXAM-1 into GIT-EXAM-2 locally to demonstrate the conflict

``` bash
git switch GIT-EXAM-2
git merge GIT-EXAM-1
```

Git should report a conflict in `program1.py`. Check:

``` bash
git status
```

Open `program1.py` in a text editor. Git inserts conflict markers
similar to:

``` text
<<<<<<< HEAD
for i in range(1, 11):
    print("Number:", i, "| Square:", i ** 2)
=======
for i in range(1, 11):
    print("Square of", i, "is", i * i)
>>>>>>> GIT-EXAM-1
```

`HEAD` is the current branch (`GIT-EXAM-2`); the other section is from
`GIT-EXAM-1`.

### 16. Resolve the conflict

Choose the version you want to keep, or combine both. For example,
replace the entire file with:

``` python
for i in range(1, 11):
    print("Square of", i, "is", i ** 2)
```

Save the file. Remove all `<<<<<<<`, `=======`, and `>>>>>>>` markers.

Then run and test:

``` bash
python program1.py
```

Mark the conflict as resolved and commit:

``` bash
git add program1.py
git commit -m "Resolve program1.py merge conflict"
git push
```

Now `GIT-EXAM-2` contains the merged changes. The conflict has been
resolved on that branch.

## Part E -- Create Pull Request from GIT-EXAM-2 to GIT-EXAM-1

### 17. Create the Pull Request on GitHub

1.  Open the GitHub repository.
2.  Go to **Pull requests** → **New pull request**.
3.  Set **base** to `GIT-EXAM-1`.
4.  Set **compare** to `GIT-EXAM-2`.
5.  Review the changed files.
6.  Enter a title such as `Merge GIT-EXAM-2 into GIT-EXAM-1`.
7.  Click **Create pull request**.
8.  If GitHub reports no conflicts, click **Merge pull request** and
    then **Confirm merge**. If GitHub reports conflicts, resolve them on
    GitHub or locally before merging.

### 18. Verify after merge

``` bash
git switch GIT-EXAM-1
git pull origin GIT-EXAM-1
git log --oneline --graph --all --decorate
ls
```

Confirm that `program1.py` and `program2.py` are present and that the
pull request shows as merged on GitHub.

## Expected final result

-   `GIT-EXAM-1` contains `program1.py` (squares) and `program2.py`
    (palindrome).
-   `GIT-EXAM-2` receives the changes from `GIT-EXAM-1`, then makes a
    different edit to `program1.py`.
-   A merge conflict is deliberately produced and resolved.
-   A Pull Request is created with base `GIT-EXAM-1` and compare
    `GIT-EXAM-2`.

## Common errors and fixes

  ---------------------------------------------------------------------------------------------------
  Error / situation                                Fix
  ------------------------------------------------ --------------------------------------------------
  `git: command not found`                         Install Git for Windows and reopen Git Bash.

  `python: command not found`                      Try `python3`, or install Python and enable PATH.

  `fatal: not a git repository`                    Use `pwd` and `cd` into the project folder.

  `remote origin already exists`                   Use
                                                   `git remote set-url origin YOUR-REPOSITORY-URL`.

  `fatal: refusing to merge unrelated histories`   Usually the GitHub repository was initialized
                                                   separately. Prefer an empty GitHub repository for
                                                   this practical. Ask your instructor before using
                                                   `--allow-unrelated-histories`.

  `Already up to date`                             Check current branch with `git branch`; confirm
                                                   the other branch has been pushed and contains
                                                   commits.

  Merge conflict appears                           Edit the conflicted file, remove conflict markers,
                                                   save, then `git add` and `git commit`.

  `git push` rejected                              Run `git status` and integrate the remote changes
                                                   carefully before pushing.
  ---------------------------------------------------------------------------------------------------

## Viva questions and short answers

1.  **What is a branch in Git?** An independent line of development.
2.  **What does `git switch -c` do?** Creates a branch and switches to
    it.
3.  **What does `git pull origin GIT-EXAM-1` do?** Fetches the remote
    branch and integrates it into the current branch.
4.  **What is a merge conflict?** A situation where Git cannot
    automatically combine changes, commonly because the same lines were
    changed differently.
5.  **Why did we create a conflict in `program1.py`?** Both branches
    changed the same lines with different content.
6.  **What do `<<<<<<<`, `=======`, and `>>>>>>>` mean?** They mark the
    conflicting sections in a file.
7.  **What does `git status` show during a conflict?** It identifies
    files that need conflict resolution.
8.  **How do you mark a conflict resolved?** Edit the file, then run
    `git add filename`.
9.  **What does `git merge` do?** Combines changes from another branch
    into the current branch.
10. **What is a Pull Request?** A proposal to merge one branch into
    another on GitHub.
11. **What is the base branch in this practical's Pull Request?**
    `GIT-EXAM-1`.
12. **What is the compare branch?** `GIT-EXAM-2`.
13. **What is a palindrome?** A string that reads the same forward and
    backward.
14. **What does `range(1, 11)` generate?** Integers 1 through 10; the
    stop value 11 is excluded.

## Practical completion checklist

-   [ ] Git and Python versions checked
-   [ ] Empty GitHub repository created and remote configured
-   [ ] `GIT-EXAM-1` created
-   [ ] `program2.py` palindrome program created and tested
-   [ ] `program1.py` squares 1 to 10 program created and tested
-   [ ] Changes committed and pushed to `GIT-EXAM-1`
-   [ ] `GIT-EXAM-2` created
-   [ ] Changes pulled from `GIT-EXAM-1` into `GIT-EXAM-2`
-   [ ] Different edits made to `program1.py` on both branches
-   [ ] Branch differences demonstrated
-   [ ] Merge conflict demonstrated and resolved
-   [ ] Changes pushed
-   [ ] Pull Request created from `GIT-EXAM-2` into `GIT-EXAM-1`
-   [ ] Pull Request merged and final state verified

**Exam tip:** A conflict occurs only when branches have incompatible
edits. Do not expect a conflict just because two branches exist. Run
`git status` whenever Git pauses for conflict resolution.
