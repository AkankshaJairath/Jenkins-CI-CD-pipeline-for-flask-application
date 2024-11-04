pipeline {
    agent { label 'AkankshaNode' }

    environment {
        VENV_PATH = "venv" // Path to the virtual environment
    }
    stages {
        stage('Build') {
            steps {
                // Install dependencies in a virtual environment
                sh 'python3 -m venv $VENV_PATH'
                sh './$VENV_PATH/bin/pip install -r requirements.txt'
            }
        }
        stage('Test') {
            steps {
                // Run unit tests
                sh './$VENV_PATH/bin/pytest tests/'
            }
        }
        stage('Deploy') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                // Deploy to staging (example: simple gunicorn command)
                script {
                    sh """
                    sudo cp myflaskapp.service /etc/systemd/system/myflaskapp.service
                    sudo systemctl start myflaskapp
                    sudo systemctl enable myflaskapp
                    sudo systemctl status myflaskapp
                    """
                }
            }
        }
    }
    post {
        success {
            emailext (
                to: 'akanshajairath123@gmail.com',
                subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """
                     Good news! The build for '${env.JOB_NAME} [${env.BUILD_NUMBER}]' was successful.
                     
                     Project: ${env.JOB_NAME}
                     Build Number: ${env.BUILD_NUMBER}
                     URL: ${env.BUILD_URL}
                """,
                mimeType: 'text/html'
            )
        }
        failure {
           emailext (
                to: 'akanshajairath123@gmail.com',
                subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """
                     Unfortunately, the build for '${env.JOB_NAME} [${env.BUILD_NUMBER}]' failed.
                     
                     Project: ${env.JOB_NAME}
                     Build Number: ${env.BUILD_NUMBER}
                     URL: ${env.BUILD_URL}
                """,
                mimeType: 'text/html'
            )
        }
        aborted{
          emailext (
                to: 'akanshajairath123@gmail.com',
                subject: "Abort: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """
                     Unfortunately, the build for '${env.JOB_NAME} [${env.BUILD_NUMBER}]' Aborted.
                     
                     Project: ${env.JOB_NAME}
                     Build Number: ${env.BUILD_NUMBER}
                     URL: ${env.BUILD_URL}
                """,
                mimeType: 'text/html'
            )
            
    }
}
}
