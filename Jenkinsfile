pipeline { 
    agent any 
    environment { 
        NODE_VERSION = '18.16'
    } 
 
    stages { 
        stage('Clonar Repositorio') {
            steps {
                echo "** Clonando repositorio"
                // git 'https://github.com/Renescript/Apoyo-Prueba---Implementacio-n-de-un-pipeline-de-integracio-n-continua.git'
                checkout scm
            }
        }

        stage('Instalar nvm y Node.js') {
            steps {
                script {
                    try {
                        echo "⚙️ Instalando nvm y Node.js..."

                        // Install nvm and Node.js
                        sh '''
                            curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
                            export NVM_DIR="$HOME/.nvm"
                            [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"   # Removed the escape character
                            nvm install ${NODE_VERSION}
                            nvm use ${NODE_VERSION}
                        '''

                        // Now install dependencies with npm
                        sh 'npm install'
                    } catch (Exception e) { 
                        error("❌ Error en la instalación de nvm y Node.js") 
                    } 
                } 
            }
        }
 
        stage('Test') { 
            steps { 
                script { 
                    try { 
                        echo "🧪 Ejecutando pruebas..." 
                        sh 'npm test' 
                    } catch (Exception e) { 
                        error("❌ Error en la etapa de Test") 
                    } 
                } 
            } 
        } 

        stage('Docker') {
            steps {
                script {
                    //def imagen = 'desafio_jenkins:latest'
                    sh "docker build -t desafio_jenkins ."
                    sh "docker run -p 3000:3000 desafio_jenkins"
                }
            }
        } 
    } 
 
    post { 
        success { 
            echo "✅ Pipeline completado con éxito" 
        } 
        failure { 
            echo "❌ El pipeline ha fallado" 
        } 
    } 
}
