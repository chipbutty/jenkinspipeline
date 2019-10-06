pipeline {
    agent any
    
    tools {
        maven 'localMaven'
    }
 //   parameters { 
 //        string(name: 'tomcat_dev', defaultValue: 'http://localhost:8090', description: 'Staging Server')
 //        string(name: 'tomcat_prod', defaultValue: 'http://localhost:8081', description: 'Production Server')
 //  } 

 //   triggers {
 //        pollSCM('* * * * *') // Polling Source Control
 //    }


stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {  
                        build job: 'Deploy-to-staging'
                       // sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        build job: 'Deploy-to-prod'
                        //sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        } 
    }
}