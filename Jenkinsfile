pipeline {
    agent any

    stages {
        // 準備 Python 環境
        stage('Prepare Environment') {
            steps {
                script {
                    docker.image('python:3.10').inside {
                        echo 'Preparing Python virtual environment...'
                        sh 'python -m venv venv' // 創建虛擬環境
                    }
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    docker.image('python:3.10').inside {
                        echo 'Installing dependencies...'
                        sh '. venv/bin/activate && pip install pytest' // 安裝 pytest
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    docker.image('python:3.10').inside {
                        echo 'Running tests...'
                        sh '. venv/bin/activate && pytest --junitxml=results.xml' // 測試並生成測試報告
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Archiving artifacts and test results...'
            junit 'results.xml' 
        }
        success {
            echo 'All tests passed!'
        }
        failure {
            echo 'Some tests failed.'
        }
    }
}
