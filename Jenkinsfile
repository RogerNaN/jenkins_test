pipeline {
    agent {
        docker {
            image 'python:3.9'
        }
    }

    environment {
        PYTHON_VERSION = '3.9'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/RogerNaN/jenkins_test.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    pip install --upgrade pip
                    if [ -f requirements.txt ]; then
                        pip install -r requirements.txt
                    fi
                    pip install pytest
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    mkdir -p reports
                    pytest --junitxml=reports/test-results.xml
                '''
            }
            post {
                always {
                    junit 'reports/test-results.xml'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline 成功完成！所有測試通過。'
        }
        failure {
            echo 'Pipeline 失敗。請檢查錯誤和測試報告。'
        }
        always {
            cleanWs()
        }
    }
}
