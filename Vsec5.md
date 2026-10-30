# Experiment 5 -- GitHub Desktop: Branching and .gitignore

## Aim

Using GitHub Desktop, initialize a Git repository, publish it to GitHub,
create a branch named `GIT-EXAM-1`, add a palindrome program and a
squares program, commit and push the changes, create an `IGNORE`
directory containing `test1.txt` and `test2.txt`, add the directory to
`.gitignore`, and demonstrate that the ignored directory is not uploaded
to GitHub.

## Requirements

-   GitHub Desktop installed
-   GitHub account
-   Python installed
-   Visual Studio Code or another text editor
-   Internet connection

## Important notes

-   This guide follows Experiment 5 in the supplied practical PDF. It
    uses GitHub Desktop for Git operations; Python programs may be
    written in a text editor.
-   The PDF names the palindrome file `program2` in one step. This guide
    uses `program2.py`, the normal Python filename extension.
-   Create the Python files on `GIT-EXAM-1`, as required by the PDF.
-   The `.gitignore` file must be created in the project root, alongside
    the Python files.
-   Create `.gitignore` before staging/committing the `IGNORE`
    directory. If the files were already tracked, `.gitignore` alone
    will not untrack them.

## Part A -- Install, sign in, and create repository

### 1. Install GitHub Desktop

1.  Visit https://desktop.github.com/.
2.  Download GitHub Desktop for Windows.
3.  Install and open the application.

### 2. Sign in

1.  Click **Sign in to GitHub.com**.
2.  Complete sign-in in the browser.
3.  Authorize GitHub Desktop and return to the app.

### 3. Create a new repository

1.  In GitHub Desktop, select **File → New repository**.
2.  Name: `Experiment-5`.
3.  Choose a local path where you can easily find the project.
4.  Select **Initialize this repository with a README**.
5.  Click **Create repository**.

### 4. Publish repository to GitHub

1.  Click **Publish repository** in the top toolbar.
2.  Confirm the name `Experiment-5`.
3.  Choose Public or Private as required.
4.  Click **Publish repository**.
5.  Wait for publishing to finish.

### 5. Create branch GIT-EXAM-1

1.  In GitHub Desktop, click **Current Branch**.
2.  Select **New Branch**.
3.  Enter `GIT-EXAM-1`.
4.  Keep the branch based on `main` (or the default branch shown).
5.  Click **Create Branch**.
6.  Confirm that the Current Branch selector shows `GIT-EXAM-1`.

**Important:** Create the Python files after switching to `GIT-EXAM-1`,
so they are committed on the branch required by the practical.

## Part B -- Create Python programs

### 6. Open the repository folder

In GitHub Desktop, select **Repository → Show in Explorer** (Windows).
You can also use **Open in Visual Studio Code** if available.

### 7. Create `program1.py` -- squares from 1 to 10

Create a new file named `program1.py` in the repository root and add:

``` python
for i in range(1, 11):
    print(i, "square =", i * i)
```

Save the file. To test it, open a terminal in the project folder and
run:

``` bash
python program1.py
```

Expected square values: `1, 4, 9, 16, 25, 36, 49, 64, 81, 100`.

### 8. Create `program2.py` -- palindrome check

Create a new file named `program2.py` in the repository root and add:

``` python
text = input("Enter a string: ")

if text == text[::-1]:
    print("Palindrome")
else:
    print("Not a palindrome")
```

Save and test:

``` bash
python program2.py
```

Example: `madam` → `Palindrome`; `hello` → `Not a palindrome`.

### 9. Commit the two Python programs

1.  Return to GitHub Desktop.
2.  Open the **Changes** tab.
3.  Confirm that `program1.py` and `program2.py` are listed.
4.  In **Summary**, enter: `Add palindrome and squares programs`.
5.  Click **Commit to GIT-EXAM-1**.

### 10. Publish the branch

1.  Click **Publish branch** in the top toolbar.
2.  Wait for publishing to finish.
3.  Open your GitHub repository in a browser.
4.  Use the branch selector to select `GIT-EXAM-1`.
5.  Confirm that `program1.py` and `program2.py` are visible.

If the branch has already been published, the toolbar may show **Push
origin** instead. Click it to upload new commits.

## Part C -- Create IGNORE directory and files

### 11. Create the IGNORE directory

In the repository folder, create a folder named exactly `IGNORE`.

### 12. Create test files

Inside `IGNORE`, create: - `test1.txt` with content
`This is test file 1` - `test2.txt` with content `This is test file 2`

The local structure should now be:

``` text
Experiment-5/
├── README.md
├── program1.py
├── program2.py
└── IGNORE/
    ├── test1.txt
    └── test2.txt
```

### 13. Create `.gitignore` in the repository root

Create a file named exactly `.gitignore` beside `README.md`,
`program1.py`, and `program2.py`. Add this line:

``` text
IGNORE/
```

Save the file. Make sure Windows has not named it `.gitignore.txt`. In
File Explorer, enable **View → Show → File name extensions** if
necessary.

## Part D -- Verify ignore rule in GitHub Desktop

### 14. Check the Changes tab

