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
                sh './$VENV_PATH/bin/gunicorn -b 0.0.0.0:5000 app:app'
            }
        }
    }
    post {
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
