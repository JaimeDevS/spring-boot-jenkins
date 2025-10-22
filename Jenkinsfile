pipeline {
    agent any
    
    tools {
        jdk 'JDK21'
        maven 'Maven-3.8'
    }
    
    environment {
        APP_NAME = 'spring-boot-jenkins'
        VERSION = '1.0.0'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo '📦 Obtendo código do repositório...'
                git branch: 'main', 
                    url: 'https://github.com/JaimeDevS/spring-boot-jenkins'
            }
        }
        
        stage('Build') {
            steps {
                echo '🔨 Compilando aplicação...'
                bat 'mvn clean compile'
            }
        }
        
        stage('Test') {
            steps {
                echo '🧪 Executando testes...'
                bat 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Package') {
            steps {
                echo '📦 Gerando JAR...'
                bat 'mvn clean package -DskipTests'
            }
        }
        
        stage('Deploy') {
            steps {
                echo '🚀 Implantando aplicação...'
                script {
                    // Para ambiente local Windows
                    bat 'taskkill /F /IM java.exe 2>nul || echo "Nenhuma instância Java rodando"'
                    bat 'java -jar target/spring-boot-jenkins-1.0.0.jar &'
                    echo '✅ Aplicação implantada e rodando!'
                }
            }
        }
    }
    
    post {
        always {
            echo '🏁 Pipeline finalizado!'
            cleanWs() // Limpa workspace
        }
        success {
            echo '🎉 Pipeline executado com sucesso!'
        }
        failure {
            echo '❌ Pipeline falhou!'
            // Aqui você pode configurar notificações
        }
    }
}