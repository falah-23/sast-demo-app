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
                sh 'python3 -m venv venv'
                sh '. venv/bin/activate && pip install bandit'
            }
        }
        stage('SAST Analysis') {
            steps {
                sh '''
                    . venv/bin/activate
                    bandit -f html -o bandit-report.html -r . || true
                '''
            }
        }
        stage('Publish Bandit Report') {
            steps {
                publishHTML(target: [
                    reportDir: '.', 
                    reportFiles: 'bandit-report.html', 
                    reportName: 'Bandit Analysis'
                ])
            }
        }
    }
}
