pipeline {
    agent any
    
    tools {
        maven 'Maven'
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

	stage('Deploy To Staging'){
		
		steps{

		build job: 'DeployToStaging'	
		}
           }
	
	
	stage('Deploy To Production'){
	
	steps{
		timeout(time:5,unit:'DAYS'){
		input message: 'Approve Production Deployment?'
		}
	
		build job:'DeployToProduction'
	     }	
		post{
			success{
				echo 'Code depoloyed A-OK'
			}
			failure{
				echo 'Deployment Flopped'
			}
			
		
		}	
		
	}
	
	
      }
}
     
