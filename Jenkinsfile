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

