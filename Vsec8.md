# Experiment 8: Jenkins -- Continuous Integration and Continuous Delivery Pipeline

## Aim

To set up Jenkins, connect it with a GitHub repository, create a
Pipeline job, configure a Jenkinsfile and build trigger, and verify that
Jenkins automatically runs a build after code changes are pushed to
GitHub.

## Tasks mentioned in the practical PDF

1.  Create a repository on GitHub and upload project files.
2.  Install and launch Jenkins (`http://localhost:8080`).
3.  Install required plugins: Git, GitHub and Pipeline.
4.  Create a new Pipeline job in Jenkins.
5.  Configure Pipeline script from SCM and add the GitHub repository
    URL.
6.  Add a Jenkinsfile to the repository.
7.  Configure a build trigger (Poll SCM or GitHub Webhook).
8.  Push code changes to GitHub.
9.  Verify that Jenkins automatically triggers the build.

------------------------------------------------------------------------

## 1. What is Jenkins?

Jenkins is an automation server used to automate software development
tasks such as building, testing and deploying applications.

For this practical, Jenkins will connect to GitHub, retrieve the project
and execute the instructions written in the `Jenkinsfile`.

### Important terms

-   **CI (Continuous Integration):** Frequently integrating code changes
    and checking them automatically.
-   **CD (Continuous Delivery/Deployment):** Preparing or automatically
    releasing an application after successful checks.
-   **Pipeline:** A sequence of automated steps executed by Jenkins.
-   **Jenkinsfile:** A text file containing the pipeline instructions.
-   **Plugin:** An extension that adds functionality to Jenkins.
-   **SCM:** Source Code Management; a system for managing source-code
    changes.

## 2. Workflow of this practical

1.  Create a GitHub repository.
2.  Upload project files and a `Jenkinsfile`.
3.  Install and open Jenkins.
4.  Install/check the required plugins.
5.  Create a Pipeline job.
6.  Connect the job to the GitHub repository using Pipeline script from
    SCM.
7.  Configure Poll SCM or GitHub Webhook.
8.  Push a new change to GitHub.
9.  Verify the Jenkins build.

------------------------------------------------------------------------

## Step 1: Install Java

Jenkins requires a supported Java version to run. Check the current
Jenkins Java support policy before installing. For current Jenkins
releases, use Java 21 or later where required by the selected release.

1.  Download a supported Java JDK from:
    https://www.oracle.com/java/technologies/downloads/
2.  Install it using the Windows installer.
3.  Open Command Prompt.
4.  Run:

``` bash
java -version
```

**Expected result:** The command displays the installed Java version.

If Windows reports that `java` is not recognized, check the installation
and PATH configuration.

## Step 2: Download and install Jenkins
## Step 2: Download and install Jenkins

**Screenshot – Jenkins Windows installer setup wizard:**

