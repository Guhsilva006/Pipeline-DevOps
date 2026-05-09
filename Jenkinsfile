pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh 'echo "Iniciando Build..."'
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
