pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
    }
    stages {
        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}\\${VIRTUAL_ENV}")) {
                        bat "python -m venv ${VIRTUAL_ENV}"
                    }
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && pip install -r requirements.txt"
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && flake8 app.py"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && set PYTHONPATH=%CD% && pytest"
                }
            }
        }
         stage('Coverage') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && set PYTHONPATH=%CD% && coverage run -m pytest"
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && coverage report"
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && coverage html"  // Optional: Generates HTML report
                }
            }
        }
         stage('Security Scan') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && bandit -r ."
                }
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying application..."
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
