pipeline {
    agent {
        docker {
            image 'python:3.9'
            args '-v /tmp:/tmp' 
        }
    }

    environment {
        VENV_DIR = 'venv'
        PATH = "${VENV_DIR}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/RogerNaN/jenkins_test.git', credentialsId: 'github-credentials-id'
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                    python -m venv ${VENV_DIR}
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
                    source venv/bin/activate
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
