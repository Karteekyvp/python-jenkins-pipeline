pipeline {
    agent {
        docker {
            image 'python:3.7'
        }
    }

    environment {
        PROJECT_KEY = 'python-jenkins-pipeline'  // must match project key in SonarQube
    }

    stages {
        stage('Build Python Code') {
            steps {
                echo "Installing dependencies and building the code..."
                sh 'pip install -r requirements.txt'
                sh 'python -m compileall .'
            }
        }

        stage('SonarQube Scan') {
            steps {
                echo "Running SonarQube scanner..."
                withSonarQubeEnv('LocalSonarQube') {
                    sh 'sonar-scanner'
                }
            }
        }

        stage('Generate Sonar Report') {
            steps {
                echo "Fetching SonarQube quality gate status and exposing report URL..."
                script {
                    echo "View your report at: http://localhost:9000/dashboard?id=${env.PROJECT_KEY}"
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully. SonarQube report is available."
        }
        failure {
            echo "Pipeline failed. Check logs and SonarQube for more details."
        }
    }
}
