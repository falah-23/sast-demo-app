pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/falah-23/sast-demo-app.git', branch: 'master'
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    // Membuat dan mengaktifkan virtual environment
                    sh 'python3 -m venv venv'
                    sh '. venv/bin/activate && pip install bandit'
                }
            }
        }
        stage('SAST Analysis') {
            steps {
                script {
                    // Jalankan Bandit di dalam virtual environment
                    sh '. venv/bin/activate && bandit -f xml -o bandit-output.xml -r . || true'
                    recordIssues tools: [bandit(pattern: 'bandit-output.xml')]
                }
            }
        }
    }
}
