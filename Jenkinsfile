pipeline {
    agent any
  
    tools {
        nodejs "NodeJS"  // Specify the NodeJS installation configured in Jenkins
    }

    environment {
        GIT_CREDENTIALS_ID = 'your-git-credentials-id'
        JENKINS_URL = 'http://192.168.189.133:8080'
    }

    stages {
        stage('Checkout') {
            steps {
                git(
                    url: 'https://github.com/your-username/your-angular-project.git',
                    credentialsId: env.GIT_CREDENTIALS_ID,
                    branch: 'main'  // Adjust the branch if necessary
                )
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Lint') {
            steps {
                sh 'npm run lint'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build --prod'
            }
        }

        stage('Archive') {
            steps {
                // Archive the build artifacts (e.g., the dist folder)
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }

        stage('Notify Slack') {
            steps {
                script {
                    def artifactPath = "dist/"
                    def artifactURL = "${env.JENKINS_URL}/job/${env.JOB_NAME}/${env.BUILD_NUMBER}/artifact/${artifactPath}"

                    slackSend channel: 'fpl',
                    message: "A new Angular build is available: ---> Result: ${currentBuild.currentResult}, Job: ${env.JOB_NAME}, Build: ${env.BUILD_NUMBER} \n <${artifactURL}|Click here to download>"
                }
            }
        }
    }
}
