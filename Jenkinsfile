def runCommand(String unixCommand, String windowsCommand) {
    if (isUnix()) {
        sh unixCommand
    } else {
        bat windowsCommand
    }
}

pipeline {
    agent any

    environment {
        npm_config_registry = 'https://registry.npmjs.org/'
        npm_config_audit = 'false'
        npm_config_fund = 'false'
        npm_config_cache = '.npm-cache'
    }

    tools {
        nodejs 'Node25'
    }

    stages {
        stage('Instalar dependencias') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                            export NPM_CONFIG_USERCONFIG=/dev/null
                            export NPM_CONFIG_GLOBALCONFIG=/dev/null
                            rm -rf node_modules .npm-cache
                            npm install -g npm@9.9.4
                            npm install --ignore-scripts --no-audit --no-fund --no-package-lock --progress=false
                        '''
                    } else {
                        bat '''
                            set NPM_CONFIG_USERCONFIG=NUL
                            set NPM_CONFIG_GLOBALCONFIG=NUL
                            if exist node_modules rmdir /s /q node_modules
                            if exist .npm-cache rmdir /s /q .npm-cache
                            npm install -g npm@9.9.4
                            npm install --ignore-scripts --no-audit --no-fund --no-package-lock --progress=false
                        '''
                    }
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