![Jenkins Windows setup wizard](https://www.jenkins.io/doc/book/resources/tutorials/windows-installation-setup-wizard.png)

**Screenshot – selecting the Jenkins port (default 8080):**

![Jenkins port selection](https://www.jenkins.io/doc/book/resources/tutorials/windows-select-port.png)


1.  Open the official Jenkins download page:
    https://www.jenkins.io/download/
2.  Choose the **LTS** release.
3.  Download the **Windows installer (.msi)**.
4.  Run the installer.
5.  Follow the setup wizard.
6.  Keep the default port `8080` if it is available.
7.  Complete the installation.

**Expected result:** Jenkins is installed as a Windows service.

If port 8080 is already in use, select an available port and use that
port in the browser.

## Step 3: Launch Jenkins and unlock it
## Step 3: Launch Jenkins and unlock it

**Screenshot – Unlock Jenkins page:**

![Unlock Jenkins page](https://www.jenkins.io/doc/book/resources/tutorials/setup-jenkins-02-unlock-jenkins-page.png)


1.  Open Chrome or another browser.
2.  Visit:

``` text
http://localhost:8080
```

3.  Wait for the **Unlock Jenkins** page.
4.  Open File Explorer and go to the Jenkins secrets directory. For a
    default Windows installation, it is commonly:

``` text
C:\Program Files\Jenkins\secrets
```

5.  Open `initialAdminPassword`.
6.  Copy the password and paste it into the Unlock Jenkins page.
7.  Click **Continue**.

**Important:** Keep the administrator password private. The exact
secrets directory can differ if you selected a custom installation
location.

## Step 4: Install the required plugins
## Step 4: Install the required plugins

**Screenshot – Jenkins Plugin Manager (example of the Available Plugins screen):**

![Jenkins Plugin Manager](https://www.jenkins.io/doc/book/resources/blueocean/intro/blueocean-plugins-filtered.png)

*The screenshot illustrates the plugin manager layout. Search for the exact required plugins: Git, GitHub and Pipeline.*


A Jenkins plugin adds extra functionality.

The practical requires these plugins:

  Plugin     Purpose
  ---------- ---------------------------------------------
  Git        Allows Jenkins to retrieve Git repositories
  GitHub     Provides GitHub integration
  Pipeline   Supports Jenkins Pipeline jobs

During the initial setup, you can select **Install suggested plugins**.
After setup, verify the required plugins:

1.  Open **Manage Jenkins**.
2.  Open **Plugins** or **Manage Plugins** (the label varies by Jenkins
    version).
3.  Open the **Installed** tab.
4.  Search for `Git`, `GitHub` and `Pipeline`.
5.  If a required plugin is missing, find it in the **Available** tab
    and install it.

Complete the initial administrator account setup if prompted, then open
the Jenkins dashboard.

## Step 5: Create a GitHub repository and upload project files

1.  Open https://github.com/ and sign in.
2.  Click **+ → New repository**.
3.  Enter repository name:

``` text
Jenkins-Practical
```

4.  Choose **Public** for this beginner demonstration.
5.  Select **Add a README file**.
6.  Click **Create repository**.

### Create `app.py`

In the GitHub repository:

1.  Click **Add file → Create new file**.
2.  Enter the filename `app.py`.
3.  Paste:

``` python
print("Hello from Jenkins practical!")
print("This project is connected to GitHub.")
```

4.  Click **Commit changes**.

### Create `Jenkinsfile`

1.  Click **Add file → Create new file**.
2.  Enter the exact filename:

``` text
Jenkinsfile
```

3.  Paste this pipeline:

``` groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing the project...'
            }
        }

        stage('Finish') {
            steps {
                echo 'Jenkins pipeline completed successfully!'
            }
        }
    }
}
```

4.  Click **Commit changes**.

**What does this Jenkinsfile do?**

-   `pipeline` defines the pipeline.
-   `agent any` allows Jenkins to run it on any available agent.
-   `stages` groups the pipeline tasks.
-   `stage('Build')`, `stage('Test')` and `stage('Finish')` define the
    stages.
-   `echo` prints a message in the build log.

This simple example demonstrates a pipeline. It does not actually
compile or test the Python application.

## Step 6: Create a Pipeline job in Jenkins
## Step 6: Create a Pipeline job in Jenkins

**Screenshot – Jenkins dashboard with New Item:**

![Jenkins dashboard New Item](https://www.jenkins.io/doc/book/resources/pipeline/classic-ui-new-item.png)

**Screenshot – naming a job and selecting Pipeline:**

![Jenkins New Item Pipeline selection](https://www.jenkins.io/doc/book/resources/pipeline/new-item-creation.png)


1.  Open `http://localhost:8080`.
2.  On the dashboard, click **New Item**.
3.  Enter the job name:

``` text
My-First-Pipeline
```

4.  Select **Pipeline**.
5.  Click **OK**.

**Expected result:** The Pipeline job is created and its configuration
page opens.

## Step 7: Configure Pipeline script from SCM
## Step 7: Configure Pipeline script from SCM

**Visual reference – Pipeline job configuration:** The exact SCM form varies slightly by Jenkins version. Use the field-by-field instructions below: Definition = Pipeline script from SCM, SCM = Git, Repository URL = your GitHub URL, Branch = `*/main` (or `*/master`), Script Path = `Jenkinsfile`.

Official Jenkins guide: https://www.jenkins.io/doc/book/pipeline/getting-started/#defining-a-pipeline-in-scm


SCM means Source Code Management. This setting tells Jenkins to retrieve
the pipeline script from your GitHub repository.

1.  Open the job.
2.  Click **Configure**.
3.  Scroll down to the **Pipeline** section.
4.  Under **Definition**, select:

``` text
Pipeline script from SCM
```

5.  Under **SCM**, select **Git**.
6.  In **Repository URL**, paste your repository URL:

``` text
https://github.com/YOUR-USERNAME/Jenkins-Practical.git
```

Replace `YOUR-USERNAME` with your GitHub username.

7.  For a public repository, credentials can normally remain as
    `- none -`. For a private repository, configure suitable
    credentials.
8.  Under **Branches to build**, enter:

``` text
*/main
```

If your repository's default branch is `master`, use `*/master`.

9.  Under **Script Path**, enter:

``` text
Jenkinsfile
```

10. Click **Save**.

### Meaning of the settings

  -----------------------------------------------------------------------
  Setting                             Meaning
  ----------------------------------- -----------------------------------
  Pipeline script from SCM            Jenkins reads the pipeline
                                      definition from source control

  Git                                 Git is used to retrieve the
                                      repository

  Repository URL                      Address of the GitHub repository

  Branch                              Branch Jenkins should build

  Script Path                         Location of the Jenkinsfile in the
                                      repository
  -----------------------------------------------------------------------

## Step 8: Configure the build trigger
## Step 8: Configure the build trigger

**Visual reference:** The Poll SCM checkbox and schedule field are located in the job's **Configure → Build Triggers** section. Their position and labels may differ slightly by Jenkins version. The official Jenkins Pipeline guide explains SCM polling: https://www.jenkins.io/doc/book/pipeline/getting-started/


The practical allows either **Poll SCM** or **GitHub Webhook**.

### Option A: Poll SCM

Poll SCM makes Jenkins check the repository periodically for new
commits. It is a convenient choice when Jenkins is running locally on
your laptop.

1.  Open the Pipeline job.
2.  Click **Configure**.
3.  Find **Build Triggers**.
4.  Select **Poll SCM**.
5.  Enter this schedule:

``` text
H/2 * * * *
```

This schedules checks approximately every two minutes. The `H` symbol
lets Jenkins distribute the checking time rather than always using the
same exact minute.

6.  Click **Save**.

### Option B: GitHub Webhook

A webhook is a notification GitHub sends to Jenkins when a selected
event, such as a push, occurs.

General setup:

1.  Jenkins must be reachable by GitHub over the network.
2.  Open your GitHub repository.
3.  Go to **Settings → Webhooks → Add webhook**.
4.  Enter the publicly reachable Jenkins webhook endpoint, commonly
    ending in:

``` text
/github-webhook/
```

5.  Select JSON as the content type.
6.  Select the push event.
7.  Save the webhook.
8.  Configure the corresponding GitHub trigger in Jenkins.

**Important:** `http://localhost:8080` is accessible only from your own
computer. GitHub cannot send a webhook to your laptop's localhost
address. For a local classroom practical, Poll SCM is generally simpler
unless a public Jenkins endpoint has been configured.

## Step 9: Run the first build
## Step 9: Run the first build

**Screenshot – Build Now option in the Pipeline job:**

![Jenkins Build Now](https://www.jenkins.io/doc/book/resources/pipeline/classic-ui-left-column-on-item-build-now.png)

**Screenshot – example of successful Console Output:**

![Jenkins successful Console Output](https://www.jenkins.io/doc/book/resources/pipeline/hello-world-console-output.png)


1.  Open the Jenkins dashboard.
2.  Click `My-First-Pipeline`.
3.  Click **Build Now**.
4.  Look at **Build History**.
5.  Click the new build number.
6.  Click **Console Output**.

Expected messages include:

``` text
Building the project...
Testing the project...
Jenkins pipeline completed successfully!
Finished: SUCCESS
```

**Expected result:** The build completes successfully and Jenkins
displays `SUCCESS`.

## Step 10: Push a new change and verify automatic triggering

1.  Open your GitHub repository.
2.  Open `app.py`.
3.  Click the pencil icon to edit the file.
4.  Add another line:

``` python
print("This is my second change.")
```

5.  Click **Commit changes**.
6.  Return to Jenkins.
7.  Wait for the next Poll SCM check (approximately two minutes with the
    schedule above).
8.  Refresh the job page.
9.  Check Build History for a new build.
10. Open its Console Output and verify the result.

**Expected result:** Jenkins detects the new commit and starts another
build. The build should complete successfully if the configuration is
correct.

------------------------------------------------------------------------

## Common errors and solutions

  -----------------------------------------------------------------------
  Error/problem                       Possible solution
  ----------------------------------- -----------------------------------
  Jenkins page does not open          Check whether the Jenkins Windows
                                      service is running and port 8080 is
                                      available

  `java is not recognized`            Install a supported Java version
                                      and check PATH

  Jenkins service does not start      Check Java compatibility and
                                      service configuration

  Repository URL error                Verify the GitHub repository URL

  Authentication failed               Configure credentials if the
                                      repository is private

  `Jenkinsfile` not found             Check exact filename and ensure it
                                      is in the repository root

  Branch not found                    Verify whether the branch is `main`
                                      or `master`

  Automatic build does not start      Check Poll SCM schedule, job
                                      configuration and whether the new
                                      commit was pushed

  Build fails                         Open the build's Console Output and
                                      inspect the error
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## Viva questions and answers

**1. What is Jenkins?**\
Jenkins is an open-source automation server used for Continuous
Integration and Continuous Delivery.

**2. Why do we use Jenkins?**\
To automate software development tasks such as building, testing and
deploying applications.

**3. What is CI/CD?**\
CI means Continuous Integration. CD means Continuous Delivery or
Continuous Deployment.

**4. What is a Jenkins Pipeline?**\
A sequence of automated stages executed by Jenkins.

**5. What is a Jenkinsfile?**\
A text file that contains pipeline instructions, usually stored in the
project's Git repository.

**6. What is a plugin?**\
A plugin adds additional functionality to Jenkins.

**7. What is SCM?**\
SCM stands for Source Code Management.

**8. What is Poll SCM?**\
It periodically checks a source repository for new changes.

**9. What is a GitHub Webhook?**\
It sends an event notification from GitHub to Jenkins when a configured
event occurs.

**10. What is Build Now?**\
It manually starts a Jenkins build.

**11. What is Console Output?**\
It displays the logs and messages generated during a build.

**12. What does `Finished: SUCCESS` mean?**\
The Jenkins build completed successfully.

**13. What is the purpose of port 8080?**\
It is the default port commonly used by the Jenkins web interface.

**14. What stages are used in our Jenkinsfile?**\
Build, Test and Finish.

**15. What is the purpose of `agent any`?**\
It allows Jenkins to run the pipeline on any available agent.

------------------------------------------------------------------------

## Practical checklist

-   [ ] Install Java.
-   [ ] Download and install Jenkins.
-   [ ] Open Jenkins at `http://localhost:8080`.
-   [ ] Unlock Jenkins using `initialAdminPassword`.
-   [ ] Install/check Git, GitHub and Pipeline plugins.
-   [ ] Create a GitHub repository.
-   [ ] Upload `app.py` and `Jenkinsfile`.
-   [ ] Create a Pipeline job in Jenkins.
-   [ ] Configure Pipeline script from SCM.
-   [ ] Enter repository URL, branch and Jenkinsfile path.
-   [ ] Configure Poll SCM or GitHub Webhook.
-   [ ] Run Build Now and check Console Output.
-   [ ] Push a new change to GitHub.
-   [ ] Verify that Jenkins triggers another build.

## Conclusion

In this experiment, Jenkins was installed and connected to a GitHub
repository. A Pipeline job was created using Pipeline script from SCM, a
Jenkinsfile was added, and a build trigger was configured. After pushing
a new change to GitHub, Jenkins was checked to verify that it
automatically started a new build.

## Screenshot sources and notes

The embedded Jenkins interface screenshots above are reference images from the official Jenkins documentation. They show example screens and may look slightly different from the version installed on your computer. GitHub repository creation and editing screens are not included as fabricated screenshots; follow the written GitHub steps and compare them with the current GitHub interface.

- Jenkins Windows installation screenshots: https://www.jenkins.io/doc/book/installing/windows/
- Jenkins Pipeline screenshots: https://www.jenkins.io/doc/book/pipeline/getting-started/
- Jenkins plugin manager documentation: https://www.jenkins.io/doc/book/managing/plugins/

## Official references

-   Jenkins installation on Windows:
    https://www.jenkins.io/doc/book/installing/windows/
-   Jenkins Pipeline documentation:
    https://www.jenkins.io/doc/book/pipeline/
-   Jenkins Java support policy:
    https://www.jenkins.io/doc/book/platform-information/support-policy-java/
