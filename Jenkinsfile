pipeline {

    agent any

    environment {
        APP_NAME = "Pipeline-DevOps"
        APP_VERSION = "1.0.${BUILD_NUMBER}"
        LOG_FILE = "logs/history.log"
    }

    stages {

        stage('Sistema e Info') {
            steps {

                echo 'Coletando informações do servidor...'

                sh '''
                    echo "===== SISTEMA ====="

                    echo "Usuário atual: $(whoami)"

                    echo "Kernel do Linux: $(uname -r)"

                    echo "Hostname: $(hostname)"

                    echo "Tempo de atividade (Uptime):"
                    uptime

                    echo "Uso de memória:"
                    free -h

                    echo "Uso de disco:"
                    df -h
                '''
            }
        }

        stage('Preparação de Ambiente') {
            steps {

                echo 'Criando diretórios e arquivos temporários...'

                sh '''
                    mkdir -p build_output
                    mkdir -p logs

                    echo "Iniciando build em $(date)" \
                    > build_output/log_compilacao.txt

                    echo "APP_NAME=${APP_NAME}" \
                    > build_output/config.env

                    echo "APP_VERSION=${APP_VERSION}" \
                    >> build_output/config.env
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

                        echo "Erro: Arquivo não encontrado"

                        exit 1
                    fi
                '''
            }
        }

        stage('Relatório de Recursos') {
            steps {

                echo 'Analisando consumo de disco e arquivos gerados...'

                sh '''
                    echo "===== RELATÓRIO ====="

                    echo "Tamanho da pasta de build:"
                    du -sh build_output

                    echo "Listagem detalhada:"
                    ls -lh build_output
                '''
            }
        }

        stage('Arquivar Artefatos') {
            steps {

                echo 'Salvando artefatos do build...'

                archiveArtifacts artifacts: 'build_output/*', fingerprint: true
            }
        }
    }

    post {

        success {

            echo 'Pipeline executada com sucesso!'

            withCredentials([usernamePassword(
                credentialsId: 'github-token',
                usernameVariable: 'GIT_USER',
                passwordVariable: 'GIT_TOKEN'
            )]) {

                sh '''
                    echo "[SUCCESS] Build #${BUILD_NUMBER} - $(date)" \
                    >> ${LOG_FILE}

                    git config --global user.email "jenkins@devops.com"

                    git config --global user.name "Jenkins CI"

                    git add ${LOG_FILE}

                    git commit -m "[skip ci] Log SUCCESS build ${BUILD_NUMBER}" || true

                    git push https://${GIT_USER}:${GIT_TOKEN}@github.com/Guhsilva006/Pipeline-DevOps.git HEAD:master
                '''
            }
        }

        failure {

            echo 'Pipeline falhou!'

            withCredentials([usernamePassword(
                credentialsId: 'github-token',
                usernameVariable: 'GIT_USER',
                passwordVariable: 'GIT_TOKEN'
            )]) {

                sh '''
                    mkdir -p logs

                    echo "[FAILED] Build #${BUILD_NUMBER} - $(date)" \
                    >> ${LOG_FILE}

                    git config --global user.email "jenkins@devops.com"

                    git config --global user.name "Jenkins CI"

                    git add ${LOG_FILE}

                    git commit -m "[skip ci] Log FAILED build ${BUILD_NUMBER}" || true

                    git push https://${GIT_USER}:${GIT_TOKEN}@github.com/Guhsilva006/Pipeline-DevOps.git HEAD:master
                '''
            }
        }

        always {

            echo 'Finalizando pipeline...'

            sh '''
                echo "========== RESUMO FINAL =========="

                echo "Build Number: ${BUILD_NUMBER}"

                echo "Projeto: ${APP_NAME}"

                echo "Versão: ${APP_VERSION}"

                echo "Data:"
                date
            '''
        }
    }
}
