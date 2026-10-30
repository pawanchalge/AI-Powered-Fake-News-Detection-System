# Experiment 2 -- Git Bash: Even/Odd, Reverse String and Pull Request

## Aim

Initialize a Git repository, push it to GitHub, create an even/odd
program on `main`, create a `GIT-EXAM` branch with a reverse-string
program, compare the branches, and create a Pull Request to merge
`GIT-EXAM` into `main`.

## Requirements

-   Git and Git Bash installed
-   Python installed
-   GitHub account and internet connection

## Before starting

Create an **empty** GitHub repository named `Experiment-2` at
https://github.com/new. Do not initialize it with a README,
`.gitignore`, or license. Replace `YOUR-USERNAME` and the email
placeholder with your own details.

This guide uses `main` as the default branch. If your instructor
requires `master`, use `master` consistently instead of `main`.

## Part A -- Create repository and push even/odd program

### 1. Check Git and Python

``` bash
git --version
python --version
```

If `python` is not recognized, try `python3 --version` and use `python3`
to run the programs.

### 2. Create project folder and initialize Git

``` bash
mkdir Experiment-2
cd Experiment-2
git init
git status
```

### 3. Configure your Git identity

``` bash
git config --global user.name "Your Name"
git config --global user.email "your-email@gmail.com"
git config --global --list
```

### 4. Create an empty GitHub repository

1.  Open https://github.com/new.
2.  Enter repository name: `Experiment-2`.
3.  Choose Public or Private.
4.  Leave README, `.gitignore`, and license unchecked.
5.  Click **Create repository** and copy its HTTPS URL.

### 5. Connect local repository to GitHub

``` bash
git remote add origin https://github.com/YOUR-USERNAME/Experiment-2.git
git remote -v
```

If `origin` already exists, update it:

``` bash
git remote set-url origin https://github.com/YOUR-USERNAME/Experiment-2.git
```

### 6. Create `program1.py` -- check even or odd

``` bash
cat > program1.py <<'EOF'
num = int(input("Enter a number: "))

if num % 2 == 0:
    print("Even number")
else:
    print("Odd number")
EOF
```

Run:

``` bash
python program1.py
```

Example: input `7` should display `Odd number`. Input `8` should display
`Even number`.

### 7. Add, commit, and push to `main`

``` bash
git add program1.py
git status
git commit -m "Add even odd program"
git branch -M main
git push -u origin main
```

Refresh the GitHub repository and confirm that `program1.py` is visible
on `main`.

## Part B -- Create GIT-EXAM branch and reverse-string program

### 8. Create and switch to `GIT-EXAM`

``` bash
git switch -c GIT-EXAM
git branch
```

The asterisk (`*`) should appear beside `GIT-EXAM`. For older Git
versions, use `git checkout -b GIT-EXAM`.

### 9. Create `program2.py` -- reverse a string

``` bash
cat > program2.py <<'EOF'
text = input("Enter a string: ")

reverse = text[::-1]

print("Reverse string:", reverse)
EOF
```

Run:

``` bash
python program2.py
```

Example: input `hello` should display `olleh`.

### 10. Commit and push `GIT-EXAM`

``` bash
git add program2.py
git status
git commit -m "Add reverse string program"
git push -u origin GIT-EXAM
```

Refresh GitHub. Confirm that `GIT-EXAM` appears in the branch selector
and contains `program2.py`.

## Part C -- Compare branches

### 11. List local branches

``` bash
git branch
```

### 12. Show commits present in GIT-EXAM but not main

``` bash
git log --oneline main..GIT-EXAM
```

### 13. Show files changed between branches

``` bash
git diff --name-status main..GIT-EXAM
```

Expected: `A program2.py` (`A` means Added).

To view the actual code difference:

``` bash
git diff main..GIT-EXAM
```

## Part D -- Create a Pull Request on GitHub

### 14. Open the Pull Request

