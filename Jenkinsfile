pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '35.174.106.85', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.87.158.132', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

	tools {
	maven 'localMaven' //This was missing in original jenkinsfile

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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "pscp -i C:/Users/shres/jenkins/NorthVirgina.ppk C:/Program Files (x86)/Jenkins/workspace/FullyAutomated/webapp/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "pscp -i C:/Users/shres/jenkins/NorthVirgina.ppk C:/Program Files (x86)/Jenkins/workspace/FullyAutomated/webapp/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
