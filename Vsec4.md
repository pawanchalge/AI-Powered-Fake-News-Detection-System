# Experiment 4 -- Git Branching and .gitignore

## Aim

Initialize a Git repository, create a branch named `GIT-EXAM-1`, create
a palindrome program and a squares program, push the code to GitHub,
create an `IGNORE` directory containing `test1.txt` and `test2.txt`,
create a `.gitignore` file to ignore the `IGNORE` directory, and
demonstrate that the ignored directory is not pushed to GitHub.

## Requirements

-   Git and Git Bash installed
-   Python installed
-   GitHub account
-   Internet connection

## Important notes

-   This guide follows the task wording in your practical PDF.
-   The PDF says to create a file named `program2` and later refers to
    `program2.py`. This guide uses `program2.py`, which is the correct
    Python filename extension.
-   This guide uses `main` as the default branch. If your instructor
    requires `master`, use `master` consistently.
-   Create a fresh, empty GitHub repository for this experiment. Do not
    add a README, `.gitignore`, or license on GitHub.

## Part A -- Initialize repository and create programs

### 1. Check Git and Python

``` bash
git --version
python --version
```

If `python` is not recognized, try:

``` bash
python3 --version
```

Use `python3` instead of `python` in later run commands if necessary.

### 2. Create project folder and initialize Git

``` bash
mkdir Experiment-4
cd Experiment-4
git init
git status
```

### 3. Configure Git identity

Use your own name and email:

``` bash
git config --global user.name "Your Name"
git config --global user.email "your-email@gmail.com"
git config --global --list
```

### 4. Create an empty GitHub repository

1.  Open https://github.com/new.
2.  Repository name: `Experiment-4`.
3.  Choose Public or Private.
4.  Leave README, `.gitignore`, and license unchecked.
5.  Click **Create repository** and copy its HTTPS URL.

### 5. Connect local repository to GitHub

Replace `YOUR-USERNAME` with your GitHub username:

``` bash
git remote add origin https://github.com/YOUR-USERNAME/Experiment-4.git
git remote -v
```

If `origin` already exists, update it:

``` bash
git remote set-url origin https://github.com/YOUR-USERNAME/Experiment-4.git
```

### 6. Create the first commit on main

A new Git repository needs at least one commit before creating the
experiment branch:

``` bash
git branch -M main
echo "# Experiment 4" > README.md
git add README.md
git commit -m "Initial commit"
git push -u origin main
```

### 7. Create and switch to GIT-EXAM-1

``` bash
git switch -c GIT-EXAM-1
git branch
```

The asterisk (`*`) should appear beside `GIT-EXAM-1`. Older Git
alternative:

``` bash
git checkout -b GIT-EXAM-1
```

### 8. Create `program2.py` -- palindrome check

A palindrome reads the same forward and backward, such as `madam`.

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

Example: - Input `madam` → `Palindrome` - Input `hello` →
`Not a palindrome`

### 9. Create `program1.py` -- squares from 1 to 10

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

Expected square values: `1, 4, 9, 16, 25, 36, 49, 64, 81, 100`.

### 10. Add, commit, and push the programs

``` bash
git add program1.py program2.py
git status
git commit -m "Add palindrome and squares programs"
git push -u origin GIT-EXAM-1
```

Open GitHub, select branch `GIT-EXAM-1`, and confirm that `program1.py`
and `program2.py` are visible.

## Part B -- Create IGNORE directory and .gitignore

### 11. Create the IGNORE directory

Make the directory:

``` bash
mkdir IGNORE
```

Create `test1.txt`:

``` bash
echo "This is test file 1" > IGNORE/test1.txt
```

Create `test2.txt`:

``` bash
echo "This is test file 2" > IGNORE/test2.txt
```

Check the files:

``` bash
ls IGNORE
```

Expected:

``` text
test1.txt
test2.txt
```

### 12. Check Git status before ignoring

``` bash
git status
```

At this point Git will normally show the `IGNORE/` directory as
untracked. This is because Git can see the files and they are not yet
excluded.

### 13. Create `.gitignore`

Create a file named exactly `.gitignore` in the project root:

``` bash
echo "IGNORE/" > .gitignore
```

The trailing slash indicates that the directory named `IGNORE` should be
ignored.

View the file:

``` bash
cat .gitignore
```

Expected:

``` text
IGNORE/
```

### 14. Verify that Git ignores the directory

``` bash
git status
```

Expected: `IGNORE/` should no longer appear as an untracked change.
`.gitignore` should appear as an untracked file.

To explicitly check an ignored file:

``` bash
git check-ignore -v IGNORE/test1.txt
```

Expected output identifies the `.gitignore` rule that ignores
`IGNORE/test1.txt`.

### 15. Add and commit `.gitignore`

``` bash
git add .gitignore
git status
git commit -m "Add gitignore to exclude IGNORE directory"
```

### 16. Push the commit

``` bash
git push
```

If Git asks for an upstream, use:

``` bash
git push -u origin GIT-EXAM-1
```