1.  Open your GitHub repository.
2.  Select the `GIT-EXAM` branch, or open the **Pull requests** tab.
3.  Click **Compare & pull request** if shown, or **New pull request**.
4.  Set **base** to `main` and **compare** to `GIT-EXAM`.
5.  Confirm that `program2.py` is listed in the changes.
6.  Enter a title such as `Merge GIT-EXAM into main`.
7.  Click **Create pull request**.

### 15. Merge the Pull Request

1.  On the Pull Request page, click **Merge pull request**.
2.  Click **Confirm merge**.
3.  The Pull Request should show as merged.

### 16. Update local main and verify

``` bash
git switch main
git pull origin main
ls
```

Expected files on `main` after the merge:

``` text
program1.py
program2.py
```

The merge makes the branch changes part of `main`. The `GIT-EXAM` branch
normally remains on GitHub unless it is explicitly deleted.

## Expected final result

-   Before merging: `program1.py` is on `main`; `program1.py` and
    `program2.py` are on `GIT-EXAM`.
-   After merging the Pull Request: both programs are available on
    `main`.
-   GitHub shows a merged Pull Request from `GIT-EXAM` into `main`.

## Common errors and fixes

  ------------------------------------------------------------------------------------------
  Error / situation                       Fix
  --------------------------------------- --------------------------------------------------
  `git: command not found`                Install Git for Windows and reopen Git Bash.

  `python: command not found`             Try `python3`; install Python and enable PATH if
                                          needed.

  `remote origin already exists`          Use
                                          `git remote set-url origin YOUR-REPOSITORY-URL`.

  `src refspec main does not match any`   Make sure a commit exists, then check
                                          `git branch`.

  Authentication failed                   Complete browser sign-in / Git Credential Manager.
                                          GitHub does not accept account passwords for Git
                                          HTTPS authentication.

  `nothing to commit`                     Run `git status`; check that the file is saved in
                                          the repository folder.

  `branch already exists`                 Use `git switch GIT-EXAM` instead of creating it
                                          again.

  Pull Request has no changes             Confirm you pushed `GIT-EXAM` and selected base
                                          `main`, compare `GIT-EXAM`.
  ------------------------------------------------------------------------------------------

## Viva questions and short answers

1.  **What is Git?** A distributed version control system used to track
    file changes.
2.  **What is GitHub?** An online platform for hosting Git repositories
    and collaboration.
3.  **What is a repository?** A project folder whose changes are tracked
    by Git.
4.  **What does `git init` do?** Initializes a local Git repository.
5.  **What does `git add` do?** Stages changes for the next commit.
6.  **What is a commit?** A saved snapshot of staged changes.
7.  **What does `git push` do?** Uploads local commits to a remote
    repository.
8.  **What is a branch?** An independent line of development.
9.  **What is a Pull Request?** A proposal to merge changes from one
    branch into another.
10. **What is the base branch?** The branch that will receive the
    proposed changes.
11. **What is the compare branch?** The branch containing the proposed
    changes.
12. **What does `text[::-1]` do?** It returns the string in reverse
    order using Python slicing.
13. **What does `git diff main..GIT-EXAM` show?** Differences between
    the two branch tips.
14. **Does creating a Pull Request automatically merge code?** No. It
    must be reviewed and merged.

## Practical completion checklist

-   [ ] Git and Python versions checked
-   [ ] Empty GitHub repository created
-   [ ] Local repository initialized
-   [ ] Remote `origin` configured
-   [ ] `program1.py` created, tested, committed, and pushed to `main`
-   [ ] `GIT-EXAM` branch created
-   [ ] `program2.py` created and tested
-   [ ] Branch committed and pushed
-   [ ] Differences between `main` and `GIT-EXAM` demonstrated
-   [ ] Pull Request created with base `main` and compare `GIT-EXAM`
-   [ ] Pull Request merged
-   [ ] Local `main` pulled and both files verified

**Exam tip:** Run one command at a time. Verify important stages with
`git status`, `git branch`, and the GitHub branch selector. Do not paste
`YOUR-USERNAME` literally.
