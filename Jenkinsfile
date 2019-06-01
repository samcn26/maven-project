pipeline {
    agent any

    tools {
      maven 'localMaven'
    }

    stages {
        stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success{
                    echo 'Now Archiving'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to Staging') {
            steps {
                timeout (time: 5, unit: 'DAYS') {
                    input message: 'Approve Production deployment?'
                }

                build job: 'deploy-to-staging-pipeline'
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo 'Deployment failed'
                }
            }
        }
    }
}