## Part C -- Demonstrate IGNORE is not pushed to GitHub

### 17. Verify local directory still exists

``` bash
ls
ls IGNORE
```

The `IGNORE` directory and its two text files still exist on your
computer. `.gitignore` does not delete local files; it tells Git not to
track them.

### 18. Verify ignored status

``` bash
git status --short
git check-ignore -v IGNORE/test1.txt IGNORE/test2.txt
```

Expected: - No `IGNORE/` files are listed as staged or untracked. -
`git check-ignore` shows that both text files match the `IGNORE/` rule.

### 19. Verify on GitHub

1.  Open your GitHub repository.
2.  Select branch `GIT-EXAM-1`.
3.  Refresh the page.
4.  Confirm `.gitignore`, `program1.py`, and `program2.py` are present.
5.  Confirm that the `IGNORE` directory is not present.

## Expected final repository structure

Local computer:

``` text
Experiment-4/
├── README.md
├── program1.py
├── program2.py
├── .gitignore
└── IGNORE/
    ├── test1.txt
    └── test2.txt
```

GitHub (`GIT-EXAM-1` branch):

``` text
Experiment-4/
├── README.md
├── program1.py
├── program2.py
└── .gitignore
```

The local `IGNORE/` folder remains on your computer but is excluded from
Git tracking and therefore is not pushed.

## Important warning

`.gitignore` only prevents **untracked** files from being added. If a
file inside `IGNORE/` was already committed or tracked before adding the
rule, Git will continue tracking it. For this experiment, add
`.gitignore` before staging or committing the `IGNORE/` files. If they
were already tracked, remove them from Git's index with:

``` bash
git rm -r --cached IGNORE
git add .gitignore
git commit -m "Stop tracking IGNORE directory"
git push
```

This removes the directory from Git tracking but keeps the local files.

## Common errors and fixes

  ------------------------------------------------------------------------------------------
  Error / situation                       Fix
  --------------------------------------- --------------------------------------------------
  `git: command not found`                Install Git for Windows and reopen Git Bash.

  `python: command not found`             Try `python3`, or install Python and enable PATH.

  `fatal: not a git repository`           Use `cd Experiment-4` to enter the project
                                          directory.

  `remote origin already exists`          Use
                                          `git remote set-url origin YOUR-REPOSITORY-URL`.

  `src refspec main does not match any`   Make an initial commit before pushing.

  `IGNORE/` still appears in Git status   Check that `.gitignore` is in the repository root
                                          and contains exactly `IGNORE/`.

  Files in IGNORE still appear on GitHub  They may already be tracked; use
                                          `git rm -r --cached IGNORE`, commit, and push.

  `.gitignore` is not visible             Ensure the filename is exactly `.gitignore`, not
                                          `.gitignore.txt`.
  ------------------------------------------------------------------------------------------

## Viva questions and short answers

1.  **What is Git?** A distributed version control system used to track
    changes in files.
2.  **What is GitHub?** An online platform for hosting Git repositories
    and collaboration.
3.  **What is a branch?** An independent line of development.
4.  **What is `.gitignore`?** A file containing patterns for files and
    directories Git should ignore when they are untracked.
5.  **Why do we use `.gitignore`?** To prevent unnecessary files,
    temporary files, or sensitive local files from being added to the
    repository.
6.  **What does `IGNORE/` mean in `.gitignore`?** Ignore directories
    named `IGNORE` and their contents.
7.  **Does `.gitignore` delete local files?** No. It only tells Git to
    ignore matching untracked files.
8.  **Does `.gitignore` stop tracking files already committed?**
    No. Already tracked files remain tracked until removed from the Git
    index.
9.  **What does `git status` do?** Shows the current state of the
    working tree and staging area.
10. **What does `git check-ignore -v` do?** Shows which ignore rule
    matches a path.
11. **What does `git rm -r --cached IGNORE` do?** Removes the directory
    from Git's index while keeping local files.
12. **What is the difference between `git add` and `git commit`?**
    `git add` stages changes; `git commit` saves the staged snapshot.

## Practical completion checklist

-   [ ] Git and Python versions checked
-   [ ] Empty GitHub repository created
-   [ ] Local repository initialized and connected to GitHub
-   [ ] Initial commit pushed to `main`
-   [ ] `GIT-EXAM-1` branch created
-   [ ] `program2.py` palindrome program created and tested
-   [ ] `program1.py` squares program created and tested
-   [ ] Programs committed and pushed
-   [ ] `IGNORE` directory created
-   [ ] `test1.txt` and `test2.txt` created inside `IGNORE`
-   [ ] `.gitignore` created with `IGNORE/`
-   [ ] `git status` confirms the directory is ignored
-   [ ] `.gitignore` committed and pushed
-   [ ] GitHub confirms `IGNORE` directory is absent
-   [ ] Local computer still contains the ignored files

**Exam tip:** Create `.gitignore` before staging the `IGNORE` directory.
Remember: ignored files remain on your computer; they are simply
excluded from Git tracking.
