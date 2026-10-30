# Experiment 7: GitHub Desktop -- Fork, Feature Branch and Pull Request

## Aim

To use GitHub Desktop to clone a GitHub repository, fork it, create a
feature branch, make changes, push the branch to your fork, submit a
Pull Request, and understand how to resolve merge conflicts and
incorporate review feedback.

## Repository given in the practical

Original repository:

https://github.com/KalyanM45/Spam-Email-Detection.git

## Tasks mentioned in the practical PDF

1.  Clone the repository from the given path.
2.  Demonstrate that branch creation is disallowed for this repository.
3.  Fork this AI project repository.
4.  Create a feature branch named `GIT-EXAM` to implement changes.
5.  Change/add your code in the branch.
6.  Submit a Pull Request for review.
7.  During the review process, resolve merge conflicts and incorporate
    feedback to ensure smooth integration and collaboration.

------------------------------------------------------------------------

## Important concept

You can normally create a local branch in a repository you cloned.
However, without write permission, you generally cannot push your branch
to the original repository. Forking creates your own copy on GitHub,
where you can push your changes.

-   **Clone:** Downloads a remote repository to your computer.
-   **Fork:** Creates a copy of another user's repository under your
    GitHub account.
-   **Branch:** A separate line of development.
-   **Pull Request (PR):** A request asking the original repository
    owner to review and merge your changes.

## Step 1: Open GitHub Desktop

1.  Install and open **GitHub Desktop**.
2.  Sign in to your GitHub account.
3.  Make sure the correct GitHub account is shown in the application.

## Step 2: Clone the original repository

1.  Open the repository page:\
    https://github.com/KalyanM45/Spam-Email-Detection

2.  Click the green **Code** button.

3.  Copy the HTTPS repository URL.

4.  In GitHub Desktop, select **File → Clone repository**.

5.  Open the **URL** tab.

6.  Paste:

    `https://github.com/KalyanM45/Spam-Email-Detection.git`

7.  Choose a local path where you want to save the project.

8.  Click **Clone**.

**Expected result:** The repository is downloaded to your computer and
appears in GitHub Desktop.

## Step 3: Demonstrate the permission restriction

The practical asks you to demonstrate that branch creation is
disallowed.

**Technical clarification:** GitHub Desktop can normally create a local
branch even when you do not have permission to write to the original
repository. The restriction is generally on publishing/pushing the
branch to the original repository.

To demonstrate this, you can try publishing a test branch to the
original repository only if the application offers that action. If you
do not have write permission, GitHub rejects the push. Do not mistake
local branch creation for remote write permission.

## Step 4: Fork the repository

1.  Open the original repository in your web browser:
    https://github.com/KalyanM45/Spam-Email-Detection
2.  Sign in to GitHub.
3.  Click **Fork** near the top-right of the repository page.
4.  Select your GitHub account.
5.  Click **Create fork**.

**Expected result:** A copy of the repository is created under your
GitHub account.

## Step 5: Clone your fork in GitHub Desktop

1.  Open your fork on GitHub.

2.  Click **Code** and copy its HTTPS URL. It will look like:

    `https://github.com/YOUR-USERNAME/Spam-Email-Detection.git`

3.  In GitHub Desktop, select **File → Clone repository → URL**.

4.  Paste your fork URL.

5.  Choose a local path and click **Clone**.

Replace `YOUR-USERNAME` with your actual GitHub username.

**Expected result:** GitHub Desktop now has your fork cloned locally.
This is the copy where you have permission to push changes.

## Step 6: Create the feature branch

1.  In GitHub Desktop, select your cloned fork repository.

2.  On the top bar, click **Current Branch**.

3.  Click **New Branch**.

4.  Enter the branch name:

    `GIT-EXAM`

