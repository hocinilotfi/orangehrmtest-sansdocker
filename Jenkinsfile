pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: '', url: 'https://github.com/hocinilotfi/orangehrmtest-sansdocker.git'
            }
        }
        stage('Build Maven Project') {
            steps {
                script {
                    try {
                        sh 'mvn clean install'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }
        stage('Run Selenium Tests') {
            steps {
                script {
                    try {
                        // Assurez-vous que le serveur Selenium est en cours d'exécution
                        // Par exemple, utilisez une commande ou un script pour démarrer Selenium si nécessaire
                        sh 'mvn test -Dcucumber.plugin="json:target/cucumber-report/cucumber-report.json"'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }
    }
    post {
        always {
            cucumber 'target/cucumber-report/cucumber-report.json'
            // Nettoyage, si nécessaire
        }
    }
}