1.  Return to GitHub Desktop.
2.  Select the **Changes** tab.
3.  `.gitignore` should appear as a changed/untracked file.
4.  `IGNORE/test1.txt` and `IGNORE/test2.txt` should not appear.
5.  If the text files appear, check the spelling and location of
    `.gitignore` and confirm it contains `IGNORE/`.

### 15. Commit `.gitignore`

1.  In the Summary box, enter:
    `Add gitignore to ignore IGNORE directory`.
2.  Click **Commit to GIT-EXAM-1**.

### 16. Push the commit

Click **Push origin** in the top toolbar. Wait until the push completes.

## Part E -- Verify the result on GitHub

### 17. Check the remote repository

1.  Open the repository on GitHub.
2.  Select branch `GIT-EXAM-1`.
3.  Refresh the page.
4.  Confirm that `README.md`, `program1.py`, `program2.py`, and
    `.gitignore` are visible.
5.  Confirm that the `IGNORE` directory and its two text files are not
    visible.

### 18. Confirm local files remain

Return to File Explorer. The local `IGNORE` folder should still contain
`test1.txt` and `test2.txt`.

**Result:** The `IGNORE` directory remains on the local computer but is
ignored by Git and is not pushed to GitHub.

## Expected final structure

Local computer:

``` text
Experiment-5/
├── README.md
├── program1.py
├── program2.py
├── .gitignore
└── IGNORE/
    ├── test1.txt
    └── test2.txt
```

GitHub branch `GIT-EXAM-1`:

``` text
Experiment-5/
├── README.md
├── program1.py
├── program2.py
└── .gitignore
```

## Common errors and fixes

  -----------------------------------------------------------------------
  Problem                             Fix
  ----------------------------------- -----------------------------------
  GitHub Desktop is not signed in     Sign in through **File → Options →
                                      Accounts** or the sign-in prompt.

  Repository not found locally        In GitHub Desktop use **File → Add
                                      Local Repository** and select the
                                      project folder.

  Python command not found            Try `py program1.py` on Windows or
                                      install Python and enable PATH.

  Wrong branch selected               Check **Current Branch** and select
                                      `GIT-EXAM-1` before committing.

  Publish branch button not visible   The branch may already be
                                      published; use **Push origin** if
                                      there are new commits.

  IGNORE files appear in Changes      Confirm `.gitignore` is in the
                                      repository root and contains
                                      exactly `IGNORE/`.

  IGNORE files are still on GitHub    They may have been committed before
                                      the ignore rule. Use Git Bash in
                                      the repository:
                                      `git rm -r --cached IGNORE`, then
                                      commit and push.

  `.gitignore` became                 Enable file name extensions in File
  `.gitignore.txt`                    Explorer and rename it to
                                      `.gitignore`.
  -----------------------------------------------------------------------

## Viva questions and short answers

1.  **What is GitHub Desktop?** A graphical application for working with
    Git repositories without typing most Git commands.
2.  **What is a repository?** A project folder tracked by Git.
3.  **What is a branch?** An independent line of development.
4.  **What is the purpose of `GIT-EXAM-1`?** It is the branch used to
    hold the practical's program and ignore-rule changes.
5.  **What is a commit?** A saved snapshot of selected repository
    changes.
6.  **What does Publish repository do?** Creates/publishes the local
    repository on GitHub.
7.  **What does Publish branch do?** Uploads a new local branch to the
    remote repository.
8.  **What does Push origin do?** Uploads local commits to the remote
    repository.
9.  **What is `.gitignore`?** A file containing patterns for untracked
    files and directories Git should ignore.
10. **What does `IGNORE/` mean?** Ignore directories named `IGNORE` and
    their contents.
11. **Does `.gitignore` delete local files?** No, local files remain.
12. **Does `.gitignore` stop tracking already committed files?**
    No. Already tracked files must be removed from the Git index
    separately.
13. **Why is `.gitignore` created in the root?** So its rule applies to
    the `IGNORE` directory under the repository root.
14. **What is the difference between local and remote repository?** The
    local repository is on your computer; the remote repository is
    hosted on GitHub.

## Practical completion checklist

-   [ ] GitHub Desktop installed
-   [ ] Signed in to GitHub
-   [ ] Repository `Experiment-5` created
-   [ ] Repository published to GitHub
-   [ ] Branch `GIT-EXAM-1` created
-   [ ] `program1.py` created and tested
-   [ ] `program2.py` created and tested
-   [ ] Both programs committed to `GIT-EXAM-1`
-   [ ] Branch published to GitHub
-   [ ] `IGNORE` directory created
-   [ ] `test1.txt` and `test2.txt` created
-   [ ] `.gitignore` created in repository root with `IGNORE/`
-   [ ] GitHub Desktop shows `.gitignore` but not the ignored text files
-   [ ] `.gitignore` committed and pushed
-   [ ] GitHub confirms `IGNORE` is absent
-   [ ] Local computer still contains the `IGNORE` directory and files

**Exam tip:** The important demonstration is that `IGNORE` remains on
your computer but does not appear in GitHub. Make sure `.gitignore` is
committed and pushed on `GIT-EXAM-1`.
