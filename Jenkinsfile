pipeline {
 agent any

	parameters{
		string(name: 'Tomcat_Stage, defaultValue:'18.218.26.161',description: 'Staging Server')
		string(name: 'Tomcat_Prod, defaultValue:'18.219.23.187',description: 'Production Server')      
		       }
		       triggers{
			       pollSCM('* * * * *')
		       }
		       
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
                        sh "scp -i /Users/Fayez/Desktop/tomcatDemoKey.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /Users/Fayez/Desktop/tomcatDemoKey.pem  **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}



     
