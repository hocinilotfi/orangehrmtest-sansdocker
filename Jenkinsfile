pipeline {
    agent any

    environment {
        MAVEN_HOME = '/usr/share/maven'  // Chemin sur l'hôte où Maven est installé
        PATH = "${MAVEN_HOME}/bin:${env.PATH}"  // Ajoute Maven au PATH
    }

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
        }
    }
}
