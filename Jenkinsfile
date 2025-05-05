pipeline {
    agent any

    environment {
        VENV_PATH = 'venv'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/falah-23/sast-demo-app.git', branch: 'master'
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                sh 'python3 -m venv ${VENV_PATH}'
                sh './${VENV_PATH}/bin/pip install --upgrade pip'
                sh './${VENV_PATH}/bin/pip install bandit'
            }
        }

        stage('SAST Analysis') {
            steps {
                script {
                    // Jalankan Bandit dan simpan hasil XML + HTML
                    sh './${VENV_PATH}/bin/bandit -r . -f xml -o bandit-output.xml || true'
                    sh './${VENV_PATH}/bin/bandit -r . -f html -o bandit-report.html || true'

                    // Rekam hasil ke tab "Warnings"
                    recordIssues tools: [bandit(pattern: 'bandit-output.xml')]
                }
            }
        }

        stage('Publish Report') {
            steps {
                publishHTML target: [
                    reportDir: '.',
                    reportFiles: 'bandit-report.html',
                    reportName: 'Bandit Security Report'
                ]
            }
        }
    }
}