5.  Select the branch you want to create it from (normally the
    repository's default branch).

6.  Click **Create Branch**.

**Expected result:** The Current Branch selector displays `GIT-EXAM`.

## Step 7: Change or add code

1.  Click **Repository → Show in Explorer** (or use the equivalent
    option to open the local repository folder).
2.  Open the project in your code editor.
3.  Inspect the existing project files.
4.  Make a small, meaningful change to an existing file or add a
    suitable file.
5.  Save your changes.
6.  Return to GitHub Desktop.

The practical PDF does not specify which file or exact code change is
required. Choose a suitable change after inspecting the repository.

**Expected result:** The changed file appears in the **Changes** tab in
GitHub Desktop.

## Step 8: Commit the changes

1.  In GitHub Desktop, open the **Changes** tab.

2.  Review the files listed.

3.  Select the files you want to include.

4.  Enter a commit summary, for example:

    `Added changes in GIT-EXAM branch`

5.  Click **Commit to GIT-EXAM**.

**Expected result:** The changes are saved as a local commit on the
`GIT-EXAM` branch.

## Step 9: Push the branch to your fork

1.  Click **Publish branch** or **Push origin** at the top of GitHub
    Desktop.
2.  Wait for the push to finish.
3.  Open your fork on GitHub and check that the `GIT-EXAM` branch is
    visible.

**Expected result:** Your changes are uploaded to your fork on GitHub.

## Step 10: Create a Pull Request

1.  In GitHub Desktop, select **Branch → Create Pull Request**, if
    available. This opens GitHub in your browser.
2.  Alternatively, open your fork on GitHub and select the `GIT-EXAM`
    branch.
3.  Click **Contribute** or **Compare & pull request**.
4.  Check the Pull Request direction carefully:
    -   **Base repository:** `KalyanM45/Spam-Email-Detection` (original
        repository).
    -   **Base branch:** The original repository's default branch.
    -   **Head repository:** Your fork.
    -   **Compare branch:** `GIT-EXAM`.
5.  Enter a title and description.
6.  Click **Create pull request**.

**Expected result:** The Pull Request is submitted to the original
repository for review.

## Step 11: Resolve merge conflicts and incorporate feedback

A merge conflict can occur if the original repository and your feature
branch contain incompatible changes to the same part of a file.

1.  If the Pull Request shows a conflict, open the repository locally in
    GitHub Desktop.
2.  Fetch the latest changes from the original repository if the
    original has been configured as an upstream remote. GitHub Desktop's
    available menu options may vary.
3.  Use the merge/update options to bring the target branch's latest
    changes into your feature branch.
4.  Open conflicted files in your editor.
5.  Review both versions and manually create the correct final content.
6.  Remove conflict markers such as `<<<<<<<`, `=======`, and `>>>>>>>`.
7.  Save the resolved files.
8.  Return to GitHub Desktop, review the changes, and commit the
    conflict resolution.
9.  Push the updated `GIT-EXAM` branch to your fork.

Your existing Pull Request updates when you push additional commits to
the same branch.

**Review feedback:** If the reviewer requests changes, edit the relevant
files on `GIT-EXAM`, commit the changes, and push the branch again.

**Note:** A conflict will not necessarily occur automatically. Do not
claim that you resolved a conflict unless you actually demonstrate one.
The exact GitHub Desktop menu labels can vary by version and remote
configuration.

------------------------------------------------------------------------

## Common errors and solutions

### 1. Push or publish is rejected

You may be trying to push to the original repository without write
permission. Fork the repository and push to your own fork.

### 2. The wrong repository is selected

Check the repository name in the GitHub Desktop top bar. Select your
fork's local clone before making and publishing changes.

### 3. The branch is not visible on GitHub

Make sure you clicked **Publish branch** or **Push origin** and that the
push completed successfully.

### 4. No files appear under Changes

Confirm that you saved your edits inside the cloned repository folder,
not in a different folder.

### 5. Merge conflict

Open the conflicted files, combine the correct changes, remove conflict
markers, save, commit, and push.

------------------------------------------------------------------------

## Viva questions and answers

**1. What is GitHub Desktop?**\
GitHub Desktop is a graphical application that helps users work with Git
repositories without entering every Git command manually.

**2. What is cloning?**\
Cloning downloads a remote repository to a local computer.

**3. What is forking?**\
Forking creates a copy of another user's repository under your GitHub
account.

**4. Why do we fork a repository?**\
We fork a repository when we want to contribute but do not have
permission to push directly to the original repository.

**5. What is a branch?**\
A branch is a separate line of development used to make changes
independently.

**6. What is the feature branch name in this experiment?**\
`GIT-EXAM`.

**7. What is a commit?**\
A commit records a set of changes in the repository's history.

**8. What is Push?**\
Push uploads local commits to a remote repository.

**9. What is a Pull Request?**\
A Pull Request asks the project owner to review and merge proposed
changes.

**10. What is a merge conflict?**\
A merge conflict occurs when Git cannot automatically combine changes.

**11. What is the difference between Fork and Clone?**\
Fork creates a copy on GitHub under your account. Clone downloads a
repository to your computer.

**12. Can a user create a local branch without write permission to the
original repository?**\
Yes. Local branch creation is normally allowed. Pushing to the original
repository is what may be restricted.

------------------------------------------------------------------------

## Practical checklist

-   [ ] Open GitHub Desktop and sign in.
-   [ ] Clone the original Spam-Email-Detection repository.
-   [ ] Demonstrate the push permission restriction, if your account
    lacks write access.
-   [ ] Fork the repository on GitHub.
-   [ ] Clone your fork in GitHub Desktop.
-   [ ] Create the `GIT-EXAM` branch.
-   [ ] Add or change code.
-   [ ] Commit the changes.
-   [ ] Publish/push the branch to your fork.
-   [ ] Create a Pull Request to the original repository.
-   [ ] Explain how to resolve conflicts and incorporate review
    feedback.

## Final workflow

**GitHub Desktop → Clone original → Fork → Clone your fork → Create
`GIT-EXAM` → Make changes → Commit → Publish branch → Pull Request →
Review and resolve conflicts**
