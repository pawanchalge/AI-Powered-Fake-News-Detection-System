# Experiment 8: Jenkins – Create a CI/CD Pipeline (Screenshot Guide)

> **Beginner guide:** Follow the steps in order. Each screenshot is a visual reference showing the screen or control you should look for. Jenkins and GitHub interfaces can change between versions, so a few labels may look slightly different on your computer.
>
> **Screenshot note:** These are genuine reference screenshots from the official Jenkins and GitHub documentation. They are not screenshots captured from your own laptop. Where an exact screen is not shown in the official documentation (for example, the full SCM configuration form or Poll SCM schedule), the instructions explain exactly what to select rather than pretending an illustrative image is a real screenshot.

## Aim

To install Jenkins, connect it with a GitHub repository, create a Pipeline job, configure a Jenkinsfile and build trigger, and verify that Jenkins automatically runs a build after code changes are pushed to GitHub.

## Tasks from the practical PDF

1. Create a repository on GitHub and upload project files.
2. Install and launch Jenkins (`http://localhost:8080`).
3. Install required plugins (Git, GitHub, Pipeline).
4. Create a new Pipeline job in Jenkins.
5. Configure Pipeline script from SCM and add the GitHub repository URL.
6. Add a Jenkinsfile to the repository.
7. Configure a build trigger (Poll SCM or GitHub Webhook).
8. Push code changes to GitHub.
9. Verify that Jenkins automatically triggers the build.

---

## Before starting: understand the workflow

