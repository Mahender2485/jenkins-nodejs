pipeline {
    agent any

    tools {
        nodejs 'nodejs-24-1-0' // This name must match the one configured in "Global Tool Configuration"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/Mahender2485/jenkins-nodejs.git'  // Change this to your GitHub repo URL
            }
        }

        stage('Slack Approval') {
            steps {
                script {
                    slackSend(
                        channel: '#jenkin-pipeline',
                        message: "*Approval Needed:* Please approve the deployment job in Jenkins UI."
                    )
                    def userInput = input(
                        id: 'ApproveDeployment', message: 'Approve Deployment?',
                        parameters: [choice(choices: 'Yes\nNo', description: 'Do you approve?', name: 'approval')]
                    )
                    if (userInput != 'Yes') {
                        error "Deployment not approved!"
                    }
                }
            }
        }

        stage('Run index.js') {
            steps {
                sh 'node index.js'  // Runs the Node.js script
            }
        }
    }
    post {
        success {
            slackSend channel: '#jenkin-pipeline', message: "✅ *Build succeeded*: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
        }
        failure {
            slackSend channel: '#jenkin-pipeline', message: "❌ *Build failed*: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
        }
    }
}