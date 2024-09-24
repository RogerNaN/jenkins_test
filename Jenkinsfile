pipeline {
    agent any

    stages {
        stage('Run Tests in Docker') {
            agent {
                docker {
                    image 'python:3.10'  
                    args '-v /var/run/docker.sock:/var/run/docker.sock'  // 共享 Docker 容器
                }
            }
            steps {
                // 安裝 pytest 並運行測試
                sh 'pip install pytest'
                sh 'pytest test_sample.py'
            }
        }
    }

    post {
        always {
            // 保存測試結果
            junit '**/test-reports/*.xml'
        }
    }
}
