pipeline {
    agent any

    stages {
        stage('Install dependencies') {
            steps {
                // 安裝 pytest
                sh 'pip install pytest'
            }
        }

        stage('Run Tests') {
            steps {
                // 執行 pytest 來運行測試
                sh 'pytest test_sample.py'
            }
        }
    }

    post {
        always {
            // 無論測試是否成功，都會保存測試結果
            junit '**/test-reports/*.xml'
        }
    }
}
