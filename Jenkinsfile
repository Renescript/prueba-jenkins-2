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
 
        stage('Dependencias') { 
            steps { 
                script { 
                    try { 
                        echo "⚙️ Instalando dependencias..." 
                        sh 'npm install' 
                    } catch (Exception e) { 
                        error("❌ Error en la etapa de dependencias") 
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
