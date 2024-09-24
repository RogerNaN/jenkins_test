pipeline {
    agent any

    stages {
        stage('Run Tests in Docker') {
            steps {
                script {
                    docker.image('python:3.10').inside {
                        sh 'python -m venv venv'
                        sh '. venv/bin/activate && pip install pytest'
                        sh '. venv/bin/activate && pytest --junitxml=results.xml'
                    }
                }
            }
        }
    }

    post {
        always {
            junit 'results.xml'
        }
    }
}
