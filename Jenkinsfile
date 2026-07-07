def runCommand(String unixCommand, String windowsCommand) {
    if (isUnix()) {
        sh unixCommand
    } else {
        bat windowsCommand
    }
}

pipeline {
    agent any

    tools {
        nodejs 'Node18'
    }

    stages {
        stage('Instalar dependencias') {
            steps {
                script {
                    runCommand('npm install', 'npm install')
                }
            }
        }

        stage('Ejecutar tests') {
            steps {
                script {
                    runCommand('npm test', 'npm test')
                }
            }
        }

        stage('Construir imagen Docker') {
            steps {
                script {
                    runCommand('docker build -t hola-mundo-node:latest .', 'docker build -t hola-mundo-node:latest .')
                }
            }
        }

        stage('Desplegar contenedor') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                            docker stop hola-mundo-node || true
                            docker rm hola-mundo-node || true
                            docker run -d --name hola-mundo-node -p 3000:3000 hola-mundo-node:latest
                        '''
                    } else {
                        bat '''
                            docker stop hola-mundo-node >NUL 2>&1
                            docker rm hola-mundo-node >NUL 2>&1
                            docker run -d --name hola-mundo-node -p 3000:3000 hola-mundo-node:latest
                        '''
                    }
                }
            }
        }
    }
}
