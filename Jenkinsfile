pipeline {
    agent any

    environment {
        PROJECT_KEY = 'python-jenkins-pipeline'  // must match project key in SonarQube
        SONAR_SCANNER = '/opt/homebrew/Cellar/sonar-scanner/7.1.0.4889/bin/sonar-scanner'  // Adjust if sonar-scanner is installed elsewhere
    }

    stages {
        stage('Build Python Code') {
            steps {
                echo "Installing dependencies and building the code..."

                // Show which python is available
                sh 'echo "Using Python binary:"'
                sh 'which python || echo "python not found"'
                sh 'which python3 || echo "python3 not found"'
                sh 'python3 --version || echo "Python3 not available"'

                // Install dependencies using python3
                sh 'python3 -m pip install -r requirements.txt'

                // Compile all Python files using python3
                sh 'python3 -m compileall .'
            }
        }

        stage('SonarQube Scan') {
            steps {
                echo "Running SonarQube scanner..."
                withSonarQubeEnv('sonar-jenkins') {
                    // Use full path to sonar-scanner to avoid PATH issues
                    sh "${env.SONAR_SCANNER}"
                }
            }
        }

        stage('Generate Sonar Report') {
            steps {
                script {
                    echo "View your report at: http://localhost:9000/dashboard?id=${env.PROJECT_KEY}"
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully. SonarQube report is available."
        }
        failure {
            echo "❌ Pipeline failed. Check logs and SonarQube for more details."
        }
    }
}
