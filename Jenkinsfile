pipeline {
    agent any

    tools { 
        maven 'maven' 
        jdk 'jdk1.8.0_144' 
    }
    
    parameters { 
         string(name: 'tomcat_staging', defaultValue: 'http://localhost:8090', description: 'Staging Server')
         string(name: 'tomcat_production', defaultValue: 'http://localhost:9090', description: 'Production Server')
    } 
 
stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'

                    junit 'target/surefire-reports/**/*.xml'
                }
            }
        }

        stage ('Static code analysis'){
            steps {
                build job: 'checkstyle'
            }
        }

        stage ('Deploy to Staging'){
            steps {
                build job: 'deploy-to-staging'
            }
        }

        stage ('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                }

                build job: 'deploy-to-production'
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
 
    }
}