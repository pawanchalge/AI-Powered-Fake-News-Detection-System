# Experiment 6: Git Bash -- Fork, Feature Branch and Pull Request

## Aim

To clone a public GitHub repository, fork it, create a feature branch,
make changes, push the branch to your fork, submit a Pull Request, and
understand how to resolve merge conflicts and incorporate review
feedback.

## Repository given in the practical

Original repository: https://github.com/KalyanM45/AI-Project-Gallery.git

## Important concept

You can create a local branch in a repository that you cloned. However,
without write permission, you generally cannot push that branch to the
original repository. Forking creates your own GitHub copy, where you can
push your changes.

-   **Clone:** Downloads a remote repository to your computer.
-   **Fork:** Creates a copy of another user's repository under your
    GitHub account.
-   **origin:** The remote that points to your fork.
-   **upstream:** The remote that points to the original repository.
-   **Feature branch:** A separate branch used to make changes without
    directly changing the main branch.
-   **Pull Request (PR):** A request asking the original project owner
    to review and merge your changes.

------------------------------------------------------------------------

## Step 1: Clone the original repository

Open Git Bash and run:

``` bash
git clone https://github.com/KalyanM45/AI-Project-Gallery.git
cd AI-Project-Gallery
ls
git remote -v
```

**Expected result:** The project files are downloaded. Initially,
`origin` points to the original repository.

## Step 2: Demonstrate the push permission restriction

Create a local branch:

``` bash
git switch -c GIT-EXAM
git branch
```

The `GIT-EXAM` branch should be marked with `*`.

**Important:** Local branch creation is normally allowed. The permission
restriction is usually on pushing to the original repository. To
demonstrate this, you may try:

``` bash
git push -u origin GIT-EXAM
```

If your account does not have write permission, GitHub rejects the push.
If it succeeds, your account has write access and this restriction does
not apply.

## Step 3: Fork the repository

1.  Open https://github.com/KalyanM45/AI-Project-Gallery
2.  Sign in to your GitHub account.
3.  Click **Fork**.
4.  Select your account.
5.  Click **Create fork**.

Your fork URL will look like:

``` text
https://github.com/YOUR-USERNAME/AI-Project-Gallery.git
```

Replace `YOUR-USERNAME` with your actual GitHub username.

## Step 4: Set your fork as origin and the original repository as upstream

If you already cloned the original repository, run:

``` bash
git remote set-url origin https://github.com/YOUR-USERNAME/AI-Project-Gallery.git
git remote add upstream https://github.com/KalyanM45/AI-Project-Gallery.git
git remote -v
```

Expected remote meanings:

-   `origin` → your fork
-   `upstream` → original repository

If `upstream` is already configured, do not add it again. Check with
`git remote -v`.

## Step 5: Create the feature branch

If you created `GIT-EXAM` in Step 2, you can use that branch. Otherwise:

``` bash
git switch -c GIT-EXAM
```

Verify:

``` bash
git branch
```

## Step 6: Add or change code

1.  Open the `AI-Project-Gallery` folder in VS Code or another editor.
2.  Inspect the existing project files.
3.  Make a small, meaningful change to an existing file or add a
    suitable file.
4.  Save the change.

The practical sheet does not specify a particular file or code change,
so choose a suitable change based on the repository contents.

Check the changes:

``` bash
git status
```

## Step 7: Commit and push your feature branch

``` bash
git add .
git commit -m "Added changes in GIT-EXAM branch"
git push -u origin GIT-EXAM
```

**Expected result:** The `GIT-EXAM` branch appears in your fork on
GitHub.

## Step 8: Create a Pull Request

1.  Open your fork on GitHub.
2.  Select the `GIT-EXAM` branch.
3.  Click **Contribute** or **Compare & pull request**.
4.  Check the target and source carefully:
    -   **Base repository:** `KalyanM45/AI-Project-Gallery` (original
        repository)
    -   **Base branch:** The original repository's default branch (check
        GitHub; it may be `main` or `master`).
    -   **Head repository:** Your fork.
    -   **Compare branch:** `GIT-EXAM`.
5.  Enter a title and description.
6.  Click **Create pull request**.

**Expected result:** A Pull Request is created for the original
repository owner to review.

## Step 9: Update from upstream and resolve conflicts

A conflict may occur if the original repository changes the same part of
a file that you changed. First fetch the latest original repository
changes:

``` bash
git fetch upstream
```

Check the original repository's default branch name. If it is `main`:

