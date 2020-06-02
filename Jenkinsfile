pipeline{

      agent none

      tools {
          maven 'Maven'
      }
        
        stages{

            stage('Build'){                
                steps{
                    script{
                        sh 'mvn clean install'
                 	}

               	}  
            }	
		
        }	       	     	         
}
