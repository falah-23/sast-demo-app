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
                    // Membuat virtual environment dan menginstal Bandit
                    sh 'python3 -m venv venv'
                    sh '. venv/bin/activate && pip install bandit'
                }
            }
        }
        stage('SAST Analysis') {
            steps {
                script {
                    // Jalankan Bandit dan output ke file XML
                    sh '. venv/bin/activate && bandit -f xml -o bandit-output.xml -r . || true'
                    
                    // Menampilkan laporan Bandit dalam Jenkins
                    publishHTML(target: [
                        reportName: 'Bandit Analysis',
                        reportDir: 'bandit-output.xml',
                        reportFiles: 'bandit-output.xml',
                        keepAll: true
                    ])
                }
            }
        }
    }
}
