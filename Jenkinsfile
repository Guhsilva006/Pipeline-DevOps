pipeline {
    agent any

    stages {
        stage('Sistema e Info') {
            steps {
                echo 'Coletando informações do servidor...'
                sh '''
                    echo "Usuário atual: $(whoami)"
                    echo "Kernel do Linux: $(uname -r)"
                    echo "Tempo de atividade (Uptime):"
                    uptime
                '''
            }
        }

        stage('Preparação de Ambiente') {
            steps {
                echo 'Criando diretórios e arquivos temporários...'
                sh '''
                    mkdir -p build_output
                    echo "Iniciando build em $(date)" > build_output/log_compilacao.txt
                    echo "APP_VERSION=1.0.${BUILD_NUMBER}" >> build_output/config.env
                '''
            }
        }

        stage('Validação de Integridade') {
            steps {
                echo 'Verificando se os arquivos foram criados corretamente...'
                sh '''
                    if [ -f build_output/config.env ]; then
                        echo "Arquivo de config encontrado!"
                        cat build_output/config.env
                    else
                        echo "Erro: Arquivo não encontrado" && exit 1
                    fi
                '''
            }
        }

        stage(' Relatório de Recursos') {
            steps {
                echo 'Analisando consumo de disco e arquivos gerados...'
                sh '''
                    echo "Tamanho da pasta de build:"
                    du -sh build_output
                    echo "Listagem detalhada:"
                    ls -lh build_output
                '''
            }
        }
    }

    post {
        always {
            echo 'Limpando rastros da demonstração...'
            // sh 'rm -rf build_output' // Comentei para você poder mostrar os arquivos no final se quiser
        }
        success {
            echo 'Demonstração finalizada com sucesso na VM/Vagrant!'
        }
    }
}