``` bash
git merge upstream/main
```

If it is `master`:

``` bash
git merge upstream/master
```

If Git reports a conflict:

1.  Open the conflicted file in your editor.
2.  Find conflict markers:
    -   `<<<<<<<`
    -   `=======`
    -   `>>>>>>>`
3.  Edit the file to keep the correct combined content.
4.  Remove all conflict markers.
5.  Save the file.

Then stage, commit and push the resolution:

``` bash
git add .
git commit -m "Resolved merge conflicts"
git push origin GIT-EXAM
```

The existing Pull Request updates automatically after you push new
commits to the same branch.

**Note:** A conflict will not necessarily happen by itself. Do not claim
that you resolved a conflict unless you actually demonstrate one. Review
feedback can be handled by editing the requested file, committing the
changes, and pushing again to `GIT-EXAM`.

------------------------------------------------------------------------

## Useful Git commands

  -----------------------------------------------------------------------
  Command                             Purpose
  ----------------------------------- -----------------------------------
  `git clone URL`                     Clone a repository

  `git remote -v`                     Display configured remotes

  `git switch -c GIT-EXAM`            Create and switch to a branch

  `git branch`                        List local branches

  `git status`                        Show working-tree changes

  `git add .`                         Stage changes

  `git commit -m "message"`           Save staged changes in a commit

  `git push -u origin GIT-EXAM`       Push branch to your fork

  `git fetch upstream`                Fetch changes from original
                                      repository

  `git merge upstream/main`           Merge original `main` into current
                                      branch
  -----------------------------------------------------------------------

## Common errors and solutions

### 1. `Permission denied` or `Write access to repository not granted`

You are trying to push to a repository where you do not have write
access. Fork the repository and push to your fork (`origin`).

### 2. `remote upstream already exists`

The upstream remote is already configured. Run:

``` bash
git remote -v
```

Do not add it again.

### 3. `src refspec GIT-EXAM does not match any`

Check the branch name and ensure you have committed changes:

``` bash
git branch
git status
```

### 4. Authentication failed

Sign in to GitHub using the authentication method supported by your Git
installation. Do not put your password or access token directly into
shared command examples.

### 5. Merge conflict

Edit the conflicted files, remove conflict markers, then run `git add`,
`git commit`, and `git push`.

------------------------------------------------------------------------

## Viva questions and answers

**1. What is Git?**\
Git is a distributed version control system used to track changes in
source code.

**2. What is GitHub?**\
GitHub is an online platform for hosting Git repositories and
collaborating on projects.

**3. What is cloning?**\
Cloning downloads a remote repository to a local computer.

**4. What is forking?**\
Forking creates a copy of another user's repository under your GitHub
account.

**5. Why do we fork a repository?**\
We fork when we want to contribute to a project but do not have
permission to push directly to its original repository.

**6. What is a feature branch?**\
A separate branch used to develop a feature or make changes
independently.

**7. What is a Pull Request?**\
A request to review and merge proposed changes into another branch or
repository.

**8. What is a merge conflict?**\
A merge conflict occurs when Git cannot automatically combine changes,
for example when the same lines are changed differently.

**9. What is the difference between origin and upstream?**\
`origin` usually points to your fork. `upstream` points to the original
repository.

**10. Can we create a local branch after cloning a public repository?**\
Yes. Local branch creation is normally allowed. Pushing to the original
repository may be restricted by its permissions.

**11. What happens after the owner accepts a Pull Request?**\
The proposed changes are merged into the target branch, subject to the
repository's review and merge rules.

**12. How do you update a Pull Request after review feedback?**\
Make the requested changes on the same feature branch, commit them, and
push the branch again. The existing Pull Request updates automatically.

------------------------------------------------------------------------

## Practical checklist

-   [ ] Clone the original AI-Project-Gallery repository.
-   [ ] Demonstrate the push permission restriction, if your account
    does not have write access.
-   [ ] Fork the repository on GitHub.
-   [ ] Set `origin` to your fork and `upstream` to the original
    repository.
-   [ ] Create the `GIT-EXAM` feature branch.
-   [ ] Add or change code.
-   [ ] Commit and push the changes to your fork.
-   [ ] Create a Pull Request to the original repository.
-   [ ] Explain how to resolve conflicts and incorporate review
    feedback.

## Final workflow

**Clone → Fork → Create feature branch → Make changes → Commit → Push to
fork → Create Pull Request → Review → Resolve conflicts/feedback**
