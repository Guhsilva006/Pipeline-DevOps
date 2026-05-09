# Pipeline-DevOps

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "Iniciando build..."'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Executando testes..."'
                sh 'uname -a'
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo "Deploy realizado com sucesso!"'
            }
        }
    }
}
