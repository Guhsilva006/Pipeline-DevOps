pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Buscando código do repositório...'
                // Aqui simula a integração com Git
            }
        }
        
        stage('Build') {
            steps {
                echo 'Compilando o projeto...'
                sh 'echo "Versão 1.0.0" > versao.txt'
            }
        }

        stage('Test') {
            steps {
                echo 'Executando testes unitários...'
                sh 'echo "Testes aprovados!"'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Implantando em ambiente de Staging...'
                sh 'ls -lh'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline finalizado com sucesso!'
        }
    }
}
