pipeline {
    agent any

    tools {
        nodejs 'NodeJs'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner'

                    withSonarQubeEnv('SonarQube') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=Smart-Task-Management-system \
                            -Dsonar.sources=src \
                            -Dsonar.sourceEncoding=UTF-8
                        """
                    }
                }
            }
        }
       stage('Quality Gate') {
           steps {
               timeout(time: 5, unit: 'MINUTES') {
                   waitForQualityGate abortPipeline: true
            }
        }
    }

        stage('Build Frontend') {
            steps {
                sh 'npm run build'
            }
        }
    }

    post {
        success {
            echo 'Frontend pipeline completed successfully.'
        }

        failure {
            echo 'Frontend pipeline failed. Check the stage logs.'
        }
    }
}
