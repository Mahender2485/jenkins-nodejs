# Jenkins Pipeline for Node.js

This repository contains a Jenkins pipeline script that automates the process of cloning a Git repository and executing a Node.js script.

## Prerequisites

Before using this pipeline, ensure that you have the following installed:
- Jenkins (with appropriate permissions to run pipelines)
- Git
- Node.js (installed via Jenkins Global Tool Configuration)

## Installation & Setup

### Step 1: Install Node.js via Jenkins Plugin and Tools

#### Installing NodeJS Plugin
1. Open **Jenkins Dashboard**.
2. Navigate to **Manage Jenkins** > **Manage Plugins**.
3. Click on the **Available** tab and search for **NodeJS**.
4. Select **NodeJS Plugin** and click **Install without restart**.
5. Wait for the installation to complete.

#### Configuring Node.js as a Tool
1. Go to **Manage Jenkins** > **Global Tool Configuration**.
2. Scroll down to **NodeJS** section.
3. Click **Add NodeJS**.
4. Provide a name (e.g., `NodeJS`).
5. Select **Install automatically** to download and install Node.js.
6. Choose the desired **Node.js version**.
7. Check **Global npm packages to install** (optional) and specify any global npm packages if needed.
8. Click **Save**.

#### Verifying Installation
To verify if Node.js is installed correctly, you can add the following stage in your pipeline:

```groovy
pipeline {
    agent any

    tools {
        nodejs 'NodeJS'  // This must match the configured name in Global Tool Configuration
    }

    stages {
        stage('Check Node.js Version') {
            steps {
                sh 'node -v'  // Verifies Node.js installation
                sh 'npm -v'   // Verifies npm installation
            }
        }
    }
}
```

### Step 2: Create a Jenkins Pipeline Job

1. Open Jenkins and create a new **Pipeline** job.
2. In the **Pipeline** section, copy and paste the following script:

   ```groovy
   pipeline {
       agent any

       tools {
           nodejs 'NodeJS' // This name must match the one configured in "Global Tool Configuration"
       }

       stages {
           stage('Clone Repository') {
               steps {
                   git branch: 'main', 
                       url: 'https://github.com/tarasowski/jenkins-nodejs.git'  // Change this to your GitHub repo URL
               }
           }

           stage('Run index.js') {
               steps {
                   sh 'node index.js'  // Runs the Node.js script
               }
           }
       }
   }
   ```

3. Save and run the pipeline.

## Explanation of the Pipeline

This pipeline consists of three main stages:

1. **Check Node.js Version**: 
   - Ensures that Node.js and npm are installed correctly.

2. **Clone Repository**: 
   - Pulls the latest code from the specified GitHub repository.
   - Uses the `git` step to clone the repository from the `main` branch.

3. **Run index.js**: 
   - Executes a Node.js script (`index.js`) using the `sh 'node index.js'` command.
   - Requires Node.js to be installed and configured in Jenkins.

## Conclusion

This pipeline simplifies the process of automating Node.js application execution in a Jenkins environment. Make sure your Jenkins instance has the necessary plugins and configurations to run the pipeline successfully.
