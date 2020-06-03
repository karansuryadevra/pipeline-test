pipeline{

      agent any

      tools {
          maven 'Maven'
      }
        
        stages{

            stage('Quality Gate Status Check'){                
                steps{
                    script{
                        withSonarQubeEnv('sonar-server'){
                            sh 'mvn sonar:sonar'
                        }
                    timeout(time: 1, unit: 'HOURS'){
                        def qg = waitForQualtiyGate()
                            if (qg.status != 'OK'){
                                error "Pipeline aborted due to Quality gate failure: ${qg.status}"
                            }                                
                    }
                    sh 'mvn clean install'
                        
                 	}

               	}  
            }	
		
        }	       	     	         
}