![Jenkins Pipeline job creation reference](https://www.jenkins.io/doc/book/resources/pipeline/new-item-creation.png)

**Workflow:** GitHub stores your project → Jenkins checks GitHub → Jenkins reads `Jenkinsfile` → Jenkins runs the stages → Jenkins displays SUCCESS or FAILURE.

### Important words

| Word | Simple meaning |
|---|---|
| Jenkins | Automation server |
| CI | Continuous Integration |
| CD | Continuous Delivery or Continuous Deployment |
| Pipeline | A series of automated steps |
| Jenkinsfile | File containing instructions for the pipeline |
| Plugin | Add-on that gives Jenkins extra features |
| SCM | Source Code Management |
| Build | Running the pipeline for a project |
| Poll SCM | Jenkins checks the repository at intervals |
| Webhook | GitHub sends Jenkins a notification when an event happens |

---

# Part A — Install and open Jenkins

## Step 1: Install Java

Jenkins needs a supported Java runtime. The current Jenkins Windows installation guide specifies **Java 21 or later**. Check the support policy for the Jenkins release you download.

1. Open the Java download page: https://www.oracle.com/java/technologies/downloads/
2. Download a supported JDK for Windows.
3. Run the installer and complete installation.
4. Open Command Prompt.
5. Run:

```bash
java -version
```

**Expected result:** Windows displays the installed Java version.

If you see `java is not recognized`, Java may not be installed correctly or its `bin` folder may not be on PATH.

*There is no Jenkins-specific Java installation screenshot here; the command output depends on the Java distribution installed on your computer.*

## Step 2: Download Jenkins

1. Open the official Jenkins download page: https://www.jenkins.io/download/
2. Choose the **LTS** release.
3. Download the **Windows installer (.msi)**.
4. Open the downloaded file.

## Step 3: Start the Windows setup wizard

When the installer opens, this is the first setup screen.

![Jenkins Windows installer – setup wizard](https://www.jenkins.io/doc/book/resources/tutorials/windows-installation-setup-wizard.png)

**What to do:** Click **Next**.

## Step 4: Choose the installation folder

![Jenkins Windows installer – destination folder](https://www.jenkins.io/doc/book/resources/tutorials/windows-installation-destination.png)

**What to do:**
1. Keep the default folder unless you have a reason to change it.
2. Click **Next**.

## Step 5: Configure the Windows service account

![Jenkins Windows installer – service logon credentials](https://www.jenkins.io/doc/book/resources/tutorials/windows-jenkins-logon-credentials.png)

This screen decides which Windows account runs the Jenkins service.

**For a beginner's local practical:** follow the installer guidance and use the option appropriate for your computer/account. Do not enter a password you do not know. If you choose a specific Windows user, that account needs permission to log on as a service.

## Step 6: Select the Jenkins port

![Jenkins Windows installer – port selection](https://www.jenkins.io/doc/book/resources/tutorials/windows-select-port.png)

1. Keep port `8080` if it is available.
2. Click **Test Port**.
3. If the port is free, the installer should indicate that it is available.
4. Click **Next**.

**What is a port?** A port is a numbered communication endpoint. Jenkins uses it to provide its web page.

If port 8080 is already in use, choose a free port and remember it. You must then open Jenkins using that port instead of 8080.

## Step 7: Select the Java home directory

![Jenkins Windows installer – Java home directory](https://www.jenkins.io/doc/book/resources/tutorials/windows-select-jdk-installation.png)

1. Confirm the selected Java installation is supported by the Jenkins release.
2. Click **Next**.

## Step 8: Select the custom setup options

![Jenkins Windows installer – custom setup](https://www.jenkins.io/doc/book/resources/tutorials/windows-select-features-to-install.png)

Review the options and continue with the default setup unless your instructor has specified otherwise.

## Step 9: Start installation

![Jenkins Windows installer – ready to install](https://www.jenkins.io/doc/book/resources/tutorials/windows-ready-to-install.png)

Click **Install**.

![Jenkins Windows installer – installation progress](https://www.jenkins.io/doc/book/resources/tutorials/windows-installation-progress.png)

Wait until the installation finishes.

## Step 10: Finish installation and check the service

![Jenkins Windows installer – finish](https://www.jenkins.io/doc/book/resources/tutorials/windows-finish-install.png)

Click **Finish**.

Jenkins is installed as a Windows service. You can check this using Windows Services:

1. Press `Windows + R`.
2. Type `services.msc`.
3. Press Enter.
4. Find **Jenkins** in the list.
5. Check that its status is **Running**.

![Jenkins Windows service in Services app](https://www.jenkins.io/doc/book/resources/tutorials/windows-jenkins-service.png)

## Step 11: Open Jenkins in your browser

Open Chrome and enter:

```text
http://localhost:8080
```

If you selected a different port during installation, use that port instead.

## Step 12: Unlock Jenkins

The first time Jenkins starts, it asks for an initial administrator password.

![Jenkins Unlock Jenkins page](https://www.jenkins.io/doc/book/resources/tutorials/setup-jenkins-02-unlock-jenkins-page.png)

To find the password in the default Windows installation:

1. Open File Explorer.
2. Go to:

```text
C:\Program Files\Jenkins\secrets
```

3. Find the file named `initialAdminPassword`.

![File Explorer showing initialAdminPassword](https://www.jenkins.io/doc/book/resources/tutorials/windows-initial-password-location.png)

4. Open the file using Notepad.
5. Copy the password in the file.
6. Paste it into the Jenkins Unlock page.
7. Click **Continue**.

![Notepad showing the initialAdminPassword file](https://www.jenkins.io/doc/book/resources/tutorials/windows-initial-password-file.png)

**Security:** The password visible in an official documentation screenshot is only an example. Use the password generated on your own computer. Never share your administrator password.

If the default folder does not exist, check the installation directory you selected or the Jenkins service log.

## Step 13: Install suggested plugins

After unlocking, Jenkins shows its setup wizard.

Choose **Install suggested plugins**. Jenkins will install a recommended set of plugins. This is the easiest first-time option.

![Jenkins Plugin Manager – available plugins screen](https://www.jenkins.io/doc/book/resources/blueocean/intro/blueocean-plugins-filtered.png)

The screenshot above shows the plugin manager layout. It is an example screen; the plugin list and appearance may differ in your version.

Wait for the installation process to complete.

## Step 14: Create your first administrator user

When the **Create First Admin User** page appears:

1. Enter a username.
2. Enter a password and confirm it.
3. Enter the requested name/email details.
4. Click **Save and Finish**.
5. When Jenkins is ready, click **Start using Jenkins**.

**Expected result:** The Jenkins dashboard opens.

## Step 15: Verify the required plugins

Your practical specifically names these plugins:

- **Git**
- **GitHub**
- **Pipeline**

To verify them:

1. Open **Manage Jenkins**.
2. Open **Plugins** (some versions show **Manage Plugins**).
3. Select the **Installed** tab.
4. Search for each plugin name.
5. If a required plugin is missing, search in the **Available** tab and install it.

The Jenkins documentation describes the Plugin Manager under **Manage Jenkins → Plugins**. The exact tab names and layout can vary with Jenkins versions.

---

# Part B — Create the GitHub project

## Step 16: Create a new GitHub repository

1. Open https://github.com/ and sign in.
2. Click the **+** menu near the top-right.
3. Choose **New repository**.

![GitHub plus menu showing New repository](https://docs.github.com/assets/cb-29762/images/help/repository/repo-create-global-nav-update.png)

4. Enter repository name:

```text
Jenkins-Practical
```

5. Choose **Public** for this beginner exercise.
6. Select **Add a README file**.
7. Click **Create repository**.

![GitHub repository name field](https://docs.github.com/assets/cb-89988/images/help/repository/create-repository-name.png)

**Expected result:** Your new repository page opens.

## Step 17: Create the Python file `app.py`

1. On the repository page, click **Add file**.
2. Select **Create new file**.

![GitHub Add file control](https://docs.github.com/assets/cb-60263/images/help/repository/add-file-buttons.png)

3. In the filename field, type:

```text
app.py
```

4. Paste this code:

```python
print("Hello from Jenkins practical!")
print("This project is connected to GitHub.")
```

5. Click **Commit changes**.

## Step 18: Create the `Jenkinsfile`

Repeat **Add file → Create new file**.

Enter the exact filename:

```text
Jenkinsfile
```

Paste:

```groovy
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

Click **Commit changes**.

![GitHub file editor and Preview tab](https://docs.github.com/assets/cb-84712/images/help/repository/new-file-preview.png)

**Important:** The screenshot is a general GitHub file editor example. Your editor should show the filename `Jenkinsfile` and the pipeline code above.

### Understand the Jenkinsfile

| Jenkinsfile part | Meaning |
|---|---|
| `pipeline { }` | Starts the pipeline definition |
| `agent any` | Run on any available agent |
| `stages { }` | Holds the pipeline stages |
| `stage('Build')` | First stage |
| `stage('Test')` | Second stage |
| `stage('Finish')` | Third stage |
| `echo` | Prints a message in the build log |

This is a demonstration pipeline. It prints messages; it does not actually execute or test the Python file.

Your repository should now contain at least:

```text
Jenkins-Practical/
├── README.md
├── app.py
└── Jenkinsfile
```

---

# Part C — Create and configure a Jenkins Pipeline job

## Step 19: Open the Jenkins dashboard

Go to:

```text
http://localhost:8080
```

Sign in if Jenkins asks you to.

## Step 20: Click New Item

On the Jenkins dashboard, find **New Item** in the left-side menu.

![Jenkins dashboard left menu with New Item](https://www.jenkins.io/doc/book/resources/pipeline/classic-ui-new-item.png)

Click **New Item**.

## Step 21: Name the job and select Pipeline

1. Enter this job name:

```text
My-First-Pipeline
```

2. Select **Pipeline** from the item types.
3. Click **OK**.

![Jenkins New Item page with Pipeline selected](https://www.jenkins.io/doc/book/resources/pipeline/new-item-creation.png)

**Expected result:** Jenkins opens the configuration page for your new Pipeline job.

## Step 22: Find the Pipeline configuration section

1. On the job configuration page, scroll down or click the **Pipeline** tab in the left-side menu.
2. Find the **Definition** dropdown.

The following official screenshot shows where the Pipeline tab and Definition dropdown are located. It shows the `Pipeline script` option as an example. For this practical, you must change it to **Pipeline script from SCM**.

![Jenkins configuration page showing Pipeline tab and Definition dropdown](https://www.jenkins.io/doc/book/resources/pipeline/example-pipeline-in-classic-ui.png)

## Step 23: Select Pipeline script from SCM

Set these values:

| Field | What to select or enter |
|---|---|
| Definition | `Pipeline script from SCM` |
| SCM | `Git` |
| Repository URL | Your GitHub repository URL |
| Credentials | `- none -` for a public repository |
| Branches to build | `*/main` (or `*/master` if that is your default branch) |
| Script Path | `Jenkinsfile` |

Example repository URL:

```text
https://github.com/YOUR-USERNAME/Jenkins-Practical.git
```

Replace `YOUR-USERNAME` with your actual GitHub username.

**Important:** The full SCM form can look different depending on the installed Jenkins and Git plugin versions. The official Jenkins guide shows the same configuration sequence: choose **Pipeline script from SCM**, choose **Git**, complete the repository details and set the Script Path to the location of the Jenkinsfile.

Official visual and written guide: https://www.jenkins.io/doc/book/pipeline/getting-started/#in-scm

Click **Save**.

### What does SCM mean here?

SCM means Source Code Management. Instead of typing the pipeline script directly into Jenkins, Jenkins downloads the `Jenkinsfile` from your GitHub repository.

---

# Part D — Configure automatic build trigger

## Step 24: Choose Poll SCM

For a beginner's local practical, use **Poll SCM**. It does not require GitHub to reach your laptop from the internet.

1. Open the Pipeline job.
2. Click **Configure**.
3. Find **Build Triggers**.
4. Tick **Poll SCM**.
5. Enter:

```text
H/2 * * * *
```

6. Click **Save**.

This schedule asks Jenkins to check approximately every two minutes. `H` lets Jenkins distribute the checking time rather than always using the same exact minute.

**Visual location:** Poll SCM is in the job's Configure page under the Build Triggers section. The exact layout can vary between Jenkins versions. The official Jenkins Pipeline documentation explains SCM polling and how it triggers builds when repository changes are detected: https://www.jenkins.io/doc/book/pipeline/getting-started/#in-scm

## Step 25: Understand the alternative — GitHub Webhook

A webhook is a notification sent by GitHub to Jenkins when a selected event, such as a push, occurs.

General setup:

1. Jenkins must be reachable from GitHub over the network.
2. Open the GitHub repository.
3. Select **Settings → Webhooks → Add webhook**.
4. Enter the publicly reachable Jenkins webhook endpoint, commonly ending with:

```text
/github-webhook/
```

5. Choose JSON as the content type.
6. Select the push event.
7. Save the webhook.
8. Configure the corresponding GitHub trigger in Jenkins.

**Very important:** `http://localhost:8080` is reachable only from your own computer. GitHub cannot send a webhook to your laptop's localhost address. For this local practical, use **Poll SCM** unless your Jenkins instance has a properly configured public endpoint.

---

# Part E — Run the build and verify it

## Step 26: Click Build Now

1. Open the Jenkins dashboard.
2. Click `My-First-Pipeline`.
3. Find **Build Now** on the left-side menu.
4. Click it.

![Jenkins Pipeline job menu showing Build Now](https://www.jenkins.io/doc/book/resources/pipeline/classic-ui-left-column-on-item-build-now.png)

## Step 27: Open Build History

After clicking Build Now, look for **Build History**. A build number such as `#1` should appear.

Click the build number.

## Step 28: Open Console Output

Click **Console Output** to see what Jenkins executed.

![Jenkins successful Console Output example](https://www.jenkins.io/doc/book/resources/pipeline/hello-world-console-output.png)

For this practical, output should contain messages similar to:

```text
Building the project...
Testing the project...
Jenkins pipeline completed successfully!
Finished: SUCCESS
```

<badge color="success">Expected result: SUCCESS</badge>

**Meaning:** Jenkins successfully retrieved and executed the stages from the Jenkinsfile.

---

# Part F — Test automatic triggering

## Step 29: Make a new change in GitHub

1. Open your GitHub repository.
2. Click `app.py`.
3. Click the pencil icon to edit.
4. Add this line:

```python
print("This is my second change.")
```

5. Click **Commit changes**.

## Step 30: Wait for Poll SCM

1. Return to Jenkins.
2. Wait for the next scheduled poll (approximately two minutes with the schedule above).
3. Refresh the Jenkins job page.
4. Check Build History for another build number, such as `#2`.
5. Open the new build and check Console Output.

**Expected result:** Jenkins detects the new GitHub commit and starts another build. The build should show `SUCCESS` if the configuration is correct.

---

# Troubleshooting

| Problem | What to check |
|---|---|
| `localhost:8080` does not open | Check that the Jenkins Windows service is running and port 8080 is available |
| `java is not recognized` | Install a supported Java version and configure PATH |
| Jenkins service does not start | Check Java compatibility and the service configuration |
| Repository URL error | Verify the GitHub URL |
| Authentication failed | Configure credentials if the repository is private |
| `Jenkinsfile` not found | Check exact spelling and confirm it is in the repository root |
| Branch not found | Check whether the default branch is `main` or `master` |
| Build does not start automatically | Check Poll SCM schedule, job configuration and whether the commit was pushed |
| Build fails | Open that build's Console Output and inspect the first error |

---

# Viva questions and answers

**1. What is Jenkins?**  
Jenkins is an open-source automation server used for Continuous Integration and Continuous Delivery.

**2. Why do we use Jenkins?**  
To automate software development tasks such as building, testing and deploying applications.

**3. What is CI/CD?**  
CI means Continuous Integration. CD means Continuous Delivery or Continuous Deployment.

**4. What is a Jenkins Pipeline?**  
A sequence of automated stages executed by Jenkins.

**5. What is a Jenkinsfile?**  
A text file containing pipeline instructions, usually stored in the project's Git repository.

**6. What is a plugin?**  
A plugin adds extra functionality to Jenkins.

**7. What is SCM?**  
SCM stands for Source Code Management.

**8. What is Poll SCM?**  
It periodically checks a source repository for new changes.

**9. What is a GitHub Webhook?**  
It sends an event notification from GitHub to Jenkins when a configured event occurs.

**10. What is Build Now?**  
It manually starts a Jenkins build.

**11. What is Console Output?**  
It displays the logs and messages generated during a build.

**12. What does `Finished: SUCCESS` mean?**  
The Jenkins build completed successfully.

**13. What is the purpose of port 8080?**  
It is the default port commonly used by the Jenkins web interface.

**14. What stages are used in our Jenkinsfile?**  
Build, Test and Finish.

**15. What is the purpose of `agent any`?**  
It allows Jenkins to run the pipeline on any available agent.

---

# Practical checklist

- [ ] Install supported Java.
- [ ] Download and install Jenkins.
- [ ] Open Jenkins at `http://localhost:8080`.
- [ ] Unlock Jenkins using `initialAdminPassword`.
- [ ] Install/check Git, GitHub and Pipeline plugins.
- [ ] Create a GitHub repository.
- [ ] Upload `app.py` and `Jenkinsfile`.
- [ ] Create a Pipeline job in Jenkins.
- [ ] Configure Pipeline script from SCM.
- [ ] Enter repository URL, branch and Jenkinsfile path.
- [ ] Configure Poll SCM or GitHub Webhook.
- [ ] Run Build Now and check Console Output.
- [ ] Push a new change to GitHub.
- [ ] Verify that Jenkins triggers another build.

## Conclusion

In this experiment, Jenkins was installed and connected to a GitHub repository. A Pipeline job was created using Pipeline script from SCM, a Jenkinsfile was added, and a build trigger was configured. After a new change was pushed to GitHub, Jenkins was checked to verify that it automatically started another build.

## Official screenshot and learning references

- Jenkins Windows installation: https://www.jenkins.io/doc/book/installing/windows/
- Jenkins Pipeline and Pipeline from SCM: https://www.jenkins.io/doc/book/pipeline/getting-started/
- Jenkins plugin management: https://www.jenkins.io/doc/book/managing/plugins/
- GitHub create repository: https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository
- GitHub create new file: https://docs.github.com/en/repositories/working-with-files/managing-files/creating-new-files
