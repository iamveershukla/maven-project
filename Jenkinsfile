pipeline {
    agent any
    
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
                }
            }
        }
 
    }
}